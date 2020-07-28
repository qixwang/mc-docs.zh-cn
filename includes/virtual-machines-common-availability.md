---
title: include 文件
description: include 文件
services: virtual-machines
author: rockboyfor
ms.service: virtual-machines
ms.topic: include
origin.date: 06/30/2020
ms.date: 07/27/2020
ms.testscope: no
ms.testdate: ''
ms.author: v-yeche
ms.custom: include file
ms.openlocfilehash: cef5fe7350a7646a771d259fed3883ee1e3eaf00
ms.sourcegitcommit: 2b78a930265d5f0335a55f5d857643d265a0f3ba
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/28/2020
ms.locfileid: "87260186"
---
本文概述了 Azure 虚拟机 (VM) 的可用性功能。

## <a name="high-availability"></a>高可用性

工作负荷通常分布在不同的虚拟机上，以获得高吞吐量、高性能并实现冗余，防止 VM 因更新或其他事件而受影响。 

Azure 提供了几个选项来实现高可用性。 首先，让我们谈一谈基本构造。 

<!--Not Available on ### Availability zones-->

### <a name="fault-domains"></a>容错域

容错域是共享公用电源和网络交换机的基础硬件逻辑组，类似于本地数据中心内的机架。 

### <a name="update-domains"></a>更新域

更新域是可以同时维护或重新启动的基础硬件逻辑组。 

Azure 平台进行定期维护时，此方法可确保至少有一个应用程序实例始终保持运行状态。 在维护期间，更新域的重启顺序可能不会按序进行，但一次只重启一个更新域。

## <a name="virtual-machines-scale-sets"></a>虚拟机规模集 

使用 Azure 虚拟机规模集可以创建并管理一组负载均衡的 VM。 可以根据需求或定义的计划自动增减 VM 实例的数目。 规模集为应用程序提供高可用性，用于集中管理、配置和更新许多 VM。 建议在一个规模集内创建两个或多个 VM，使应用程序高度可用，并满足 [99.95% Azure SLA](https://www.azure.cn/support/sla/virtual-machines/) 的要求。 规模集本身是免费的，你只需为创建的每个 VM 实例付费。 当单个 VM 使用 [Azure 高级 SSD](/virtual-machines/windows/disks-types#premium-ssd) 时，Azure SLA 适用于计划外维护事件。 规模集内的虚拟机可以跨多个更新域和容错域部署，以最大程度地提高因数据中心中断、计划内或计划外维护事件而导致停机时的可用性和复原能力。

<!--Not Available on Availablity zones-->
<!--Not Available on **Fault domains and update domains**-->
<!--Not Available on  Virtual machines in a scale set can also be deployed into a single Availability zone, or regionally. Availability zone deployment options may differ based on the orchestration mode.-->

## <a name="availability-sets"></a>可用性集

可用性集是数据中心内的 VM 的逻辑分组，可让 Azure 了解应用程序的构建方式，以便提供冗余和可用性。 建议在可用性集内创建两个或多个 VM，提供高度可用的应用程序，并满足 [99.95% Azure SLA](https://www.azure.cn/support/sla/virtual-machines/) 的要求。 可用性集本身是免费的，只需为创建的每个 VM 实例付费。 当单个 VM 使用 [Azure 高级 SSD](../articles/virtual-machines/windows/disks-types.md#premium-ssd) 时，Azure SLA 适用于计划外维护事件。

在可用性集中，VM 自动分布到这些容错域中。 此方法可限制潜在物理硬件故障、网络中断或断电的影响。

对于使用 [Azure 托管磁盘](../articles/virtual-machines/windows/faq-for-disks.md)的 VM，在使用托管可用性集时，VM 与托管磁盘容错域一致。 该一致性可确保附加到 VM 的所有托管磁盘都在同一托管磁盘容错域内。 

在托管可用性集中，只能创建带托管磁盘的 VM。 托管磁盘容错域的数目因区域而异 - 每个区域两个或三个托管磁盘容错域。 可以阅读有关这些适用于 [Linux VM](../articles/virtual-machines/linux/manage-availability.md?#use-managed-disks-for-vms-in-an-availability-set) 或 [Windows VM](../articles/virtual-machines/windows/manage-availability.md?#use-managed-disks-for-vms-in-an-availability-set) 的托管磁盘容错域的详细信息。

![托管可用性集](./media/virtual-machines-common-manage-availability/md-fd-updated.png)

可用性集中的 VM 也会自动分布到更新域中。 

![可用性集](./media/virtual-machines-common-manage-availability/ud-fd-configuration.png)

<!--Not Available on ## Next steps-->
<!--Not Available on [Azure availability best practices](https://docs.microsoft.com/azure/architecture/checklist/resiliency-per-service)-->

<!-- Update_Description: update meta properties, wording update, update link -->