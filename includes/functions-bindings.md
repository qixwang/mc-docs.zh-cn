---
author: ggailey777
ms.service: azure-functions
ms.topic: include
ms.date: 12/31/2019
ms.author: v-junlch
ms.openlocfilehash: 99d0b29496b4c9ba0213ca088c25c35467a88185
ms.sourcegitcommit: 6a8bf63f55c925e0e735e830d67029743d2c7c0a
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/03/2020
ms.locfileid: "75624108"
---
下表显示了 Azure Functions 运行时的主版本支持的绑定：


| 类型 | 1.x | 2.x 及更高版本<sup>1</sup> | 触发器 | 输入 | 输出 |
| ---- | :-: | :-: | :------: | :---: | :----: |
| [Blob 存储](../articles/azure-functions/functions-bindings-storage-blob.md)          |✔|✔|✔|✔|✔|
| [Cosmos DB](../articles/azure-functions/functions-bindings-documentdb.md)               |✔|✔|✔|✔|✔|
| [事件网格](../articles/azure-functions/functions-bindings-event-grid.md)              |✔|✔|✔| | |
| [事件中心](../articles/azure-functions/functions-bindings-event-hubs.md)              |✔|✔|✔| |✔|
| [HTTP 和 Webhook](../articles/azure-functions/functions-bindings-http-webhook.md)             |✔|✔|✔| |✔|
| [IoT 中心](../articles/azure-functions/functions-bindings-event-iot.md)             |✔|✔|✔| |✔|
| [Microsoft Graph<br/>Excel 表](../articles/azure-functions/functions-bindings-microsoft-graph.md)   ||✔| |✔|✔|
| [Microsoft Graph<br/>OneDrive 文件](../articles/azure-functions/functions-bindings-microsoft-graph.md) ||✔| |✔|✔|
| [Microsoft Graph<br/>Outlook 电子邮件](../articles/azure-functions/functions-bindings-microsoft-graph.md)  ||✔| | |✔|
| [Microsoft Graph<br/>事件](../articles/azure-functions/functions-bindings-microsoft-graph.md)         ||✔|✔|✔|✔|
| [Microsoft Graph<br/>身份验证令牌](../articles/azure-functions/functions-bindings-microsoft-graph.md)    ||✔| |✔| |
| [移动应用](../articles/azure-functions/functions-bindings-mobile-apps.md)             |✔| | |✔|✔|
| [通知中心](../articles/azure-functions/functions-bindings-notification-hubs.md) |✔|| | |✔|
| [队列存储](../articles/azure-functions/functions-bindings-storage-queue.md)         |✔|✔|✔| |✔|
| [SendGrid](../articles/azure-functions/functions-bindings-sendgrid.md)                   |✔|✔| | |✔|
| [服务总线](../articles/azure-functions/functions-bindings-service-bus.md)             |✔|✔|✔| |✔|
| [SignalR](../articles/azure-functions/functions-bindings-signalr-service.md)             | |✔| |✔|✔|
| [表存储](../articles/azure-functions/functions-bindings-storage-table.md)         |✔|✔| |✔|✔|
| [计时器](../articles/azure-functions/functions-bindings-timer.md)                         |✔|✔|✔| | |

<sup>1</sup> 从版本 2.x 运行时开始，除了 HTTP 和 Timer 以外，所有绑定都必须注册。 请参阅[注册绑定扩展](../articles/azure-functions/functions-bindings-register.md)。

