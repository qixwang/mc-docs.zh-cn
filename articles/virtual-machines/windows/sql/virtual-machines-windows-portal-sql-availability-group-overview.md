---
title: Azure 虚拟机上的 SQL Server Always On 可用性组简介 | Azure
description: 本文介绍 Azure 虚拟机上的 SQL Server 可用性组。
services: virtual-machines
documentationCenter: na
author: rockboyfor
manager: digimobile
editor: monicar
tags: azure-service-management
ms.assetid: 601eebb1-fc2c-4f5b-9c05-0e6ffd0e5334
ms.service: virtual-machines-sql
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
origin.date: 01/13/2017
ms.date: 10/14/2019
ms.author: v-yeche
ms.openlocfilehash: 4e60aa3b717a05957ce247e9d9f43fa972b73bbf
ms.sourcegitcommit: c1ba5a62f30ac0a3acb337fb77431de6493e6096
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2020
ms.locfileid: "72272672"
---
# <a name="introducing-sql-server-always-on-availability-groups-on-azure-virtual-machines"></a>介绍 Azure 虚拟机上的 SQL Server Always On 可用性组

本文介绍 Azure 虚拟机上的 SQL Server 可用性组。 

Azure 虚拟机上的 Always On 可用性组类似于本地的 Always On 可用性组。 有关详细信息，请参阅 [Always On 可用性组 (SQL Server)](https://msdn.microsoft.com/library/hh510230.aspx)。 

此关系图说明了 Azure 虚拟机中完整 SQL Server 可用性组的部件。

![可用性组](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/00-EndstateSampleNoELB.png)

Azure 虚拟机中可用性组的主要区别是 Azure 虚拟机需要[负载均衡器](../../../load-balancer/load-balancer-overview.md)。 负载均衡器保留可用性组侦听器的 IP 地址。 如果有多个可用性组，则每个组都需要一个侦听器。 一个负载均衡器可以支持多个侦听器。

此外，在 Azure IaaS VM 来宾故障转移群集上，我们建议每个服务器（群集节点）使用一个 NIC 和一个子网。 Azure 网络具有物理冗余，这使得在 Azure IaaS VM 来宾群集上不需要额外的 NIC 和子网。 虽然群集验证报告将发出警告，指出节点只能在单个网络上访问，但在 Azure IaaS VM 来宾故障转移群集上可以安全地忽略此警告。 

若要增加冗余和高可用性，SQL Server VM 应位于相同的[可用性集](virtual-machines-windows-portal-sql-availability-group-prereq.md#create-availability-sets)中。

<!--Not Available on  or different [availability zones](/availability-zones/az-overview)-->

|  | Windows Server 版本 | SQL Server 版本 | SQL Server 发行版 | WSFC 仲裁配置 | 使用多区域进行灾难恢复 | 多子网支持 | 支持现有 AD | 使用具有多个区域的相同区域进行灾难恢复 | Dist-AG 支持，没有 AD 域 | Dist-AG 支持，没有群集 |  
| :------ | :-----| :-----| :-----| :-----| :-----| :-----| :-----| :-----| :-----| :-----|
| [手动](virtual-machines-windows-portal-sql-availability-group-prereq.md) | All | All | All | All | 是 | 是 | 是 | 是 | 是 | 是 |
| &nbsp; | &nbsp; |&nbsp; |&nbsp; |&nbsp; |&nbsp; |&nbsp; |&nbsp; |&nbsp; |&nbsp; |&nbsp; |

准备好在 Azure 虚拟机上生成 SQL Server 可用性组时，请参阅这些教程。

<!--Not Available on ## Manually with Azure CLI-->
<!--Not Available on ## Automatically with Azure Quickstart Templates-->

<!--Not Available on ## Automatically with an Azure Portal Template-->

## <a name="manually-in-azure-portal"></a>在 Azure 门户中手动操作

还可以不使用模板自行创建虚拟机。 首先完成先决条件，然后创建可用性组。 请参阅下列主题： 

- [配置 Azure 虚拟机上的 SQL Server Always On 可用性组的先决条件](virtual-machines-windows-portal-sql-availability-group-prereq.md)

- [创建 Always On 可用性组以提高可用性和灾难恢复能力](virtual-machines-windows-portal-sql-availability-group-tutorial.md)

## <a name="next-steps"></a>后续步骤

[在位于不同区域的 Azure 虚拟机上配置 SQL Server AlwaysOn 可用性组](virtual-machines-windows-portal-sql-availability-group-dr.md)

<!-- Update_Description: wording update, update link -->