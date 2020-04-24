---
title: 使用 Azure Site Recovery 迁移本地 Windows Server 2008 服务器 | Azure
description: 本文将介绍如何使用 Azure Site Recovery 将本地 Windows Server 2008 计算机迁移到 Azure。
author: rockboyfor
manager: digimobile
ms.service: site-recovery
ms.topic: tutorial
origin.date: 09/09/2019
ms.date: 09/30/2019
ms.author: v-yeche
ms.custom: MVC
ms.openlocfilehash: 32b805b342684de83366bc344ea812ab7b9ba560
ms.sourcegitcommit: c1ba5a62f30ac0a3acb337fb77431de6493e6096
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2020
ms.locfileid: "71340882"
---
# <a name="migrate-servers-running-windows-server-2008-to-azure"></a>将运行 Windows Server 2008 的服务器迁移到 Azure

本教程介绍如何使用 Azure Site Recovery，将运行 Windows Server 2008 或 2008 R2 的本地服务器迁移到 Azure。 本教程介绍如何执行下列操作：

> [!div class="checklist"]
> * 准备好要迁移的本地环境
> * 设置目标环境
> * 设置复制策略
> * 启用复制
> * 运行测试迁移，确保一切按预期正常工作
> * 故障转移到 Azure 并完成迁移

限制和已知问题部分列出了在将 Windows Server 2008 计算机迁移到 Azure 时存在的限制，以及可能遇到的已知问题的解决方法。 

<!--Not Available on [Learn more](../migrate/migrate-services-overview.md)-->

## <a name="supported-operating-systems-and-environments"></a>支持的操作系统和环境

|操作系统  | 本地环境  |
|---------|---------|
|Windows Server 2008 SP2 - 32 位和 64 位（IA-32 和 x86-64）<br />- Standard<br />- Enterprise<br />- Datacenter   |     VMware VM、Hyper-V VM 和物理服务器    |
|Windows Server 2008 R2 SP1 - 64 位<br />- Standard<br />- Enterprise<br />- Datacenter     |     VMware VM、Hyper-V VM 和物理服务器|

> [!WARNING]
> - 不支持迁移运行 Server Core 的服务器。
> - 在迁移之前，请确保已安装最新的 Service Pack 和 Windows 更新。

## <a name="prerequisites"></a>先决条件

在开始之前，最好是查看用于 [VMware 和物理服务器迁移](vmware-azure-architecture.md)或 [Hyper-V 虚拟机迁移](hyper-v-azure-architecture.md)的 Azure Site Recovery 体系结构 

若要迁移运行 Windows Server 2008 或 Windows Server 2008 R2 的 Hyper-V 虚拟机，请遵循[将本地计算机迁移到 Azure](migrate-tutorial-on-premises-azure.md) 教程中的步骤。

本教程的余下部分介绍如何迁移运行 Windows Server 2008 或 2008 R2 的本地 VMware 虚拟机和物理服务器。

