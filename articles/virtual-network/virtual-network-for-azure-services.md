---
title: Azure 服务的虚拟网络
titlesuffix: Azure Virtual Network
description: 了解向虚拟网络部署资源的好处。 虚拟网络中的资源可以彼此信，也可与本地资源通信，而无需遍历 Internet 的流量。
services: virtual-network
documentationcenter: na
author: rockboyfor
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
origin.date: 09/25/2017
ms.date: 02/24/2020
ms.author: v-yeche
ms.reviewer: kumud
ms.openlocfilehash: 45a51d07afac86d7906bfc5db266affd27b990e2
ms.sourcegitcommit: c1ba5a62f30ac0a3acb337fb77431de6493e6096
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2020
ms.locfileid: "79292296"
---
# <a name="virtual-network-integration-for-azure-services"></a>Azure 服务的虚拟网络集成

通过将 Azure 服务集成到 Azure 虚拟网络，可从虚拟机或虚拟网络中的计算资源私密访问服务。
可通过以下选项在虚拟网络中集成 Azure 服务：
- 将服务的专用实例部署到虚拟网络中。 随后即可在虚拟网络内以及从本地网络私密访问这些服务。

    <!--Not Available on [Private Link](../private-link/private-link-overview.md)-->
    
也可使用公共终结点来访问服务，只需通过[服务终结点](virtual-network-service-endpoints-overview.md)将虚拟网络扩展到服务即可。 服务终结点可使服务资源在虚拟网络中得到保护。

## <a name="deploy-azure-services-into-virtual-networks"></a>将 Azure 服务部署到虚拟网络

在[虚拟网络](virtual-networks-overview.md)中部署专用 Azure 服务时，可通过专用 IP 地址与服务资源进行私密通信。

![虚拟网络中部署的服务](./media/virtual-network-for-azure-services/deploy-service-into-vnet.png)

在虚拟网络中部署服务可提供以下功能：

