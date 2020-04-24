---
title: Azure 中 VM 的维护和更新 | Azure
description: 概述在 Azure 中运行的虚拟机的维护和更新。
services: virtual-machines
documentationcenter: ''
author: rockboyfor
editor: ''
tags: azure-resource-manager,azure-service-management
ms.service: virtual-machines
ms.workload: infrastructure-services
ms.topic: article
origin.date: 11/18/2019
ms.date: 02/17/2020
ms.author: v-yeche
ms.openlocfilehash: de2b45c377fde59510ed91f414c951938944b75c
ms.sourcegitcommit: c1ba5a62f30ac0a3acb337fb77431de6493e6096
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2020
ms.locfileid: "77429969"
---
# <a name="maintenance-for-virtual-machines-in-azure"></a>Azure 中虚拟机的维护

Azure 定期更新其平台，以提高虚拟机的主机基础结构的可靠性、性能及安全性。 此类更新的目的包括修补托管环境中的软件组件、升级网络组件以及硬件解除授权，等等。 

更新极少会影响到托管的 VM。 如果更新确实会造成影响，Azure 将选择影响最小的方法进行更新：

- 如果更新不需要重新启动，则会在更新主机时暂停 VM，或者将 VM 实时迁移到已更新的主机。 
- 如果维护需要重新启动，则你会收到计划内维护通知。 Azure 还会提供一个时间范围，方便你在合适的时间自行启动维护。 除非紧急执行维护，否则自我维护时限通常为 30 天。 Azure 正在投资相关技术，减少进行计划内平台维护时必须重新启动 VM 的情况。 有关管理计划内维护的说明，请参阅“使用 Azure [CLI](maintenance-notifications-cli.md)、[PowerShell](maintenance-notifications-powershell.md) 或[门户](maintenance-notifications-portal.md)处理计划内维护通知”。

本页介绍 Azure 如何执行上述两种类型的维护。 有关非计划事件（服务中断）的详细信息，请参阅 [管理 Windows VM 的可用性](./windows/manage-availability.md)或适用于 [Linux](./linux/manage-availability.md) 的相应文章。

在 VM 中，可以使用适用于 [Windows](./windows/scheduled-events.md) 或 [Linux](./linux/scheduled-events.md) 的计划事件获取有关即将进行维护的通知。

## <a name="maintenance-that-doesnt-require-a-reboot"></a>不需要重新启动的维护

大多数平台更新不会影响客户 VM。 如果更新不可避免地造成影响，Azure 会选择对客户 VM 影响最小的更新机制。 

大多数有影响的维护只会导致 VM 暂停 10 秒以下。 在某些情况下，Azure 使用内存预留维护机制。 这些机制最长会将 VM 暂停 30 秒，并在 RAM 中预留内存。 VM 随后会恢复，其时钟会自动同步。 

内存预留维护适用于 90% 以上的 Azure VM。 它不适用于 M、N 和 H 系列。 Azure 逐渐趋向于使用实时迁移技术并改进内存保留维护机制，目的是缩短暂停时间。  

<!--Not Available on G -->

无需重新启动的维护操作每次应用到一个容错域。 如果出现任何警告运行状况信号，这些操作将会停止。 

此类更新可能会影响某些应用程序。 将 VM 实时迁移到另一主机时，某些敏感的工作负荷可能会有几分钟出现导致 VM 暂停的性能略微下降情况。 若要准备 VM 维护并减小 Azure 维护期间造成的影响，请尝试使用此类应用程序的适用于 [Windows](./windows/scheduled-events.md) 或 [Linux](./linux/scheduled-events.md) 的计划事件。 

<!--Not Available on [Azure Dedicated Hosts](./linux/dedicated-hosts.md)-->
<!--Not Available on [isolated VM](../security/fundamentals/isolation-choices.md)-->

### <a name="live-migration"></a>实时迁移

实时迁移是一个无需重新启动的操作，可为 VM 预留内存。 它会导致暂停或冻结，但持续时间通常不超过 5 秒。 除 M、N 和 H 系列以外其他所有基础结构即服务 (IaaS) VM 都符合实时迁移的条件。 符合条件的 VM 代表了可部署到 Azure 机群的 90% 以上的 IaaS VM。 

