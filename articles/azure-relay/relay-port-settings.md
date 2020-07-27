---
title: Azure 中继端口设置
description: 本文包含一个表，该表描述了 Azure 中继端口值所需的配置。
ms.topic: article
origin.date: 06/23/2020
ms.date: 07/27/2020
ms.testscope: no
ms.testdate: ''
ms.author: v-yeche
author: rockboyfor
ms.openlocfilehash: c8b831b94b1bcd6d7e3b94800320a58fde226fef
ms.sourcegitcommit: 091c672fa448b556f4c2c3979e006102d423e9d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/24/2020
ms.locfileid: "87162412"
---
# <a name="azure-relay-port-settings"></a>Azure 中继端口设置

下表描述 Azure 中继端口值的所需配置。

## <a name="hybrid-connections"></a>混合连接

混合连接在端口 443 上结合使用 WebSocket 和 TLS，作为仅使用 HTTPS 的基础传输机制。 

## <a name="wcf-relays"></a>WCF 中继

|绑定|传输安全|端口|  
|-------------|------------------------|----------|  
|[BasicHttpRelayBinding 类](https://docs.azure.cn/dotnet/api/microsoft.servicebus.basichttprelaybinding?view=azure-dotnet)（客户端）|是|HTTPS| 
|" |否|HTTP|  
|[BasicHttpRelayBinding 类](https://docs.azure.cn/dotnet/api/microsoft.servicebus.basichttprelaybinding?view=azure-dotnet)（服务）|任一个|9351/HTTP|  
|[NetEventRelayBinding 类](https://docs.azure.cn/dotnet/api/microsoft.servicebus.neteventrelaybinding?view=azure-dotnet)（客户端）|是|9351/HTTPS|  
|" |否|9350/HTTP|  
|[NetEventRelayBinding 类](https://docs.azure.cn/dotnet/api/microsoft.servicebus.neteventrelaybinding?view=azure-dotnet)（服务）|任一个|9351/HTTP|  
|[NetTcpRelayBinding 类](https://docs.azure.cn/dotnet/api/microsoft.servicebus.nettcprelaybinding?view=azure-dotnet)（客户端/服务）|任一个|5671/9352/HTTP（9352/9353，如果使用混合）|  
|[NetOnewayRelayBinding 类](https://docs.azure.cn/dotnet/api/microsoft.servicebus.netonewayrelaybinding?view=azure-dotnet)（客户端）|是|9351/HTTPS|  
|" |否|9350/HTTP|  
|[NetOnewayRelayBinding 类](https://docs.azure.cn/dotnet/api/microsoft.servicebus.netonewayrelaybinding?view=azure-dotnet)（服务）|任一个|9351/HTTP|  
|[WebHttpRelayBinding 类](https://docs.azure.cn/dotnet/api/microsoft.servicebus.webhttprelaybinding?view=azure-dotnet)（客户端）|是|HTTPS|  
|" |否|HTTP|  
|[WebHttpRelayBinding 类](https://docs.azure.cn/dotnet/api/microsoft.servicebus.webhttprelaybinding?view=azure-dotnet)（服务）|任一个|9351/HTTP|  
|[WS2007HttpRelayBinding 类](https://docs.azure.cn/dotnet/api/microsoft.servicebus.ws2007httprelaybinding?view=azure-dotnet)（客户端）|是|HTTPS|  
|" |否|HTTP|  
|[WS2007HttpRelayBinding 类](https://docs.azure.cn/dotnet/api/microsoft.servicebus.ws2007httprelaybinding?view=azure-dotnet)（服务）|任一个|9351/HTTP|

## <a name="next-steps"></a>后续步骤
若要了解有关 Azure 中继的详细信息，请访问以下链接：
* [什么是 Azure 中继？](relay-what-is-it.md)
* [中继常见问题](relay-faq.md)

<!-- Update_Description: update meta properties, wording update, update link -->