<!--Not Available on [Click here](https://aka.ms/migrateVMs-signup)-->

## <a name="limitations-and-known-issues"></a>限制和已知问题

- 用于迁移 Windows Server 2008 SP2 服务器的配置服务器、其他进程服务器和移动服务应运行 9.19.0.0 版或更高版本的 Azure Site Recovery 软件。

- 不支持使用应用程序一致性恢复点和多 VM 一致性功能来复制运行 Windows Server 2008 SP2 的服务器。 应将 Windows Server 2008 SP2 服务器迁移到崩溃一致性恢复点。 默认情况下，每隔 5 分钟生成崩溃一致性恢复点。 由于缺少应用程序一致性恢复点，结合配置的应用程序一致性快照频率使用复制策略会导致应用程序运行状况变得严重。 为避免误报，请将复制策略中的应用程序一致性快照频率设置为“关闭”。

- 应在要迁移的服务器中安装 .NET Framework 3.5 Service Pack 1，这样才能使移动服务正常工作。

- 如果服务器包含动态磁盘，在某些配置中你可能会发现，这些磁盘在故障转移的服务器上已标记为脱机，或显示为外部磁盘。 此外还可能发现，不同动态磁盘上的镜像卷的镜像集状态已标记为“故障冗余”。 可以在 diskmgmt.msc 中通过手动导入并重新激活这些磁盘来解决此问题。

- 要迁移的服务器应有 vmstorfl.sys 驱动程序。 如果要迁移的服务器中没有该驱动程序，故障转移可能失败。 
    > [!TIP]
    >通过“C:\Windows\system32\drivers\vmstorfl.sys”路径检查是否存在该驱动程序。 如果找不到该驱动程序，可以通过就地创建一个虚构文件来解决问题。 
    >
    > 打开命令提示符（单击“运行”并键入 cmd），运行以下命令：“copy nul c:\Windows\system32\drivers\vmstorfl.sys”

- 将运行 32 位操作系统的 Windows Server 2008 SP2 服务器故障转移或测试故障转移到 Azure 之后，可能无法立即通过 RDP 连接到这些服务器。 请在 Azure 门户中重启故障转移的虚拟机，并重试连接。 如果仍然无法连接，请检查服务器是否配置为允许远程桌面连接，并确保没有任何防火墙规则或网络安全组阻止连接。 
    > [!TIP]
    > 在迁移服务器之前，我们强烈建议执行测试故障转移。 确保已在每个要迁移的服务器上执行至少一次成功的测试性故障转移。 在执行测试故障转移的过程中，请连接到测试故障转移的计算机，并确保一切符合预期。
    >
    >测试故障转移操作不会造成中断，可帮助你通过在所选的隔离网络中创建虚拟机来测试迁移。 与故障转移操作不同，在测试故障转移操作期间，数据复制会持续进行。 在准备好迁移之前，可以执行任意次测试故障转移。 
    >
    >

## <a name="getting-started"></a>入门

执行以下任务，准备好 Azure 订阅和本地 VMware/物理环境：

1. [准备 Azure](tutorial-prepare-azure.md)
2. 准备本地 [VMware](vmware-azure-tutorial-prepare-on-premises.md)

## <a name="create-a-recovery-services-vault"></a>创建恢复服务保管库

1. 登录到 [Azure 门户](https://portal.azure.cn) > **恢复服务**。
2. 单击“创建资源” > “监视 + 管理” > “备份和站点恢复(OMS)”。   

    <!--MOONCAKE is Correct on **Monitoring + Management**-->

3. 在“名称”中，指定友好名称 **W2K8-migration**。  如果有多个订阅，请选择合适的一个。
4. 创建资源组 **w2k8migrate**。
5. 指定 Azure 区域。 若要查看受支持的区域，请参阅 [Azure Site Recovery 定价详细信息](https://www.azure.cn/pricing/details/site-recovery/)中的“地域可用性”。
6. 若要从仪表板快速访问保管库，请单击“固定到仪表板”，然后单击“创建”。  

    ![新保管库](media/migrate-tutorial-windows-server-2008/migrate-windows-server-2008-vault.png)

新保管库将添加到“仪表板”中的“所有资源”下，以及“恢复服务保管库”主页面上。   

## <a name="prepare-your-on-premises-environment-for-migration"></a>准备好要迁移的本地环境

- 若要迁移 VMware 上运行的 Windows Server 2008 虚拟机，请[在 VMware 上设置本地配置服务器](vmware-azure-tutorial.md#set-up-the-source-environment)。
- 如果不能将配置服务器设置为 VMware 虚拟机，请[在本地物理服务器或虚拟机上设置配置服务器](physical-azure-disaster-recovery.md#set-up-the-source-environment)。

## <a name="set-up-the-target-environment"></a>设置目标环境

选择并验证目标资源。

1. 单击“准备基础结构” > “目标”，然后选择要使用的 Azure 订阅   。
2. 指定资源管理器部署模型。
3. Site Recovery 检查是否有一个或多个兼容的 Azure 存储帐户和网络。

## <a name="set-up-a-replication-policy"></a>设置复制策略

1. 若要创建新的复制策略，请单击“Site Recovery 基础结构” > “复制策略” > “+复制策略”    。
2. 在“创建复制策略”中指定策略名称  。
3. 在“RPO 阈值”中，指定恢复点目标 (RPO) 限制  。 如果复制 RPO 超过此限制，则会生成警报。
4. 在“恢复点保留期”中，指定每个恢复点的保留期时长（以小时为单位）  。 可以将复制的服务器恢复到此窗口中的任何点。 复制到高级存储的计算机最多支持 24 小时的保留期，复制到标准存储的计算机最多支持 72 小时的保留期。
5. 在“应用一致性快照频率”中，指定“关闭”。   单击“确定”创建该策略  。

此策略自动与配置服务器关联。

> [!WARNING]
> 确保在复制策略的“应用一致性快照频率”设置中指定“关闭”。  复制运行 Windows Server 2008 的服务器时，仅支持崩溃一致性恢复点。 为“应用一致性快照频率”指定任何其他值时，由于缺少应用一致性恢复点，会导致服务器的复制运行状况出现严重问题，因此会生成假警报。

![创建复制策略](media/migrate-tutorial-windows-server-2008/create-policy.png)

## <a name="enable-replication"></a>启用复制

为要迁移的 Windows Server 2008 SP2/Windows Server 2008 R2 SP1 服务器[启用复制](physical-azure-disaster-recovery.md#enable-replication)。

   ![添加物理服务器](media/migrate-tutorial-windows-server-2008/Add-physical-server.png)

   ![启用复制](media/migrate-tutorial-windows-server-2008/Enable-replication.png)

## <a name="run-a-test-migration"></a>运行测试迁移

初始复制完成并且服务器状态变为“受保护”后，可对服务器复制执行测试故障转移。 

运行[测试故障转移](tutorial-dr-drill-azure.md)，确保一切如预期正常运行。

   ![测试故障转移](media/migrate-tutorial-windows-server-2008/testfailover.png)

## <a name="migrate-to-azure"></a>迁移到 Azure

为想要迁移的计算机运行故障转移。

1. 在“受保护的项”   > “复制的项”  中，单击计算机 >“故障转移”  。

    <!--MOONCAKE: **Protected Items** to replace **Setting**-->

2. 在“故障转移”中，选择要故障转移到的“恢复点”   。 选择最新恢复点。
3. 选择“在开始故障转移前关闭计算机”  。 Site Recovery 在触发故障转移之前会尝试关闭服务器。 即使关机失败，故障转移也仍会继续。 可以在“作业”  页上跟踪故障转移进度。
4. 检查 Azure VM 是否在 Azure 中按预期显示。
5. 在“复制的项”  中，右键单击服务器 >“完成迁移”  。 这样会执行以下操作：

    - 完成迁移过程，停止服务器复制，并停止服务器的 Site Recovery 计费。
    - 此步骤清除复制数据。 它不删除迁移的 VM。

    ![完成迁移](media/migrate-tutorial-windows-server-2008/complete-migration.png)

> [!WARNING]
> **请勿取消正在进行的故障转移**：在故障转移开始前，服务器复制已停止。 如果取消正在进行的故障转移，故障转移会停止，但服务器将不继续复制。

<!-- Update_Description: update meta properties, update link, wording update -->
