---
title: Azure 虚拟机规模集自动缩放概述 | Microsoft Docs
description: 了解可以通过哪些不同的方法，根据性能或固定的计划自动缩放 Azure 虚拟机规模集
services: virtual-machine-scale-sets
documentationcenter: ''
author: cynthn
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: d29a3385-179e-4331-a315-daa7ea5701df
ms.service: virtual-machine-scale-sets
ms.workload: infrastructure-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
origin.date: 05/29/2018
ms.date: 02/12/2019
ms.author: v-junlch
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: f789d5ab7b163c0f21b2c700268a9d480322959c
ms.sourcegitcommit: c1ba5a62f30ac0a3acb337fb77431de6493e6096
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2020
ms.locfileid: "63850667"
---
# <a name="overview-of-autoscale-with-azure-virtual-machine-scale-sets"></a>Azure 虚拟机规模集自动缩放概述
Azure 虚拟机规模集可以自动增加或减少运行应用程序的 VM 实例数。 这种自动且弹性的行为可以减少监视和优化应用程序性能所需的管理开销。 创建规则，用于定义提供正面客户体验而可接受的性能。 如果满足定义的这些阈值，自动缩放规则会采取措施来调整规模集的容量。 还可以计划事件，以便在固定的时间自动增加或减少规模集的容量。 本文概述所提供的性能指标，以及自动缩放可以执行的操作。


## <a name="benefits-of-autoscale"></a>自动缩放的好处
如果应用程序需求提高，规模集中 VM 实例上的负载将会增大。 如果这种负载增大持续稳定，而不只是短暂的需求，那么可以配置自动缩放规则来增加规模集中的 VM 实例数。

创建这些 VM 实例并部署应用程序后，规模集会开始通过负载均衡器将流量分配到这些实例和应用程序。 可以控制要监视的指标（例如 CPU 或内存）、应用程序负载必须满足给定阈值的时间，以及要添加到规模集的 VM 实例数。

在夜间或周末，应用程序需求可能会降低。 如果这种负载降低在一段时间内持续稳定，可以配置自动缩放规则来减少规模集中的 VM 实例数。 这种横向缩减操作可以减少运行规模集所需的成本，因为只运行满足当前需求所需的实例数。


## <a name="use-host-based-metrics"></a>使用基于主机的指标
可以创建内置于 VM 实例提供的主机指标中的自动缩放规则。 通过主机指标可以洞察规模集中 VM 实例的性能，而无需安装或配置附加的代理和数据集合。 使用这些指标的自动缩放规则可以增加或减少 VM 实例数目来响应 CPU 使用率、内存需求或磁盘访问。

可通过以下工具之一创建使用基于主机的指标的自动缩放规则：

- [Azure 门户](virtual-machine-scale-sets-autoscale-portal.md)
- [Azure PowerShell](tutorial-autoscale-powershell.md)
- [Azure CLI](tutorial-autoscale-cli.md)
- [Azure 模板](tutorial-autoscale-template.md)

