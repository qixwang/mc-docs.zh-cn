---
title: 排查 Azure 专用链接的连接问题
description: 有关诊断专用链接连接的分步指导
services: private-link
documentationcenter: na
author: rockboyfor
manager: digimobile
editor: ''
ms.service: private-link
ms.devlang: na
ms.topic: troubleshooting
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
origin.date: 01/31/2020
ms.date: 02/24/2020
ms.author: v-yeche
ms.openlocfilehash: af7dbb5cf75ac25ba09921f36daf1842e378f537
ms.sourcegitcommit: afe972418a883551e36ede8deae32ba6528fb8dc
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/21/2020
ms.locfileid: "77541089"
---
# <a name="troubleshoot-private-link-service-connectivity-problems"></a>排查专用链接服务连接问题

本指南提供有关验证和诊断专用链接服务连接设置的分步指导。 

使用 Azure 专用链接，可以通过虚拟网络中的专用终结点访问 Azure PaaS 服务（例如，Azure 存储、Azure Cosmos DB 和 SQL 数据库）和 Azure 托管的客户服务/合作伙伴服务。 虚拟网络与服务之间的流量将通过 Azure 主干网络，因此不会从公共 Internet 泄露。 你还可以在虚拟网络 (VNet) 中创建自己的专用链接服务，并将其专门提供给自己的客户。 

可为 Azure 标准负载均衡器后面的服务启用专用链接访问。 服务的使用者可在其虚拟网络中创建专用终结点，并将此终结点映射到此服务，然后以私密方式访问此服务。

下面是专用链接服务可用的连接方案
- 同一区域中的虚拟网络 
- 区域对等互连的虚拟网络
- 全球对等互连的虚拟网络
- 基于 VPN 或 Express Route 线路的客户本地连接

## <a name="deployment-troubleshooting"></a>部署故障排除

无法从所选子网中为专用链接服务选择源 IP 地址时，请查看[在专用链接服务中禁用网络策略](/private-link/disable-private-link-service-network-policy)中的故障排除信息。

对于从中选择了源 IP 地址的子网，请确保禁用 **privateLinkServiceNetworkPolicies** 设置。

## <a name="diagnosing-connectivity-problems"></a>诊断连接问题

如果在设置专用链接时遇到连接问题，请执行下面所列的步骤，确保所有一般配置符合预期。

1. 通过浏览资源查看专用链接服务的配置 

    a) 转到“专用链接中心” 

      ![专用链接中心](./media/private-link-tsg/private-link-center.png)

    b) 在左侧导航窗格中选择“专用链接服务” 

      ![专用链接服务](./media/private-link-tsg/private-link-service.png)

    c) 筛选并选择要诊断的专用链接服务

    d) 查看专用终结点连接
     - 确保从中查找连接的专用终结点的连接状态列为“已批准”。  
     - 如果状态为“挂起”，请选择此连接并批准它。  

       ![专用终结点连接](./media/private-link-tsg/pls-private-endpoint-connections.png)

     - 单击相应的名称，导航到从中建立连接的专用终结点。 确保连接状态显示为“已批准”。 

       ![专用终结点连接概述](./media/private-link-tsg/pls-private-endpoint-overview.png)

     - 两端都获得批准后，再次尝试连接。

    e) 查看“概述”选项卡中的“别名”，以及“属性”选项卡中的“资源 ID”   
     - 确保该“别名”/“资源 ID”与用于创建此服务的专用终结点的“别名”/“资源 ID”相匹配。   

       ![验证别名](./media/private-link-tsg/pls-overview-pane-alias.png)

       ![验证资源 ID](./media/private-link-tsg/pls-properties-pane-resourceid.png)

    f) 查看可见性（“概述”部分）信息
     - 确保你的订阅属于“可见性”范围 

       ![验证可见性](./media/private-link-tsg/pls-overview-pane-visibility.png)

    g) 查看负载均衡器（“概述”）信息
     - 可以单击负载均衡器链接导航到负载均衡器

       ![验证负载均衡器](./media/private-link-tsg/pls-overview-pane-ilb.png)

     - 确保按预期方式配置了负载均衡器设置
       - 查看前端 IP 配置
       - 查看后端池
       - 查看负载均衡规则

       ![验证负载均衡器属性](./media/private-link-tsg/pls-ilb-properties.png)

     - 确保负载均衡器按上述设置正常工作
       - 在除提供负载均衡器后端池的子网以外的其他任何子网中选择一个 VM
       - 尝试从上述 VM 访问负载均衡器前端
       - 如果根据负载均衡规则连接到了后端池，则表示负载均衡器正常运行
       - 还可以通过 Azure Monitor 查看负载均衡器指标，以确定数据是否通过负载均衡器流动

2. 使用 [**Azure Monitor**](/azure-monitor/overview) 查看数据是否流动

    a) 在“专用链接服务”资源中选择“指标” 
     - 选择“传入字节”或“传出字节”  
     - 查看在尝试连接专用链接服务时数据是否流动。 预期会有大约 10 分钟的延迟。

       ![验证专用链接服务指标](./media/private-link-tsg/pls-metrics.png)

3. 如果问题仍未解决，连接问题仍然存在，请联系 [Azure 支持](https://support.azure.cn/support/support-azure/)团队。 

## <a name="next-steps"></a>后续步骤

 * [创建专用链接服务 (CLI)](/private-link/create-private-link-service-cli)

 * [专用终结点故障排除指南](troubleshoot-private-endpoint-connectivity.md)

<!-- Update_Description: new article about troubleshoot private link connectivity -->
<!--NEW.date: 02/24/2020-->