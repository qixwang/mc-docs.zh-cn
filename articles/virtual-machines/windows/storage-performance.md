---
title: 优化 Azure Lsv2 系列虚拟机上的性能 - 存储
description: 了解如何为 Lsv2 系列虚拟机上的解决方案优化性能。
author: rockboyfor
ms.service: virtual-machines-windows
ms.subservice: sizes
ms.topic: how-to
ms.workload: infrastructure-services
origin.date: 04/17/2019
ms.date: 5/18/2020
ms.author: v-yeche
ms.openlocfilehash: d445553f5b88adb09ca681f2312db476e587fdc4
ms.sourcegitcommit: f6d0bd7958e0720367022e68dc1824448c866882
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/15/2020
ms.locfileid: "83417255"
---
<!--CHECK THE BURSTING FEATHER BEFORE RELEASE-->
<!--Verified successfully-->
# <a name="optimize-performance-on-the-lsv2-series-virtual-machines"></a>优化 Lsv2 系列虚拟机上的性能

Lsv2 系列虚拟机在广泛的应用程序和行业中支持需要在本地存储上实现高 I/O 和高吞吐量的各种工作负荷。  Lsv2 系列最适用于大数据、SQL、NoSQL 数据库、数据仓库和大型事务数据库，其中包括 Cassandra、MongoDB、Cloudera 和 Redis。

Lsv2 系列虚拟机 (VM) 的设计最充分地利用了 AMD EPYC™ 7551 处理器，以在处理器、内存、NVMe 设备和 VM 等方面提供最佳性能。 除了最大程度地发挥硬件性能之外，Lsv2 系列 VM 的设计还考虑到了 Windows 和 Linux 操作系统的需求，目的是同时提高硬件和软件的性能。

