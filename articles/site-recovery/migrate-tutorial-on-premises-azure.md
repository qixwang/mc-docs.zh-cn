---
title: 使用 Azure Site Recovery 迁移本地计算机
description: 本文将介绍如何使用 Azure Site Recovery 将本地计算机迁移到 Azure。
author: rockboyfor
manager: digimobile
ms.service: site-recovery
ms.topic: tutorial
origin.date: 11/12/2019
ms.date: 06/08/2020
ms.author: v-yeche
ms.openlocfilehash: 85bae5c954bbfff7e840b1dacd7cf3ab0914013c
ms.sourcegitcommit: 5ae04a3b8e025986a3a257a6ed251b575dbf60a1
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/05/2020
ms.locfileid: "84440561"
---
# <a name="migrate-on-premises-machines-to-azure"></a>将本地计算机迁移到 Azure

本文将介绍如何使用 [Azure Site Recovery](site-recovery-overview.md) 将本地计算机迁移到 Azure。 

<!--Not Available on [Learn more](../migrate/migrate-services-overview.md)-->

本教程介绍如何将本地 VM 和物理服务器迁移到 Azure。 你将学习如何执行以下操作：

> [!div class="checklist"]
> * 设置迁移的源和目标环境
> * 设置复制策略
> * 启用复制
> * 运行测试迁移，确保一切按预期正常工作
> * 面向 Azure 运行一次性故障转移

