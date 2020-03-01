---
title: 排查 Azure 专用终结点连接问题
description: 有关诊断专用终结点连接的分步指导
services: private-endpoint
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
ms.openlocfilehash: 194d21f49473189cd6719c8737fe428132517921
ms.sourcegitcommit: afe972418a883551e36ede8deae32ba6528fb8dc
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/21/2020
ms.locfileid: "77541091"
---
# <a name="troubleshoot-private-endpoint-connectivity-problems"></a>排查专用终结点连接问题

本指南提供有关验证和诊断专用终结点连接设置的分步指导。 

Azure 专用终结点是一个网络接口，可以通过私密且安全的方式连接到专用链接服务。 此解决方案提供从虚拟网络到 Azure 服务资源的专用连接，帮助保护 Azure 中的工作负荷。 这样，就可以有效地将这些服务接入虚拟网络。 

下面是专用终结点可用的连接方案 
- 同一区域中的虚拟网络 
- 区域对等互连的虚拟网络
- 全球对等互连的虚拟网络
- 基于 VPN 或 Express Route 线路的客户本地连接

## <a name="diagnosing-connectivity-problems"></a>诊断连接问题 
执行下面所列的步骤，确保所有一般配置都符合预期，以解决专用终结点的连接问题。

1. 通过浏览资源查看专用终结点配置 

    a) 转到“专用链接中心” 

      ![专用链接中心](./media/private-endpoint-tsg/private-link-center.png)

    b) 在左侧导航窗格中选择“专用终结点”

      ![专用终结点](./media/private-endpoint-tsg/private-endpoints.png)

    c) 筛选并选择要诊断的专用终结点

    d) 查看虚拟网络和 DNS 信息

     - 验证连接状态是否为“已批准” 
     - 确保 VM 已连接到托管专用终结点的 VNet
     - 已分配 FQDN 信息（复制）和专用 IP 地址

       ![VNet 和 DNS 配置](./media/private-endpoint-tsg/vnet-dns-configuration.png)    

2. 使用 [**Azure Monitor**](/azure-monitor/overview) 查看数据是否流动

    a) 在“专用终结点”资源中，选择“监视” 
     - 选择传入数据或传出数据，查看在尝试连接到专用终结点时数据是否流动。 预期会有大约 10 分钟的延迟。

       ![验证专用终结点遥测](./media/private-endpoint-tsg/private-endpoint-monitor.png)

3. 使用**网络观察程序**中的 VM 连接故障排除工具

    a) 选择客户端 VM

    b) 依次选择“连接故障排除”部分、“出站连接”选项卡  

      ![网络观察程序 - 测试出站连接](./media/private-endpoint-tsg/network-watcher-outbound-connection.png)

    c) 选择“使用网络观察程序进行详细的连接跟踪” 

      ![网络观察程序 - 连接故障排除](./media/private-endpoint-tsg/network-watcher-connection-troubleshoot.png)

    d) 选择“按 FQDN 进行测试” 
     - 从“专用终结点”资源粘贴 FQDN
     - 提供端口（对于 Azure 存储或 COSMOS，端口通常为 443；对于 SQL 服务，端口通常为 1336） 

    e) 单击“测试”并验证测试结果 

      ![网络观察程序 - 测试结果](./media/private-endpoint-tsg/network-watcher-test-results.png)

4. 测试结果中的 DNS 解析必须包含分配给专用终结点的相同专用 IP 地址

    a) 如果 DNS 设置不正确，请执行以下操作
     - 使用专用区域： 
       - 确保客户端 VM VNet 与专用区域相关联
       - 检查专用 DNS 区域记录是否存在，如果不存在，请创建该记录

     - 使用自定义 DNS：
       - 检查客户 DNS 设置，并验证 DNS 配置是否正确。
       有关指导，请参阅[专用终结点概述 - DNS 配置](/private-link/private-endpoint-overview#dns-configuration)。

    b) 如果连接因 NSG/UDR 而失败
     - 检查 NSG 出站规则，并创建相应的出站规则来允许流量

       ![NSG 出站规则](./media/private-endpoint-tsg/nsg-outbound-rules.png)

5. 如果连接的验证结果符合预期，则连接问题可能与应用层的其他方面（例如机密、令牌和密码）相关。
   - 在这种情况下，请检查与专用终结点关联的专用链接资源的配置。 请参阅[专用链接故障排除指南](troubleshoot-private-link-connectivity.md)。 

6. 如果问题仍未解决，连接问题仍然存在，请联系 [Azure 支持](https://support.azure.cn/support/support-azure/)团队。 

## <a name="next-steps"></a>后续步骤

 * [在更新的子网上创建专用终结点（Azure 门户）](/private-link/create-private-endpoint-portal)

 * [专用链接故障排除指南](troubleshoot-private-link-connectivity.md)


<!-- Update_Description: new article about troubleshoot private endpoint connectivity -->
<!--NEW.date: 02/24/2020-->