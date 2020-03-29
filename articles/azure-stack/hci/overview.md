---
title: Azure Stack HCI 概述
description: Azure Stack HCI 是超融合的 Windows Server 2019 群集，可在本地环境使用经过验证的硬件来运行虚拟化工作负荷。 你也可以选择性地连接到 Azure 服务来执行云备份和站点恢复等操作。 Azure Stack HCI 解决方案使用经过 Microsoft 验证的硬件来确保最佳性能和可靠性，并且包含 NVMe 驱动器、持久性内存及远程直接内存访问 (RDMA) 网络等技术支持。
ms.topic: article
author: WenJason
ms.author: v-jay
ms.localizationpriority: medium
origin.date: 11/04/2019
ms.date: 03/23/2020
ms.openlocfilehash: 80e5a42b0af58afc27ba712d8f40774ac09313c9
ms.sourcegitcommit: 90d01d08faf8adb20083363a8e4e5aab139cd9b2
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/26/2020
ms.locfileid: "80290421"
---
# <a name="azure-stack-hci-overview"></a>Azure Stack HCI 概述

Azure Stack HCI 是超融合的 Windows Server 2019 群集，可在本地环境使用经过验证的硬件来运行虚拟化工作负荷。 你也可以选择性地连接到 Azure 服务来执行云备份和站点恢复等操作。 Azure Stack HCI 解决方案使用经过 Microsoft 验证的硬件来确保最佳性能和可靠性，并且包含 NVMe 驱动器、持久性内存及远程直接内存访问 (RDMA) 网络等技术支持。

Azure Stack HCI 是结合多个产品的解决方案：

- 来自 OEM 合作伙伴的硬件

- Windows Server 2019 Datacenter Edition

- Windows 管理中心

- Azure 服务（可选）

![Azure Stack HCI 是由众多硬件合作伙伴提供的 Microsoft 超融合解决方案。](media/overview/azure-stack-hci-solution.png)

Azure Stack HCI 是由众多硬件合作伙伴提供的 Microsoft 超融合解决方案。 请考虑以下适用于超融合解决方案的场景，以帮助确定 Azure Stack HCI 是否是最符合需求的解决方案：

- **更换过时的硬件。** 替代较旧的服务器和存储基础结构，并在本地环境及边缘上使用现有 IT 技能及工具运行 Windows 和 Linux 虚拟机。

- **整合虚拟化工作负荷。** 在高效的超融合基础结构上整合旧式应用。 利用用于运行大规模数据中心（例如 Microsoft Azure）的同类云效率。

- **连接到 Azure 以使用混合云服务。** 简化 Azure 中云管理和安全服务的访问，包括异地备份、站点恢复和基于云的监视，等等。

## <a name="the-azure-stack-family"></a>Azure Stack 系列

Azure Stack HCI 属于 Azure 和 Azure Stack 系列，与 Azure Stack Hub 使用相同的软件定义计算、存储及网络软件。 下面是不同解决方案的快速摘要（如需更多详细信息，请参阅[比较 Azure Stack 生态系统](../operator/compare-azure-azure-stack.md)）：

