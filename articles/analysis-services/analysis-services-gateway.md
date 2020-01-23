---
title: Azure Analysis Services 的本地数据网关 | Azure
description: 如果 Azure 中的 Analysis Services 服务器要连接到本地数据源，则本地网关是必需的。
author: rockboyfor
ms.service: azure-analysis-services
ms.topic: conceptual
origin.date: 10/29/2019
ms.date: 01/20/2020
ms.author: v-yeche
ms.reviewer: minewiskan
ms.openlocfilehash: a058df1a9e237b6382f1eec75a575c6b519a02f1
ms.sourcegitcommit: 8de025ca11b62e06ba3762b5d15cc577e0c0f15d
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/17/2020
ms.locfileid: "76165422"
---
# <a name="connecting-to-on-premises-data-sources-with-on-premises-data-gateway"></a>使用本地数据网关连接到本地数据源

本地数据网关提供本地数据源与云中的 Azure Analysis Services 服务器之间的安全数据传输。 除了适用于同一区域中的多个 Azure Analysis Services 服务器以外，最新版本的网关还适用于 Azure 逻辑应用、Power BI、Power Apps 和 Power Automate。 可将同一订阅和同一区域中的多个服务与单个网关相关联。 虽然在所有这些服务中安装的网关是相同的，但 Azure Analysis Services 和逻辑应用有一些额外的步骤。

就 Azure Analysis Services 来说，首次安装网关的过程由四个部分组成：