<!--Not Available on G -->

出现以下情况时，Azure 平台将启动实时迁移：
- 计划内维护
- 硬件故障
- 分配优化

某些计划内维护方案使用实时迁移，你可以使用计划事件来提前了解实时迁移操作何时开始。

当 Azure 机器学习算法预测即将发生的硬件故障，或者当你想要优化 VM 分配时，也可以使用实时迁移来移动 VM。 有关用于检测降级硬件实例的预测建模的详细信息，请参阅[使用预测性机器学习和实时迁移提高 Azure VM 的复原能力](https://azure.microsoft.com/blog/improving-azure-virtual-machine-resiliency-with-predictive-ml-and-live-migration/?WT.mc_id=thomasmaurer-blog-thmaure)。 实时迁移通知将显示在 Azure 门户上的“监视”和“服务运行状况”日志中以及“计划事件”中（如果使用这些服务）。

## <a name="maintenance-that-requires-a-reboot"></a>需要重新启动的维护

在极少数情况下，VM 需重新启动以进行计划内维护，这种情况下系统会提前通知。 计划内维护有两个阶段：自助服务阶段和计划内维护阶段。

自助服务阶段通常持续四周，你将在 VM 上启动维护。  在自助服务期间，可以查询每个 VM 来了解其状态，以及上次维护请求的结果。

启动自助维护时，VM 会重新部署到已更新的某个节点。 由于 VM 重新启动，临时磁盘会丢失，而与虚拟网络接口关联的动态 IP 地址会更新。

如果在自助维护期间出错，操作将会停止，VM 不会更新，而你可以使用相应的选项来重试自助维护。 

自助维护阶段结束时，计划内维护阶段随即开始。  在此阶段，仍可以查询维护阶段，但无法自行启动维护。

有关管理需要重新启动的维护的详细信息，请参阅“使用 Azure [CLI](maintenance-notifications-cli.md)、[PowerShell](maintenance-notifications-powershell.md) 或[门户](maintenance-notifications-portal.md)处理计划内维护通知”  。 

### <a name="availability-considerations-during-scheduled-maintenance"></a>计划内维护期间的可用性注意事项 

如果你决定等到计划内维护阶段开始，为了保持 VM 的最高可用性，应考虑到一些要素。 

#### <a name="paired-regions"></a>配对区域

每个 Azure 区域将与同一邻近地理范围内的另一区域配对。 它们共同构成了一个区域对。 在计划内维护阶段，Azure 只会更新一个区域对中单个区域内的 VM。 例如，更新中国北部的 VM 时，Azure 不会同时更新中国东部的任何 VM。

<!-- Not Available  Understanding how region pairs work can help you better distribute your VMs across regions. -->
<!-- Not Available on  For more information, see [Azure region pairs](/best-practices-availability-paired-regions) -->

#### <a name="availability-sets-and-scale-sets"></a>可用性集和规模集

在 Azure VM 上部署工作负荷时，可以在可用性集中创建 VM，向应用程序提供高可用性  。 使用可用性集可以确保在发生服务中断或需要重新启动的维护事件期间，至少有一个 VM 可用。

在可用性集中，个别 VM 可分布在最多 20 个更新域中。 在计划性维护期间，任何给定时间都只更新一个更新域。 更新域不一定要按顺序更新。 

虚拟机规模集是一种 Azure 计算资源，可用于将一组相同的 VM 作为单个资源进行部署和管理  。 规模集自动跨 UD 进行部署，此类更新域就像可用性集中的 VM 一样。 在计划内维护期间使用规模集时，就像使用可用性集一样，在任意给定时间只会更新一个 UD。

有关设置 VM 以实现高可用性的详细信息，请参阅 [管理 Windows VM 的可用性](./windows/manage-availability.md)或适用于 [Linux](./linux/manage-availability.md) 的相应文章。

<!--Not Available on #### Availability zones-->
## <a name="next-steps"></a>后续步骤 

可以使用 [Azure CLI](maintenance-notifications-cli.md)、[Azure PowerShell](maintenance-notifications-powershell.md) 或[门户](maintenance-notifications-portal.md)来管理计划内维护。
<!--Update_Description: update meta properties, wording update, update link -->