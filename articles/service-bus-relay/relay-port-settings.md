---
title: Azure 中继端口设置 | Microsoft Docs
description: 本文包含一个表，该表描述了 Azure 中继端口值所需的配置。
services: service-bus-relay
documentationcenter: na
author: sethmanheim
manager: timlt
editor: ''
ms.assetid: ''
ms.service: service-bus-relay
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
origin.date: 01/21/2020
ms.date: 2/26/2020
ms.author: v-lingwu
ms.openlocfilehash: d1d1064856efa3cd2a4e20e5824011d5f1b5818c
ms.sourcegitcommit: c1ba5a62f30ac0a3acb337fb77431de6493e6096
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2020
ms.locfileid: "78154982"
---
# <a name="azure-relay-port-settings"></a>Azure 中继端口设置

下表描述 Azure 中继端口值的所需配置。

## <a name="hybrid-connections"></a>混合连接

混合连接在端口 443 上结合使用 WebSocket 和 SSL，以用作仅使用 HTTPS  的基础传输机制。 

## <a name="wcf-relays"></a>WCF 中继
  
|绑定|传输安全|端口|  
|-------------|------------------------|----------|  
|[BasicHttpRelayBinding 类](/dotnet/api/microsoft.servicebus.basichttprelaybinding)（客户端）|是|HTTPS| 
|" |否|HTTP|  
|[BasicHttpRelayBinding 类](/dotnet/api/microsoft.servicebus.basichttprelaybinding)（服务）|之前或之后|9351/HTTP|  
|[NetEventRelayBinding 类](/dotnet/api/microsoft.servicebus.neteventrelaybinding)（客户端）|是|9351/HTTPS|  
|" |否|9350/HTTP|  
|[NetEventRelayBinding 类](/dotnet/api/microsoft.servicebus.neteventrelaybinding)（服务）|之前或之后|9351/HTTP|  
|[NetTcpRelayBinding 类](/dotnet/api/microsoft.servicebus.nettcprelaybinding)（客户端/服务）|之前或之后|5671/9352/HTTP（9352/9353，如果使用混合）|  
|[NetOnewayRelayBinding 类](/dotnet/api/microsoft.servicebus.netonewayrelaybinding)（客户端）|是|9351/HTTPS|  
|" |否|9350/HTTP|  
|[NetOnewayRelayBinding 类](/dotnet/api/microsoft.servicebus.netonewayrelaybinding)（服务）|之前或之后|9351/HTTP|  
|[WebHttpRelayBinding 类](/dotnet/api/microsoft.servicebus.webhttprelaybinding)（客户端）|是|HTTPS|  
|" |否|HTTP|  
|[WebHttpRelayBinding 类](/dotnet/api/microsoft.servicebus.webhttprelaybinding)（服务）|之前或之后|9351/HTTP|  
|[WS2007HttpRelayBinding 类](/dotnet/api/microsoft.servicebus.ws2007httprelaybinding)（客户端）|是|HTTPS|  
|" |否|HTTP|  
|[WS2007HttpRelayBinding 类](/dotnet/api/microsoft.servicebus.ws2007httprelaybinding)（服务）|之前或之后|9351/HTTP|

## <a name="next-steps"></a>后续步骤
若要了解有关 Azure 中继的详细信息，请访问以下链接：
* [什么是 Azure 中继？](relay-what-is-it.md)
* [中继常见问题](relay-faq.md)


<!--Update_Description:update meta properties only-->