- **下载并运行安装程序** - 此步骤在组织中的计算机上安装网关服务。 还在[租户的](https://docs.microsoft.com/previous-versions/azure/azure-services/jj573650(v=azure.100)#what-is-an-azure-ad-tenant) Azure AD 中使用帐户登录到 Azure。 不支持 Azure B2B（来宾）帐户。
    
    <!--MOONCAKE: AVAILABLE ON  [tenant's](https://docs.microsoft.com/previous-versions/azure/azure-services/jj573650(v=azure.100)#BKMK_WhatIsAnAzureADTenant)-->

- **注册网关** - 在这一步中，指定网关的名称和恢复密钥，然后选择区域，在网关云服务中注册你的网关。 网关资源可以注册在任何区域中，但我们建议处于与 Analysis Services 服务器位于同一区域。 

- **在 Azure 中创建网关资源** - 此步骤在 Azure 订阅中创建网关资源。

- **将服务器连接到网关资源** - 在订阅中创建网关资源后，可以开始将服务器连接到该资源。 可以连接多个服务器和其他资源，前提是它们位于同一订阅和同一区域中。

## <a name="how-it-works"> </a>工作原理
在组织中的计算机上安装的网关以 Windows 服务（**本地数据网关**）的形式运行。 此本地服务已通过 Azure 服务总线注册到网关云服务。 然后，为 Azure 订阅创建本地数据网关资源。 Azure Analysis Services 服务器随后会连接到 Azure 网关资源。 如果服务器上的模型需要连接到本地数据源以执行查询或处理，查询和数据流会遍历网关资源、Azure 服务总线、本地数据网关服务和数据源。 

![工作原理](./media/analysis-services-gateway/aas-gateway-how-it-works.png)

查询和数据流：

1. 查询是云服务使用本地数据源的加密凭据创建的。 查询随后会发送到队列让网关处理。
2. 网关云服务分析该查询，并将请求推送到 [Azure 服务总线](/service-bus/)。
3. 本地数据网关会针对挂起的请求轮询 Azure 服务总线。
4. 网关获取查询，对凭据进行解密，并使用这些凭据连接到数据源。
5. 网关将查询发送到数据源以便执行。
6. 结果会从数据源返回到网关，并返回到云服务和服务器。

## <a name="installing"></a>安装

针对 Azure Analysis Services 环境进行安装时，必须按[为 Azure Analysis Services 安装和配置本地数据网关](analysis-services-gateway-install.md)中介绍的步骤操作。 本文专门针对 Azure Analysis Services。 它包含在 Azure 中设置本地数据网关资源并将 Azure Analysis Services 服务器连接到该资源所需的其他步骤。

## <a name="ports-and-communication-settings"></a>端口和通信设置

网关会创建与 Azure 服务总线之间的出站连接。 它在以下出站端口上进行通信：TCP 443（默认值）、5671、5672、9350 到 9354。 网关不需要入站端口。

可能需要在防火墙中包括数据区域的 IP 地址。 可以下载 [Azure 数据中心 IP 列表](https://www.microsoft.com/download/confirmation.aspx?id=57062)。 该列表每周都会进行更新。 Azure 数据中心 IP 列表中列出的 IP 地址使用的是 CIDR 表示法。 若要了解详细信息，请参阅 [Classless Inter-Domain Routing](https://en.wikipedia.org/wiki/Classless_Inter-Domain_Routing)（无类别域际路由）。

以下是该网关所用的完全限定域名。

| 域名 | 出站端口 | 说明 |
| --- | --- | --- |
| *.powerbi.cn |80 |用于下载安装程序的 HTTP。 |
| *.powerbi.cn |443 |HTTPS |
| *.analysis.chinacloudapi.cn |443 |HTTPS |
| *.login.chinacloudapi.cn, login.live.com, aadcdn.msauth.net |443 |HTTPS |
| *.servicebus.chinacloudapi.cn |5671-5672 |高级消息队列协议 (AMQP) |
| *.servicebus.chinacloudapi.cn |443, 9350-9354 |通过 TCP 的服务总线中继上的侦听器（需要 443 来获取访问控制令牌） |
| *.frontend.clouddatahub.net |443 |HTTPS |
| *.core.chinacloudapi.cn |443 |HTTPS |
| login.chinacloudapi.cn |443 |HTTPS |
| *.msftncsi.com |443 |在 Power BI 服务无法访问网关时用于测试 Internet 连接。 |
| *.microsoftonline-p.com |443 |用于根据配置进行身份验证。 |
| dc.services.visualstudio.com  |443 |由 AppInsights 用来收集遥测数据。 |

<a name="force-https"></a>
### <a name="forcing-https-communication-with-azure-service-bus"></a>强制与 Azure 服务总线进行 HTTPS 通信

可以强制网关使用 HTTPS 而非直接 TCP 与 Azure 服务总线进行通信，但此操作可能会显著降低性能。 若要修改 *Microsoft.PowerBI.DataMovement.Pipeline.GatewayCore.dll.config* 文件，可将值从 `AutoDetect` 更改为 `Https`。 此文件通常位于 *C:\Program Files\On-premises data gateway*。

```
<setting name="ServiceBusSystemConnectivityModeString" serializeAs="String">
    <value>Https</value>
</setting>
```

## <a name="next-steps"></a>后续步骤 

以下文章包含在本地数据网关常规内容中，该内容适用于网关支持的所有服务：

* [本地数据网关常见问题解答](https://docs.microsoft.com/data-integration/gateway/service-gateway-onprem-faq)   
* [使用本地数据网关应用](https://docs.microsoft.com/data-integration/gateway/service-gateway-app)   
* [租户级别管理](https://docs.microsoft.com/data-integration/gateway/service-gateway-tenant-level-admin)
* [配置代理设置](https://docs.microsoft.com/data-integration/gateway/service-gateway-proxy)   
* [调整通信设置](https://docs.microsoft.com/data-integration/gateway/service-gateway-communication)   
* [配置日志文件](https://docs.microsoft.com/data-integration/gateway/service-gateway-log-files)   
* [故障排除](https://docs.microsoft.com/data-integration/gateway/service-gateway-tshoot)
* [监视和优化网关性能](https://docs.microsoft.com/data-integration/gateway/service-gateway-performance)

<!-- Update_Description: update meta properties, wording update, update link -->