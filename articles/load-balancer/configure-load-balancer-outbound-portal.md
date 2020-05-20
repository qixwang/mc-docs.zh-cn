---
title: 使用 Azure 门户配置负载均衡和出站规则
titleSuffix: Azure Load Balancer
description: 本文介绍如何使用 Azure 门户在标准负载均衡器中配置负载均衡和出站规则。
services: load-balancer
author: WenJason
ms.service: load-balancer
ms.topic: article
origin.date: 09/24/2019
ms.date: 04/06/2020
ms.author: v-jay
ms.openlocfilehash: 6de0bf9bdff424e3a898943457521847d2ad9758
ms.sourcegitcommit: c1ba5a62f30ac0a3acb337fb77431de6493e6096
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2020
ms.locfileid: "80625803"
---
# <a name="configure-load-balancing-and-outbound-rules-in-standard-load-balancer-by-using-the-azure-portal"></a>使用 Azure 门户在标准负载均衡器中配置负载均衡和出站规则

本文介绍如何使用 Azure 门户在标准负载均衡器中配置出站规则。  

负载均衡器资源包含两个前端及其关联的规则。 一个前端用于入站流量，另一个前端用于出站流量。  

每个前端引用一个公共 IP 地址。 在此方案中，入站流量的公共 IP 地址不同于出站流量的地址。   负载均衡规则仅提供入站负载均衡。 出站规则控制 VM 的出站网络地址转换 (NAT)。  

此方案使用两个后端池：一个池用于入站流量，另一个池用于出站流量。 这些池演示功能，并为方案提供灵活性。