- [Azure](https://azure.cn) - 将公有云服务用于按需自助式计算资源，以便迁移和现代化现有应用，以及生成新的云原生应用。
- [Azure Stack HCI](https://azure.microsoft.com/overview/azure-stack/hci) - 在本地运行虚拟化应用、更换和整合过时的服务器基础结构，以及连接到 Azure 以使用云服务。
- [Azure Stack Hub](../operator/azure-stack-overview.md) - 在连接中断时运行本地云应用，或者使用一致的 Azure 服务满足法规要求。

![Azure Stack Edge 是一项云托管设备，用于在边缘运行机器学习和容器化应用；Azure Stack HCI 是一项超聚合解决方案，用于在本地运行 VM 和存储；而 Azure Stack Hub 则在本地提供云原生 Azure 一致性服务。](media/overview/azure-family.png)

若要了解详细信息，请访问以下链接：

请访问我们的 [Azure Stack HCI](https://azure.microsoft.com/overview/azure-stack/hci) 解决方案网站了解更多信息。

## <a name="hyperconverged-efficiencies"></a>超融合效率

Azure Stack HCI 解决方案在行业标准的 x86 服务器和组件上结合了高度虚拟化的计算、存储和网络功能。 在同一群集中组合资源可以更轻松地进行部署、管理及缩放。 通过所选的命令行自动化或 Windows 管理中心进行管理。

使用 Hyper-V（Microsoft 云的基础虚拟机监控程序技术）和存储空间直通技术（内置对 NVMe、永久性内存和远程直接内存访问 (RDMA) 网络的支持），为服务器应用程序实现业界领先的虚拟机性能。

通过受防护的虚拟机、网络微段和本地加密，帮助保护应用和数据的安全。

## <a name="hybrid-capabilities"></a>混合功能

可以通过公有云中的超融合基础结构平台，利用云和本地的优势。 团队可以通过内置集成开始对 Azure 基础结构管理服务构建云技能：

- Azure Site Recovery 可以实现高可用性和灾难恢复即服务 (DRaaS)。

- Azure Monitor 是一个集中式中心，通过 AI 驱动的高级分析来跟踪应用程序、网络和基础结构中发生的所有事件。

- 云见证使用 Azure 作为群集仲裁的轻量决胜策略。

- Azure 备份可以保护场外数据和防范勒索软件。

- Azure 网络适配器可通过点到站点 VPN 将本地资源连接到 Azure 中的 VM。

## <a name="management-tools-and-system-center"></a>管理工具和 System Center

Azure Stack HCI 与 Azure Stack Hub 使用相同的虚拟化和软件定义存储和网络软件。 但是，使用 Azure Stack HCI 可在群集上拥有完全管理员权限，并且可以直接管理其所有技术：

- [Hyper-V](https://docs.microsoft.com/windows-server/virtualization/hyper-v/hyper-v-on-windows-server)
- [存储空间直通](https://docs.microsoft.com/windows-server/storage/storage-spaces/storage-spaces-direct-overview)
- [软件定义的网络](https://docs.microsoft.com/windows-server/networking/sdn/)
- [故障转移群集](https://docs.microsoft.com/windows-server/failover-clustering/failover-clustering-overview)

若要管理这些技术，可以使用以下管理工具：

- [Windows 管理中心](https://docs.microsoft.com/windows-server/manage/windows-admin-center/overview)
- [System Center](https://www.microsoft.com/cloud-platform/system-center)
- [PowerShell](https://docs.microsoft.com/powershell/?view=powershell-6)
- 其他管理工具（例如[服务器管理器](https://docs.microsoft.com/windows-server/administration/server-manager/server-manager)）和 MMC 管理单元
- 非 Microsoft 工具（例如 5Nine Manager）

如果你选择使用 System Center 部署和管理基础结构，将使用 System Center 虚拟机管理 (VMM) 和 System Center Operations Manager。 使用 VMM 可以预配和管理在私有云中创建和部署虚拟机与服务所需的资源。 使用 Operations Manager 可以监视整个企业中的服务、设备和操作，从而识别问题并立即采取措施。

## <a name="hardware-partners"></a>硬件合作伙伴

可以从 20 家合作伙伴购买运行 Windows Server 2019 的已验证 Azure Stack HCI 解决方案。 首选的 Microsoft 合作伙伴可帮助你正常运行解决方案，消除冗长的设计和生成时间。 他们还可提供单一联络点来帮助你完成服务的实施与支持。

请访问 [Azure Stack HCI 网站](https://azure.microsoft.com/overview/azure-stack/hci)，以查看这些 Microsoft 合作伙伴目前提供的 70 多种 Azure Stack HCI 解决方案：ASUS、Axellio、bluechip、DataON、Dell EMC、Fujitsu、HPE、Hitachi、华为、联想、NEC、primeLine Solutions、QCT、SecureGUARD 和 Supermicro。

## <a name="faq"></a>常见问题

### <a name="what-do-azure-stack-hub-and-azure-stack-hci-solutions-have-in-common"></a>Azure Stack Hub 和 Azure Stack HCI 解决方案有哪些共同之处？

Azure Stack HCI 解决方案与 Azure Stack Hub 同样有 Hyper-V 架构的软件定义计算、存储和网络技术功能。 这两个产品都符合严格的测试和验证准则，目的是确保与基础硬件平台配合时的可靠性和兼容性。

### <a name="how-are-they-different"></a>那么其不同之处在哪里？

使用 Azure Stack Hub 可在本地运行云服务。 可在本地运行 Azure IaaS 和 PaaS 服务，无论位于任何位置，都能以一致的方式生成并运行云应用程序，并可以在本地使用 Azure 门户进行管理。

使用 Azure Stack HCI 可在本地运行虚拟化工作负荷，并使用 Windows 管理中心和你熟悉的 Windows Server 工具进行管理。 可以选择性地连接到 Azure 以实现混合方案，例如基于云的站点恢复、监视等。

### <a name="why-is-microsoft-bringing-its-hci-offering-to-the-azure-stack-family"></a>Microsoft 为何将 HCI 产品/服务纳入 Azure Stack 系列？

Microsoft 的超融合技术已是 Azure Stack Hub 的基础。

许多 Microsoft 客户使用复杂的 IT 环境，而我们的目标是提供符合客户需求的解决方案，让他们使用适当的技术来满足适当的业务需求。 Azure Stack HCI 是基于 Windows Server 2016 的创新 Windows Server 软件定义 (WSSD) 解决方案，以前由我们的硬件合作伙伴提供。 之所以将其纳入 Azure Stack 系列，是因为我们已开始提供新选项来无缝连接 Azure，以提供基础结构管理服务。

### <a name="does-azure-stack-hci-need-to-be-connected-to-azure"></a>Azure Stack HCI 是否需要连接到 Azure？

否，这是可选的操作。 你可以使用与 Azure 集成来实现混合方案，例如异地备份和灾难恢复，以及基于云的监视和更新管理，但这些方案是可选的。 即使与 Internet 断开连接，也仍可实现这些方案。

### <a name="how-does-azure-stack-hci-relate-to-windows-server"></a>Azure Stack HCI 与 Windows Server 有何关系？

Windows Server 2019 几乎是每个 Azure 产品的基础。 你重视的所有功能都继续在 Windows Server 中提供，并且受到支持。 Azure Stack HCI 是在本地部署 HCI 的建议方式，可以使用我们的合作伙伴提供的经过 Microsoft 验证的硬件。

### <a name="can-i-upgrade-from-azure-stack-hci-to-azure-stack-hub"></a>是否可以从 Azure Stack HCI 升级到 Azure Stack Hub？ 

不能，但客户可将工作负荷从 Azure Stack HCI 迁移到 Azure Stack Hub 或 Azure。

### <a name="what-azure-services-can-i-connect-to-azure-stack-hci"></a>哪些 Azure 服务可连接到 Azure Stack HCI？

有关可与 Azure Stack HCI 连接的 Azure 服务的更新列表，请参阅[将 Windows Server 连接到 Azure 混合服务](https://docs.microsoft.com/windows-server/manage/windows-admin-center/azure/index)。

### <a name="how-does-the-cost-of-azure-stack-hci-compare-to-azure-stack-hub"></a>Azure Stack HCI 和 Azure Stack Hub 的成本有何差别？ 

Azure Stack Hub 以完全集成的系统销售，其中包括服务和支持。 你可以购买 Azure Stack Hub 作为你管理的系统，也可以作为我们的合作伙伴提供的完全托管服务。 除了基础系统以外，在 Azure Stack Hub 或 Azure 上运行的 Azure 服务也是按即用即付方式出售的。

Azure Stack HCI 解决方案遵循传统的购买模式。 可以向 Azure Stack HCI 合作伙伴购买经验证的硬件，以及从现有的各种渠道购买软件（具有软件定义的数据中心功能的 Windows Server 2019 Datacenter Edition 以及 Windows 管理中心）。 对于与 Windows 管理中心配合使用的 Azure 服务，可以通过 Azure 订阅付费。

### <a name="how-do-i-buy-azure-stack-hci-solutions"></a>如何购买 Azure Stack HCI 解决方案？

执行以下步骤：

1. 从首选的硬件合作伙伴购买经 Microsoft 验证的硬件系统。
1. 安装 Windows Server 2019 Datacenter Edition 和 Windows 管理中心以获取管理功能，以及连接到 Azure 以获取云服务
1. 选择性地使用 Azure 帐户将基于云的管理和安全服务附加到工作负荷。

![若要购买 Azure Stack HCI 解决方案，请选择最符合需求的硬件合作伙伴与配置。](media/overview/buying-azure-stack-hci.png)

## <a name="compare-azure-stack-hub-and-azure-stack-hci"></a>Azure Stack Hub 和 Azure Stack HCI 的比较

当你的组织进行数字化变革时，你可能会发现，使用公有云服务以新式体系结构作为构建基础来更新旧式应用可以加快变革的速度。 但是，出于技术和法规障碍等原因，许多工作负荷必须保留在本地。 下表可帮助你确定哪种 Microsoft 混合云策略可随时随地提供所需的服务，并为任何位置的工作负荷提供云创新。

| Azure Stack Hub | Azure Stack HCI |
| --------------- | --------------- |
| 新的技能，创新的流程 | 相同的技能，熟悉的流程 |
| 数据中心内的 Azure 服务 | 将数据中心连接到 Azure 服务 |

### <a name="when-to-use-azure-stack-hub"></a>何时使用 Azure Stack Hub

| Azure Stack Hub | Azure Stack HCI |
| --------------- | --------------- |
| 针对自助式基础结构即服务 (IaaS) 使用 Azure Stack Hub，因为其具有强式隔离，并且可对位于相同位置的多个租户精确地跟踪使用情况和进行费用分摊。 非常适合用于服务提供商和企业私有云。 来自 Azure 市场的模板。 | Azure Stack HCI 原生不会实施或提供多租户。 |
| 在本地使用 Azure Stack Hub 开发和运行依赖于 Web 应用、Functions 或事件中心等平台即服务 (PaaS) 的应用。 在 Azure Stack Hub 上运行的这些服务完全与在 Azure 中运行一样，从而提供一致的混合开发和运行时环境。 | Azure Stack HCI 不会在本地运行 PaaS 服务。 |
| 使用 Azure Stack Hub 现代化应用部署和配合 DevOps 做法的操作，这些做法包括基础结构即代码、持续集成和持续部署 (CI/CD)，以及 Azure 一致性 VM 扩展等便利功能。 非常适合用于开发和 DevOps 团队。 | Azure Stack HCI 原生不包括任何 DevOps 工具。 |

### <a name="when-to-use-azure-stack-hci"></a>何时使用 Azure Stack HCI

| Azure Stack Hub | Azure Stack HCI |
| --------------- | --------------- |
| Azure Stack Hub 至少需要 4 个节点，并需要自身的网络交换机。 | 使用 Azure Stack HCI 可以尽量减少远程办公室和分公司的占用空间。 一开始可以仅使用 2 个服务器节点和无交换机的后端到后端网络，以实现最高简便度和负担能力。 硬件产品的入门配置为 4 个驱动器和 64 GB 内存，每个节点的费用不超过 10000 美元。 |
| 为了与 Azure 保持一致，Azure Stack Hub 将限制 Hyper V 可配置性和功能。 | 使用 Azure Stack HCI 可为经典企业应用（例如 Exchange、SharePoint 和 SQL Server）实现单纯有用的 Hyper-V 虚拟化，以及虚拟化 Windows Server 角色，例如文件服务器、DNS、DHCP、IIS 和 AD。 不受限制地访问所有 Hyper-V 功能，例如受防护的 VM。|
| Azure Stack Hub 不会公开这些基础结构技术。 | 借助 Azure Stack HCI 可以使用软件定义的基础结构来代替过时的存储设备阵列或网络设备，而无需进行重大的重新改造。 可以直接访问和管理内置的 Hyper-V、存储空间直通和软件定义的网络 (SDN)。 |