- 虚拟网络内的资源可以通过专用 IP 地址彼此进行私密通信。 例如，在虚拟网络中，在虚拟机上运行的 HDInsight 与 SQL Server 之间可直接传输数据。
- 本地资源可通过[站点到站点 VPN（VPN 网关）](../vpn-gateway/vpn-gateway-about-vpngateways.md?toc=%2fvirtual-network%2ftoc.json#s2smulti)或 [ExpressRoute](../expressroute/expressroute-introduction.md?toc=%2fvirtual-network%2ftoc.json) 使用专用 IP 地址访问虚拟网络中的资源。
- 虚拟网络可使用专用 IP 地址进行[对等互连](virtual-network-peering-overview.md)，实现虚拟网络中资源之间的彼此通信。
- 虚拟网络中的服务实例通常由 Azure 服务完全托管。 这包括监视资源的运行状况并根据负载进行缩放。
- 服务实例部署在虚拟网络的子网中。 根据服务提供的指南，必须通过[网络安全组](security-overview.md#network-security-groups)对子网开放入站和出站网络访问。
- 某些服务还会对它们能够部署到其中的子网施加限制，限制策略、路由的应用，或者要求将 VM 和服务资源组合到同一子网中。 请查看每项服务，了解这些具体限制，因为它们会随时间而变化。 应用服务是此类服务的一个示例。 
    
    <!--Not Available on Azure NetApp Files, Dedicated HSM, Azure Container Instances-->
    
- （可选）服务可能需要一个[委派子网](virtual-network-manage-subnet.md#add-a-subnet)作为显式标识符，用于表示子网可承载特定服务。 服务可以通过委托获得显式权限，可以在委托的子网中创建服务专属资源。
- 如需 REST API 响应的示例，请参阅[包含委托子网的虚拟网络](https://docs.microsoft.com/rest/api/virtualnetwork/virtualnetworks/get#get-virtual-network-with-a-delegated-subnet)。 可以通过[可用委托](https://docs.microsoft.com/rest/api/virtualnetwork/availabledelegations/list) API 获得一个内容广泛的列表，其中包含的服务使用委托子网模型。

### <a name="services-that-can-be-deployed-into-a-virtual-network"></a>可部署到虚拟网络中的服务

|类别|服务| 专用¹ 子网
|-|-|-|
| 计算 | 虚拟机：[Windows](../virtual-machines/linux/infrastructure-networking-guidelines.md?toc=%2fvirtual-network%2ftoc.json) 或 [Linux](../virtual-machines/windows/infrastructure-networking-guidelines.md?toc=%2fvirtual-network%2ftoc.json) <br/>[虚拟机规模集](../virtual-machine-scale-sets/virtual-machine-scale-sets-mvss-existing-vnet.md?toc=%2fvirtual-network%2ftoc.json)<br/>[云服务](https://msdn.microsoft.com/library/azure/jj156091)：仅限虚拟网络（经典）<br/> [Azure Batch](../batch/batch-api-basics.md?toc=%2fvirtual-network%2ftoc.json#virtual-network-vnet-and-firewall-configuration)| 否 <br/> 否 <br/> 否 <br/> 否²
| 网络 | [应用程序网关 - WAF](../application-gateway/application-gateway-ilb-arm.md?toc=%2fvirtual-network%2ftoc.json)<br/>[VPN 网关](../vpn-gateway/vpn-gateway-about-vpngateways.md?toc=%2fvirtual-network%2ftoc.json)<br/>[Azure 防火墙](../firewall/overview.md?toc=%2fvirtual-network%2ftoc.json) <br/>[网络虚拟设备](https://docs.microsoft.com/windows-server/networking/sdn/manage/use-network-virtual-appliances-on-a-vn) | 是 <br/> 是 <br/> 是 <br/> 否
|data|[RedisCache](../azure-cache-for-redis/cache-how-to-premium-vnet.md?toc=%2fvirtual-network%2ftoc.json)<br/>[Azure SQL 数据库托管实例](../sql-database/sql-database-managed-instance-connectivity-architecture.md?toc=%2fvirtual-network%2ftoc.json)| 是 <br/> 是 <br/> 
| Analytics | [Azure HDInsight](../hdinsight/hdinsight-extend-hadoop-virtual-network.md?toc=%2fvirtual-network%2ftoc.json)<br/> | 否² <br/> 
| 容器 | [Azure Kubernetes 服务 (AKS)](../aks/concepts-network.md?toc=%2fvirtual-network%2ftoc.json)<br/>|否²<br/>
| Web | [API 管理](../api-management/api-management-using-with-vnet.md?toc=%2fvirtual-network%2ftoc.json)<br/>[应用服务环境](../app-service/web-sites-integrate-with-vnet.md?toc=%2fvirtual-network%2ftoc.json)<br/>|是 <br/> 是 <br/>
|||

¹ “专用”意味着只能将特定于服务的资源部署在此子网中，并且不能将其与客户 VM/VMSS 组合使用 <br/> ² 建议的最佳做法是将这些服务放在专用子网中，但不是服务强加的强制性要求。

<!-- Not Available on [Azure Databricks](../azure-databricks/what-is-azure-databricks.md?toc=%2fvirtual-network%2ftoc.json)-->
<!-- Not Available on | Identity | [Azure Active Directory Domain Services](../active-directory-domain-services/active-directory-ds-getting-started-vnet.md?toc=%2fvirtual-network%2ftoc.json)-->
<!-- Not Available on | Containers | [Azure Container Instance (ACI)](https://www.aka.ms/acivnet)<br/>[Azure Container Service Engine](https://github.com/Azure/acs-engine) with Azure Virtual Network CNI [plug-in](https://github.com/Azure/acs-engine/tree/master/examples/vnet)||-->
<!-- Not Available on [Azure Logic Apps](../logic-apps/connect-virtual-network-vnet-isolated-environment-overview.md?toc=%2fvirtual-network%2ftoc.json)<br/>-->
<!-- Not Available on | Hosted | [Azure Dedicated HSM](../dedicated-hsm/index.yml?toc=%2fvirtual-network%2ftoc.json)<br/>[Azure NetApp Files](../azure-netapp-files/azure-netapp-files-introduction.md?toc=%2fvirtual-network%2ftoc.json)<br/>|Yes <br/> Yes <br/>-->

<!-- Update_Description: update meta properties, wording update, update link -->