如果没有 Azure 订阅，可在开始前创建一个 [1 元人民币试用帐户](https://wd.azure.cn/zh-cn/pricing/1rmb-trial-full)。 

## <a name="sign-in-to-azure"></a>登录 Azure

在 [https://portal.azure.cn](https://portal.azure.cn) 中登录 Azure 门户。

## <a name="create-a-load-balancer"></a>创建负载均衡器

在本部分，你将创建一个负载均衡器用于对虚拟机进行负载均衡。 可以创建公共负载均衡器或内部负载均衡器。 创建公共负载均衡器时，将创建一个新的公共 IP 地址，该地址配置为负载均衡器的前端。 前端默认命名为 LoadBalancerFrontEnd。 

1. 在屏幕的左上方，选择“创建资源” > “网络” > “负载均衡器”。
2. 在“创建负载均衡器”页的“基本信息”选项卡中，输入或选择以下信息：  

    | 设置                 | 值                                              |
    | ---                     | ---                                                |
    | 订阅               | 选择订阅。    |    
    | 资源组         | 选择“新建”并在文本框中键入 **myResourceGroupSLB**。|
    | 名称                   | **myLoadBalancer**                                   |
    | 区域         | 选择“中国东部”。                                         |
    | 类型          | 选择“公共”。                                         |
    | SKU           | 选择“标准”或“基本”。   Azure 建议将“标准”用于生产工作负荷。 |
    | 公共 IP 地址 | 选择“新建”。  若要使用现有的公共 IP，请选择“使用现有项”。   现有公共 IP 必须是标准 SKU。   基本公共 IP 与标准 SKU 负载均衡器不兼容。   |
    | 公共 IP 地址名称              | 在文本框中键入 myPublicIP  。|

3. 对剩余的配置接受默认值。
4. 选择“查看 + 创建” 

    > [!IMPORTANT]
    > 本快速入门的其余部分假定在上述 SKU 选择过程中选择了“标准”  SKU。

5. 在“查看 + 创建”选项卡中，选择“创建”。     

    ![创建标准负载均衡器](./media/quickstart-load-balancer-standard-public-portal/create-standard-load-balancer.png)

## <a name="create-load-balancer-resources"></a>创建负载均衡器资源

在本部分中，将为后端地址池和运行状况探测配置负载均衡器设置，并指定均衡器规则。

### <a name="create-a-backend-pool"></a>创建后端池

后端地址池包含后端池中虚拟 NIC 的 IP 地址。 创建后端地址池 **myBackendPool** 以包含用于对 Internet 流量进行负载均衡的虚拟机。

1. 在左侧菜单中选择“所有服务”，选择“所有资源”，然后在资源列表中选择“myLoadBalancer”。   
2. 在“设置”下，依次选择“后端池”、“添加”。   
3. 在“添加后端池”页上，键入 **myBackendPool** 作为后端池的名称，然后选择“添加”。  

### <a name="create-a-health-probe"></a>创建运行状况探测器

运行状况探测用于监视应用的状态。 运行状况探测器基于其对运行状况检查的响应，在负载均衡器中添加或删除 VM。 创建运行状况探测 **myHealthProbe** 以监视 VM 的运行状况。

1. 在左侧菜单中选择“所有服务”，选择“所有资源”，然后在资源列表中选择“myLoadBalancer”。   
2. 在“设置”下，依次选择“运行状况探测”、“添加”。   
    
    | 设置 | 值 |
    | ------- | ----- |
    | 名称 | 输入 **myHealthProbe**。 |
    | 协议 | 选择“HTTP”。  |
    | 端口 | 输入 **80**。|
    | 时间间隔 | 输入 **15** 作为两次探测尝试之间的**时间间隔**（以秒为单位）。 |
    | 不正常阈值 | 选择“2”，作为将 VM 视为不正常所要达到的**不正常阈值**或连续探测失败次数。|
    | | |
4. 选择“确定”  。

### <a name="create-a-load-balancer-rule"></a>创建负载均衡器规则
负载均衡器规则用于定义将流量分配给 VM 的方式。 

定义：
 - 针对传入流量的前端 IP 配置。
 - 用于接收流量的后端 IP 池。
 - 所需的源和目标端口。 

在下一部分，你将创建：
 - 用于侦听端口 80 的负载均衡器规则 myHTTPRule。 
 - 前端 LoadBalancerFrontEnd。 
 - 后端地址池 myBackEndPool，它也使用端口 80。  

1. 在左侧菜单中选择“所有服务”，选择“所有资源”，然后在资源列表中选择“myLoadBalancer”。   
2. 在“设置”下，依次选择“负载均衡规则”、“添加”。   
3. 使用以下值配置负载均衡规则：
    
    | 设置 | 值 |
    | ------- | ----- |
    | 名称 | 输入 **myHTTPRule**。 |
    | 协议 | 选择“TCP”  。 |
    | 端口 | 输入 **80**。|
    | 后端端口 | 输入 **80**。 |
    | 后端池 | 选择“myBackendPool”。 |
    | 运行状况探测 | 选择“myHealthProbe”。  |
    | 创建隐式出站规则 | 请选择“否”。  在稍后的部分，我们将使用一个专用公共 IP 创建出站规则。 |
4. 将剩余的字段保留默认设置，然后选择“确定”。 

## <a name="create-outbound-rule-configuration"></a>创建出站规则配置
负载均衡器出站规则为后端池中的 VM 配置出站 SNAT。 

### <a name="create-an-outbound-public-ip-address-and-frontend"></a>创建出站公共 IP 地址和前端

1. 在左侧菜单中选择“所有服务”，选择“所有资源”，然后在资源列表中选择“myLoadBalancer”。   

2. 在“设置”下，依次选择“前端 IP 配置”、“添加”。   

3. 使用以下值完成出站前端 IP 配置：

    | 设置 | 值 |
    | ------- | ----- |
    | 名称 | 输入 LoadBalancerFrontEndOutbound。  |
    | IP 版本 | 选择“IPv4”。  |
    | IP 类型 | 选择“IP 地址”。 |
    | 公共 IP 地址 | 选择“新建”。  在“添加公共 IP 地址”中输入 myPublicIPOutbound。    选择“确定”  。 |

4. 选择 **添加** 。

### <a name="create-an-outbound-backend-pool"></a>创建出站后端池

1. 在左侧菜单中选择“所有服务”，选择“所有资源”，然后在资源列表中选择“myLoadBalancer”。   

2. 在“设置”下，依次选择“后端池”、“添加”。   

3. 在“添加后端池”页上，键入 myBackendPoolOutbound 作为后端池的名称，然后选择“添加”。   

### <a name="create-outbound-rule"></a>创建出站规则

1. 在左侧菜单中选择“所有服务”，选择“所有资源”，然后在资源列表中选择“myLoadBalancer”。   

2. 在“设置”下，依次选择“出站规则”、“添加”。   

3. 使用以下值配置出站规则：

    | 设置 | 值 |
    | ------- | ----- |
    | 名称 | 输入 myOutboundRule。  |
    | 前端 IP 地址 | 选择“LoadBalancerFrontEndOutbound”。  |
    | 空闲超时（分钟） | 将滑块移到 **15 分钟位置。|
    | TCP 重置 | 选择“启用”。 |
    | 后端池 | 选择“myBackendPoolOutbound”  |
    | 端口分配 -> 端口分配 | 选择“手动选择出站端口数”  |
    | 出站端口 -> 选择方式 | 选择“每个实例的端口数”  |
    | 出站端口 -> 每个实例的端口数 | 输入 10,000。  |

4. 选择 **添加** 。

## <a name="clean-up-resources"></a>清理资源

若不再需要资源组、负载均衡器以及所有相关资源，请将其删除。 选择包含负载均衡器的资源组 myResourceGroupSLB，然后选择“删除”。  