若要创建使用更详细性能指标的自动缩放规则，可在 VM 实例上[安装并配置 Azure 诊断扩展](#in-guest-vm-metrics-with-the-azure-diagnostics-extension)。

使用基于主机的指标和 Azure 诊断扩展中的来宾内 VM 指标的自动缩放规则可使用以下配置设置。

### <a name="metric-sources"></a>指标源
自动缩放规则可以使用来自以下源之一的指标：

| 指标源        | 用例                                                                                                                     |
|----------------------|------------------------------------------------------------------------------------------------------------------------------|
| 当前规模集    | 适用于无需附加代理即可安装或配置的基于主机的指标。                                  |
| 存储帐户      | Azure 诊断扩展会将性能指标写入 Azure 存储，然后，可使用 Azure 存储来触发自动缩放规则。 |
| 服务总线队列    | 应用程序或其他组件可将 Azure 服务总线队列中的消息传输到触发器规则。                   |


### <a name="autoscale-rule-criteria"></a>自动缩放规则条件
创建自动缩放规则时，可使用以下基于主机的指标。 如果使用 Azure 诊断扩展，请定义要通过自动缩放规则监视和使用的指标。

| 指标名称               |
|---------------------------|
| CPU 百分比            |
| 网络传入                |
| 网络传出               |
| 磁盘读取字节数           |
| 磁盘写入字节数          |
| 磁盘读取操作次数/秒  |
| 磁盘写入操作次数/秒 |
| 剩余 CPU 信用额度     |
| 已用 CPU 信用额度      |

创建用于监视给定指标的自动缩放规则时，规则会检查以下指标聚合操作之一：

| 聚合类型 |
|------------------|
| 平均值          |
| 最小值          |
| 最大值          |
| 总计            |
| 最后一个             |
| Count            |

使用以下运算符之一将指标与定义的阈值进行比较时，会触发自动缩放规则：

| 操作员                 |
|--------------------------|
| 大于             |
| 大于或等于 |
| 小于                |
| 小于或等于    |
| 等于                 |
| 不等于             |


### <a name="actions-when-rules-trigger"></a>规则触发时的操作
自动缩放规则触发时，规模集可通过以下方式之一自动缩放：

| 缩放操作     | 用例                                                                                                                               |
|---------------------|----------------------------------------------------------------------------------------------------------------------------------------|
| 增加计数   | 要创建的 VM 实例的固定数目。 在 VM 数量较少的规模集中很有用。                                           |
| 增加百分比 | VM 实例的增加数量百分比。 非常适用于较大的规模集，在其中按固定数量增加 VM 无法明显提高性能。 |
| 增加计数至   | 创建任意数量的 VM 实例来达到所需的最大数量。                                                            |
| 减少计数   | 要删除的 VM 实例的固定数目。 在 VM 数量较少的规模集中很有用。                                           |
| 降低百分比 | VM 实例的减少数量百分比。 非常适用于较大的规模集，在其中按固定数量增加 VM 无法明显降低资源消耗和成本。 |
| 减少计数至   | 删除任意数量的 VM 实例来达到所需的最小数量。                                                            |


## <a name="in-guest-vm-metrics-with-the-azure-diagnostics-extension"></a>Azure 诊断扩展中的来宾内 VM 指标
Azure 诊断扩展是在 VM 实例中运行的代理。 该代理可监视性能指标并将其保存到 Azure 存储。 这些性能指标包含有关 VM 状态的更详细信息，例如磁盘的 *AverageReadTime*，或 CPU 的 *PercentIdleTime*。 可以根据对 VM 性能的更详细认知，而不仅仅是 CPU 使用率或内存消耗量的百分比，来创建自动缩放规则。

若要使用 Azure 诊断扩展，必须为 VM 实例创建 Azure 存储帐户，安装 Azure 诊断代理，然后将 VM 配置为向存储帐户流式传输特定的性能计数器。

有关详细信息，请参阅有关如何在 [Linux VM](../virtual-machines/extensions/diagnostics-linux.md) 或 [Windows VM](../virtual-machines/extensions/diagnostics-windows.md) 上启用 Azure 诊断扩展的文章。



## <a name="scheduled-autoscale"></a>计划的自动缩放
还可以根据计划创建自动缩放规则。 使用这些基于计划的规则可在固定的时间自动缩放 VM 实例的数目。 使用基于性能的规则时，在触发自动缩放规则和预配新的 VM 实例之前，应用程序的性能可能会受影响。 如果能够预见到这种需求，可以预配更多的 VM 实例，并使其随时可用于满足更多的客户用途和应用程序的需求。

以下示例是能够从基于计划的自动缩放规则中受益的情景：

- 在工作日开始且客户需求增大时自动增加 VM 实例数。 在工作日结束且应用程序用量较低时，在夜间自动横向缩减 VM 实例数以最大程度地降低资源成本。
- 如果某个部门在月份或财政周期的某些时段重度使用某个应用程序，则自动增加 VM 实例数来适应额外的需求。
- 开展市场营销活动、促销或假日销售时，可以在预测客户需求之前自动增加 VM 实例数。 


## <a name="next-steps"></a>后续步骤
可通过以下工具之一创建使用基于主机的指标的自动缩放规则：

- [Azure PowerShell](tutorial-autoscale-powershell.md)
- [Azure CLI](tutorial-autoscale-cli.md)
- [Azure 模板](tutorial-autoscale-template.md)

有关如何管理 VM 实例的信息，请参阅[使用 Azure PowerShell 管理虚拟机规模集](virtual-machine-scale-sets-windows-manage.md)。

<!-- Update_Description: update metedata properties -->