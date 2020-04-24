---
title: Azure 服务总线 - 消息计数
description: 使用 Azure 资源管理器和 Azure 服务总线 NamespaceManager API 检索队列和订阅中保留的消息计数。
services: service-bus-messaging
documentationcenter: ''
author: lingliw
manager: digimobile
editor: spelluru
ms.service: service-bus-messaging
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
origin.date: 01/24/2020
ms.date: 04/12/2019
ms.author: v-lingwu
ms.openlocfilehash: 20df0d8a0c75559d142ee1e70f7f8f5b983fd8e4
ms.sourcegitcommit: c1ba5a62f30ac0a3acb337fb77431de6493e6096
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2020
ms.locfileid: "77067995"
---
# <a name="message-counters"></a>消息计数器

可以在 .NET Framework SDK 中使用 Azure 资源管理器和服务总线 [NamespaceManager](/dotnet/api/microsoft.servicebus.namespacemanager) API 来检索队列和订阅中含有的消息计数。

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

借助 PowerShell，可以按如下所示获取计数：

```powershell
(Get-AzServiceBusQueue -ResourceGroup mygrp -NamespaceName myns -QueueName myqueue).CountDetails
```

## <a name="message-count-details"></a>消息计数详细信息

对于确定队列是否累积了需要的处理资源比当前部署资源更多的积压工作，了解活动消息计数会十分有用。 [MessageCountDetails](/dotnet/api/microsoft.servicebus.messaging.messagecountdetails) 类提供了以下计数器详细信息：

-   [ActiveMessageCount](/dotnet/api/microsoft.servicebus.messaging.messagecountdetails.activemessagecount#Microsoft_ServiceBus_Messaging_MessageCountDetails_ActiveMessageCount)：队列或订阅中处于活动状态且已准备好进行传递的消息。
-   [DeadLetterMessageCount](/dotnet/api/microsoft.servicebus.messaging.messagecountdetails.deadlettermessagecount#Microsoft_ServiceBus_Messaging_MessageCountDetails_DeadLetterMessageCount)：死信队列中的消息。
-   [ScheduledMessageCount](/dotnet/api/microsoft.servicebus.messaging.messagecountdetails.scheduledmessagecount#Microsoft_ServiceBus_Messaging_MessageCountDetails_ScheduledMessageCount)：处于已计划状态的消息。
-   [TransferDeadLetterMessageCount](/dotnet/api/microsoft.servicebus.messaging.messagecountdetails.transferdeadlettermessagecount#Microsoft_ServiceBus_Messaging_MessageCountDetails_TransferDeadLetterMessageCount)：未能传输到其他队列或主题并且已移动到传输死信队列中的消息。
-   [TransferMessageCount](/dotnet/api/microsoft.servicebus.messaging.messagecountdetails.transfermessagecount#Microsoft_ServiceBus_Messaging_MessageCountDetails_TransferMessageCount)：等待传输到其他队列或主题的消息。

如果应用程序要基于队列长度缩放资源，则应按照慎重的进度执行此操作。 获取消息计数器是消息代理中成本高昂的操作，频繁执行它会直接对实体性能产生负面影响。

## <a name="next-steps"></a>后续步骤

若要了解有关服务总线消息传送的详细信息，请参阅以下主题：

* [服务总线队列、主题和订阅](service-bus-queues-topics-subscriptions.md)
* [服务总线队列入门](service-bus-dotnet-get-started-with-queues.md)
* [如何使用服务总线主题和订阅](service-bus-dotnet-how-to-use-topics-subscriptions.md)