<!--Not Available on [Find out more](https://aka.ms/migrateVMs-signup)-->

## <a name="before-you-start"></a>开始之前

不支持半虚拟化驱动程序导出的设备。

## <a name="prepare-azure-and-on-premises"></a>在 Azure 中和本地做好准备

1. 按[此文](tutorial-prepare-azure.md)中所述准备好 Azure。 虽然此文介绍的是灾难恢复的准备步骤，但这些步骤同样适用于迁移。
2. 准备本地 [VMware](vmware-azure-tutorial-prepare-on-premises.md) 服务器或 [Hyper-V](hyper-v-prepare-on-premises-tutorial.md) 服务器。 如果迁移的是物理机，则不需要做任何准备。 只需检查[支持矩阵](vmware-physical-azure-support-matrix.md)即可。

## <a name="select-a-protection-goal"></a>选择保护目标

选择要复制的内容以及要复制到的位置。
1. 单击“恢复服务保管库”> 保管库。
2. 在“资源”菜单中，单击“Site Recovery” > “准备基础结构” > “保护目标”。
3. 在“保护目标”中，选择要迁移的内容。
    - **VMware**：选择“到 Azure” > “是，使用 VMWare vSphere 虚拟机监控程序”。
    - **物理计算机**：选择“到 Azure” > “未虚拟化/其他”。
    - **Hyper-V**：选择“到 Azure” > “是，使用 Hyper-V”。 如果 Hyper-V VM 由 VMM 管理，则选择“是”。

## <a name="set-up-the-source-environment"></a>设置源环境

**方案** | **详细信息**
--- | --- 
VMware | 设置[源环境](vmware-azure-set-up-source.md)和[配置服务器](vmware-azure-deploy-configuration-server.md)。
物理机 | [设置](physical-azure-set-up-source.md)源环境和配置服务器。
Hyper-V | 设置[源环境](hyper-v-azure-tutorial.md#set-up-the-source-environment)<br/><br/> 为使用 System Center VMM 部署的 Hyper-V 设置[源环境](hyper-v-vmm-azure-tutorial.md#set-up-the-source-environment)。

## <a name="set-up-the-target-environment"></a>设置目标环境

选择并验证目标资源。

1. 单击“准备基础结构” > “目标”，然后选择要使用的 Azure 订阅 。
2. 指定资源管理器部署模型。
3. Site Recovery 将检查 Azure 资源。
    - 如果迁移的是 VMware VM 或物理服务器，则 Site Recovery 将会验证是否提供了一个 Azure 网络，在故障转移后创建 Azure VM 时，会将该 VM 放入该网络。
    - 如果迁移的是 Hyper-V VM，则 Site Recovery 将会验证是否存在兼容的 Azure 存储帐户和网络。
4. 如果迁移的是 System Center VMM 管理的 Hyper-V VM，请设置[网络映射](hyper-v-vmm-azure-tutorial.md#configure-network-mapping)。

## <a name="set-up-a-replication-policy"></a>设置复制策略

**方案** | **详细信息**
--- | --- 
VMware | 为 VMware VM 设置[复制策略](vmware-azure-set-up-replication.md)。
物理机 | 为物理机设置[复制策略](physical-azure-disaster-recovery.md#create-a-replication-policy)。
Hyper-V | 设置[复制策略](hyper-v-azure-tutorial.md#set-up-a-replication-policy)<br/><br/> 为使用 System Center VMM 部署的 Hyper-V 设置[复制策略](hyper-v-vmm-azure-tutorial.md#set-up-a-replication-policy)。

## <a name="enable-replication"></a>启用复制

**方案** | **详细信息**
--- | --- 
VMware | 为 VMware VM [启用复制](vmware-azure-enable-replication.md)。
物理机 | 为物理机[启用复制](physical-azure-disaster-recovery.md#enable-replication)。
Hyper-V | [启用复制](hyper-v-azure-tutorial.md#enable-replication)<br/><br/> 为使用 System Center VMM 部署的 Hyper-V [启用复制](hyper-v-vmm-azure-tutorial.md#enable-replication)。

## <a name="run-a-test-migration"></a>运行测试迁移

运行[测试故障转移](tutorial-dr-drill-azure.md)，确保一切如预期正常运行。

## <a name="migrate-to-azure"></a>迁移到 Azure

为想要迁移的计算机运行故障转移。

1. 在“受保护的项” > “复制的项”中，单击计算机 >“故障转移”。

    <!--MOONCAKE: **Protected Items** to replace **Setting**-->

2. 在“故障转移”中，选择要故障转移到的“恢复点” 。 选择最新恢复点。
3. 加密密钥设置与此方案无关。
4. 选择“在开始故障转移前关闭计算机”。 在触发故障转移之前，Site Recovery 会尝试关闭虚拟机。 即使关机失败，故障转移也仍会继续。 可以在“作业”页上跟踪故障转移进度。
5. 检查 Azure VM 是否在 Azure 中按预期显示。
6. 在“复制的项”中，右键单击 VM >“完成迁移”。 这样会执行以下操作：

    - 完成迁移过程，停止本地 VM 的复制，并停止 VM 的 Site Recovery 计费。
    - 此步骤清除复制数据。 它不删除迁移的 VM。

        ![完成迁移](./media/migrate-tutorial-on-premises-azure/complete-migration.png)

> [!WARNING]
> **请勿取消正在进行的故障转移**：在故障转移开始前，VM 复制已停止。 如果取消正在进行的故障转移，故障转移会停止，但 VM 将不再进行复制。

在某些情况下，故障转移需要大约八到十分钟的时间完成其他进程。 你可能注意到物理服务器、VMware Linux 计算机、未启用 DHCP 服务的 VMware VM 和未安装以下启动驱动程序：storvsc、vmbus、storflt、intelide、atapi 的 VMware VM 需要更长的测试故障转移时间。

## <a name="after-migration"></a>迁移之后

在计算机迁移到 Azure 后，应当完成许多步骤。

<!-- Not Available on [recovery plans](site-recovery-runbook-automation.md)-->

### <a name="post-migration-steps-in-azure"></a>Azure 中的迁移后步骤

- 执行任何迁移后应用微调，例如，更新数据库连接字符串和 Web 服务器配置。 
- 在当前在 Azure 中运行的迁移后应用程序上执行最终的应用程序和迁移验收测试。
- [Azure VM 代理](/virtual-machines/extensions/agent-windows)管理 VM 与 Azure 结构控制器之间的交互。 它是某些 Azure 服务所必需的，例如 Azure 备份、Site Recovery 和 Azure 安全。
    - 如果是迁移 VMware 计算机和物理服务器，则移动服务安装程序会在 Windows 计算机上安装可用的 Azure VM 代理。 在 Linux VM 上，建议你在故障转移后安装代理。
    - 如果是将 Azure VM 迁移到次要区域，则必须在迁移之前在 VM 上预配 Azure VM 代理。
    - 如果是将 Hyper-V VM 迁移到 Azure，请在迁移之后在 Azure VM 上安装 Azure VM 代理。
- 手动从 VM 中删除任何 Site Recovery 提供程序/代理。 如果迁移 VMware VM 或物理服务器，请从 VM 中卸载移动服务。
    
    <!--MOONCAKE: should be []() format-->
    
- 为增强恢复能力，请采取以下措施：
    - 通过使用 Azure 备份服务备份 Azure VM 来确保数据安全。 [了解详细信息]( https://docs.azure.cn/backup/quick-backup-vm-portal)。
    - 通过使用 Site Recovery 将 Azure VM 复制到次要区域，使工作负荷保持运行并持续可用。 [了解详细信息](azure-to-azure-quickstart.md)。
- 为提高安全性，请采取以下措施：
    - 通过 Azure 安全中心[恰时管理]( https://docs.azure.cn/security-center/security-center-just-in-time)锁定并限制入站流量访问。
    - 使用[网络安全组](/virtual-network/security-overview)限制到管理终结点的网络流量。
    - 部署 [Azure 磁盘加密](/security/azure-security-disk-encryption-overview)来帮助保护磁盘，并确保数据安全，防止被盗和未经授权的访问。
    - 阅读关于[保护 IaaS 资源](https://azure.microsoft.com/services/virtual-machines/secure-well-managed-iaas)的详细内容，并访问 [Azure 安全中心](https://www.azure.cn/home/features/security-center/)。

    <!--CORRECT ON [securing IaaS resources](https://azure.microsoft.com/services/virtual-machines/secure-well-managed-iaas)-->
    <!-- Not Available on  - For monitoring and management:-->
    <!-- Not Available on  [Azure Cost Management](/cost-management/overview)-->

### <a name="post-migration-steps-on-premises"></a>本地的迁移后步骤

- 将应用流量转移到在迁移后的 Azure VM 实例上运行的应用。
- 从本地 VM 清单中删除本地 VM。
- 从本地备份中删除本地 VM。
- 更新任何内部文档，以显示 Azure VM 的新位置和 IP 地址。

## <a name="next-steps"></a>后续步骤

在本教程中，我们已将本地 VM 迁移到 Azure VM。 Now

> [!div class="nextstepaction"]
> 为 Azure VM 设置到 Azure 次要区域的[灾难恢复](azure-to-azure-replicate-after-migration.md)。

<!-- Update_Description: update meta properties, wording update, update link -->