对软件和硬件进行优化后，得到了 2018 年 12 月初发布到 Azure 市场的 [Windows Server 2019 Datacenter](https://market.azure.cn/marketplace/apps/microsoftwindowsserver.windowsserver?tab=Overview) 优化版本，该版本支持在 Lsv2 系列 VM 中的 NVMe 设备上发挥最高性能。

本文提供了一些技巧和建议来确保你的工作负荷和应用程序达到 VM 中设计的最高性能。 随着更多 Lsv2 优化映像添加到 Azure 市场中，此页面上的信息将不断更新。

## <a name="amd-eypc-chipset-architecture"></a>AMD EYPC™ 芯片组体系结构

Lsv2 系列 VM 使用基于 Zen 微体系结构的 AMD EYPC™ 服务器处理器。 AMD 为 EYPC™ 开发了 Infinity Fabric (IF) 作为其 NUMA 模型的可缩放互连，该模型可用于芯片内通信、封装内通信以及多封装通信。 与 Intel 现代单片处理器上使用的 QPI（快速路径互连）和 UPI（超高性能路径互连）相比，AMD 的多 NUMA 小型芯片体系结构可能同时带来了性能优势和挑战。 内存带宽和延迟约束的实际影响取决于所运行的工作负荷的类型。

## <a name="tips-for-maximizing-performance"></a>发挥最高性能的技巧

* 为 Lsv2 系列 VM 提供支持的硬件利用了具有八个 I/O 队列对 (QP) 的 NVMe 设备。 每个 NVMe 设备 I/O 队列实际上是一对：一个提交队列和一个完成队列。 NVMe 驱动程序经过设置，可以按轮循机制计划来分发 I/O，以便优化这八个 I/O QP 的利用率。 为了获得最高性能，每个设备应相应地运行八个作业。

* 在存在活动工作负荷期间，避免将 NVMe 管理命令（例如，NVMe SMART 信息查询等）与 NVMe I/O 命令混合使用。 Lsv2 NVMe 设备由 Hyper-V NVMe Direct 技术提供支持，只要有任何 NVMe 管理命令挂起，该技术就会切换到“慢速模式”。 如果发生这种情况，Lsv2 用户可能会看到 NVMe I/O 性能显著下降。

* Lsv2 用户不应依赖于在 VM 内针对数据驱动器报告的设备 NUMA 信息（全为 0）来确定其应用的 NUMA 关联。 为了获得更好的性能，建议尽可能将工作负荷分散到多个 CPU。 

* 对于 Lsv2 VM NVMe 设备，每个 I/O 队列对支持的最大队列深度为 1024（而Amazon i3 QD 限制为 32）。 Lsv2 用户应将其（合成）基准工作负荷限制为队列深度 1024 或更低，以避免触发队列填满的情况，发生这种情况会降低性能。

## <a name="utilizing-local-nvme-storage"></a>利用本地 NVMe 存储

所有 Lsv2 VM 的 1.92 TB NVMe 磁盘上的本地存储都是暂时的。 如果以标准方式重启 VM 成功，则本地 NVMe 磁盘上的数据会保留。 如果对 VM 执行了重新部署、解除分配或删除操作，则不会在 NVMe 上持久保留数据。 如果其他问题导致 VM 或运行它的硬件出现故障，则数据不会持久保留。 发生这种情况时，系统会安全地擦除旧主机上的任何数据。

在某些情况下，可能需要将 VM 移到其他主机，例如，在执行计划内维护操作期间这样做。 可以通过 [Scheduled Events](scheduled-events.md) 来预见计划内维护操作和某些硬件故障。 应该使用 Scheduled Events 来及时了解任何预计的维护和恢复操作。

如果计划内维护事件要求在具有空的本地磁盘的新主机上重新创建 VM，则需要重新同步数据（同样，旧主机上的任何数据都会被安全地擦除）。 出现这种情况的原因是，Lsv2 系列 VM 当前不支持本地 NVMe 磁盘上的实时迁移。

计划内维护有两种模式。

### <a name="standard-vm-customer-controlled-maintenance"></a>标准 VM 客户控制的维护

- VM 会在 30 天时间窗口内移到已更新的主机。
- Lsv2 本地存储数据可能会丢失，因此建议在事件发生前备份数据。

### <a name="automatic-maintenance"></a>自动维护

- 如果客户不执行客户控制的维护，或者出现了紧急情况（如零日漏洞安全事件），则会进行自动维护。
- 目的是保留客户数据，但存在 VM 冻结或重启风险，不过风险很小。
- Lsv2 本地存储数据可能会丢失，因此建议在事件发生前备份数据。

对于即将发生的任何服务事件，请使用受控的维护过程选择对你而言最方便的更新时间。 在事件发生之前，你可以将数据备份到高级存储中。 在维护事件完成后，可以将数据返回到刷新后的 Lsv2 VM 本地 NVMe 存储。

在本地 NVMe 磁盘上维护数据的场景包括：

- VM 正在运行且运行正常。
- VM（被你或 Azure）就地重启。
- VM 已暂停（已停止，未解除分配）。
- 大部分计划内维护服务操作。

安全擦除数据以保护客户的场景包括：

- VM 被（你）重新部署、停止（解除分配）或删除。
- VM 会变得不正常，并且由于硬件问题，必须通过服务修复移到另一节点。
- 要求将 VM 重新分配到另一台主机进行维护的少量计划内维护服务操作。

若要详细了解在本地存储中备份数据的选项，请参阅 [Azure IaaS 磁盘的备份和灾难恢复](backup-and-disaster-recovery-for-azure-iaas-disks.md)。

## <a name="frequently-asked-questions"></a>常见问题

* **如何开始部署 Lsv2 系列 VM？**  
    与任何其他 VM 一样，请使用[门户](quick-create-portal.md)、[Azure CLI](quick-create-cli.md) 或 [PowerShell](quick-create-powershell.md) 来创建 VM。

* **单个 NVMe 磁盘故障是否会导致主机上的所有 VM 都出现故障？**  
    如果在硬件节点上检测到磁盘故障，则硬件处于故障状态。 出现这种情况时，系统会自动解除分配该节点上的所有 VM，并将其移到正常节点。 对于 Lsv2 系列 VM，这意味着在发生故障的节点上，客户的数据也会被安全地擦除，并需要由客户在新节点上重新创建。 如前所述，在实时迁移在 Lsv2 上变得可用之前，故障节点上的数据会在 VM 转移到另一节点时主动随 VM 一起移动。

* **在使用 Windows Server 2012 或 Windows Server 2016 时，是否需要在 Windows 中进行轮询调整？**  
    NVMe 轮询仅在 Azure 上的 Windows Server 2019 中可用。  

* **是否可以切换回传统的中断服务例程 (ISR) 模型？**  
    Lsv2 系列 VM 针对 NVMe 轮询进行了优化。 我们会持续提供更新来改进轮询性能。

* **是否可以在 Windows Server 2019 中调整轮询设置？**  
    用户无法调整轮询设置。

## <a name="next-steps"></a>后续步骤

* 请参阅 Azure 上所有[针对存储性能进行了优化的 VM](/virtual-machines/sizes-storage) 的规范

<!-- Update_Description: new article about storage performance -->
<!--NEW.date: 5/18/2020-->