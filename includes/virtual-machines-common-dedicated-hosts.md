---
title: include 文件
description: include 文件
services: virtual-machines
author: rockboyfor
ms.service: virtual-machines
ms.topic: include
origin.date: 03/10/2020
ms.date: 05/18/2020
ms.author: v-yeche
ms.custom: include file
ms.openlocfilehash: 16ce878467ed6fa385b89adb56d28a8138742046
ms.sourcegitcommit: 8d56bc6baeb42d675695ecef1909d76f5c4a6ae3
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/14/2020
ms.locfileid: "83406216"
---
<!--Verified successfully-->
## <a name="limitations"></a>限制

- 专用主机上目前不支持虚拟机规模集。

## <a name="benefits"></a>优点 

预留整个主机具有以下优势：

- 物理服务器级别的硬件隔离。 不会在你的主机上放置任何其他 VM。 专用主机部署在相同的数据中心，与其他非隔离主机共享相同的网络和底层存储基础结构。
- 控制 Azure 平台发起的维护事件。 尽管大多数维护事件对虚拟机的影响极小甚至没有任何影响，但对于某些敏感工作负荷而言，每暂停一秒都可能会造成影响。 在使用专用主机的情况下，可以选择维护时段，以降低对服务的影响。
- 借助 Azure 混合权益，你可以在 Azure 中使用自己的 Windows 和 SQL 许可证。 使用混合权益可以提供更多的优势。 有关详细信息，请参阅 [Azure 混合权益](https://www.azure.cn/pricing/hybrid-benefit/)。

## <a name="groups-hosts-and-vms"></a>组、主机和 VM  

![专用主机的新资源的视图。](./media/virtual-machines-common-dedicated-hosts/dedicated-hosts2.png)

主机组  是表示专用主机集合的资源。 你在某个区域中创建主机组，并向其中添加主机。

<!--Not Available on and an availability zone-->

主机  是映射到 Azure 数据中心内的物理服务器的资源。 创建主机时，将分配物理服务器。 主机是在主机组中创建的。 主机提供一个 SKU，用于描述可创建哪些 VM 大小。 每个主机可以托管多个不同大小的 VM，前提是这些 VM 来自相同的大小系列。

在 Azure 中创建 VM 时，可以选择要将哪个专用主机用于你的 VM。 对于要将哪些 VM 放置在你的主机上，你可以进行全面控制。

## <a name="high-availability-considerations"></a>高可用性注意事项 

要实现高可用性，应部署多个 VM，并将其分散在多个主机（最少 2 个）上。 在使用 Azure 专用主机的情况下，有多个选项可用来预配基础结构，以形成故障隔离边界。

<!--Not Available on ### Use Availability Zones for fault isolation-->

### <a name="use-fault-domains-for-fault-isolation"></a>使用容错域实现故障隔离

可以在特定的容错域中创建主机。 与规模集或可用性集中的 VM 一样，不同容错域中的主机将放置在数据中心内的不同物理机架上。 创建主机组时，需要指定容错域计数。 在主机组中创建主机时，需为每个主机分配容错域。 VM 不需要任何容错域分配。

容错域不同于归置。 对两个主机使用同一个容错域并不意味着它们彼此邻近。

容错域的范围限定为主机组。 不应该对两个主机组之间的反关联做出任何假设。

<!--Not Available on (unless they are in different availability zones)-->

部署到具有不同容错域的主机的 VM 将在多个存储戳上具有其底层托管磁盘服务，以提高故障隔离保护。

<!--Not Available on ### Using Availability Zones and Fault Domains-->

## <a name="maintenance-control"></a>维护控制

可以偶尔更新支持虚拟机的基础结构，以提高可靠性、性能和安全性，并启动新功能。 只要有可能，Azure 平台就会尝试最大程度地降低平台维护的影响，但如果客户具有对维护敏感的  工作负荷，那么他们就不能容忍出于维护目的而冻结 VM 或断开 VM 连接，哪怕只有几秒。

维护控制  为客户提供了一个选项，以用于跳过在其专用主机上计划的定期平台更新，然后在 35 天滚动时段内他们所选的时间应用更新。

> [!NOTE]
>  维护控制目前以公共预览版提供。 有关详细信息，请参阅“使用 [CLI](/virtual-machines/maintenance-control-cli?toc=/virtual-machines/linux/toc.json&bc=/azure/virtual-machines/linux/breadcrumb/toc.json) 或 [PowerShell](/virtual-machines/maintenance-control-powershell?toc=/virtual-machines/linux/toc.json&bc=/azure/virtual-machines/linux/breadcrumb/toc.json) 通过维护控制来控制更新”  。

## <a name="capacity-considerations"></a>容量注意事项

预配了专用主机之后，Azure 会将其分配给物理服务器。 这会在你需要预配 VM 时保证容量的可用性。 Azure 使用区域中的全部容量为你的主机选取物理服务器。 这也意味着，客户可以预期其专用主机占用空间能够扩展，而不必担心用尽群集中的空间。

<!--Not Available on (or zone)-->

## <a name="quotas"></a>配额

对于每个区域的专用主机，默认配额限制为 3000 个 vCPU。 但是，可以部署的主机数还受主机使用的 VM 大小系列的配额限制。 例如，在中国东部区域，标准预付费套餐  订阅仅为 Dsv3 大小系列提供 10 个 vCPU 的配额。 在这种情况下，需要请求将配额提高到至少 64 个 vCPU，才能部署专用主机。 如果需要，请选择右上角的“请求增加”按钮提出请求  。

![门户中的用量和配额页的屏幕截图](./media/virtual-machines-common-dedicated-hosts/quotas.png)

有关详细信息，请参阅[虚拟机 vCPU 配额](/virtual-machines/windows/quotas)。

试用版和 MSDN 订阅不为 Azure 专用主机提供配额。

## <a name="pricing"></a>定价

无论部署多少个 VM，都会按专用主机对用户收费。 在每月帐单中，你将看到新的可计费资源类型的主机。 专用主机上的 VM 仍会显示在帐单中，但其价格为 0。

主机价格是根据 VM 系列、类型（硬件大小）和区域设置的。 主机价格与主机上支持的最大 VM 大小相关。

软件许可、存储和网络用量与主机和 VM 分开计费。 这些可计费项没有更改。

有关详细信息，请参阅 [Azure 虚拟机定价](https://www.azure.cn/pricing/details/virtual-machines/)。

<!--Not Available on [Reserved Instance of Azure Dedicated Hosts](../articles/virtual-machines/prepay-dedicated-hosts-reserved-instances.md)-->

## <a name="sizes-and-hardware-generations"></a>大小和硬件代系

SKU 是为主机定义的，表示 VM 大小系列和类型。 可以在单个主机中混合使用多个不同大小的 VM，只要这些 VM 属于相同的大小系列即可。 

类型  为硬件代系。 相同 VM 系列的不同硬件类型来自不同的 CPU 供应商，具有不同的 CPU 代系和核心数。 

大小和硬件类型因区域而异。 请参阅主机[定价页](https://www.azure.cn/pricing/details/virtual-machines/)来了解详细信息。

## <a name="host-life-cycle"></a>主机生命周期

Azure 将会监视和管理主机的运行状况状态。 查询主机时，将返回以下状态：

| 运行状况状态   | 说明       |
|----------|----------------|
| 主机可用     | 主机不存在已知问题。   |
| 主机正在接受调查  | 主机出现了一些问题，我们正在调查。 这是一种过渡状态，需要 Azure 尝试识别所发现的问题的范围和根本原因。 主机上运行的虚拟机可能受到了影响。 |
| 主机正在等待解除分配   | Azure 无法将主机还原到正常状态，并要求你将虚拟机重新部署到此主机外部。 如果已启用 `autoReplaceOnFailure`，则虚拟机会将服务修复  到正常的硬件上。 否则，虚拟机可能在即将发生故障的主机上运行。|
| 主机已解除分配  | 所有虚拟机都已从主机中删除。 由于硬件已退出轮换，因此不再向你收取此主机的费用。   |

<!-- Update_Description: new article about virtual machines common dedicated hosts -->
<!--NEW.date: 04/27/2020-->