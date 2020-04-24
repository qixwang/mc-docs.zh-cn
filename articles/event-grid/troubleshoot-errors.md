---
title: Azure 事件网格 - 故障排除指南
description: 本文提供错误代码列表、错误消息、说明和建议的措施。
services: event-grid
author: spelluru
ms.service: event-grid
ms.topic: conceptual
origin.date: 08/22/2019
ms.date: 09/30/2019
ms.author: v-yiso
ms.openlocfilehash: b8150817e6bce088fa82d473f98dbc2480b39e04
ms.sourcegitcommit: c1ba5a62f30ac0a3acb337fb77431de6493e6096
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2020
ms.locfileid: "71156468"
---
# <a name="troubleshoot-azure-event-grid-errors"></a>排查 Azure 事件网格错误
本故障排除指南提供 Azure 事件网格错误代码列表、错误消息、说明以及在收到这些错误时应采取的建议措施。 

## <a name="error-code-400"></a>错误代码：400
| 错误代码 | 错误消息 | 说明 | 建议 |
| ---------- | ------------- | ----------- | -------------- | 
| HttpStatusCode.BadRequest<br/>400 | 主题名称的长度必须为 3 到 50 个字符。 | 自定义主题名称的长度应为 3 到 50 个字符。 主题名称中只允许字母数字字符、数字和“-”字符。 此外，名称的开头不能是以下保留字： <ul><li>Microsoft</li><li>EventGrid</li><li>系统</li></ul> | 请选择符合主题名称要求的其他主题名称。 |
| HttpStatusCode.BadRequest<br/>400 | 域名的长度必须为 3 到 50 个字符。 | 域名的长度应为 3 到 50 个字符。 主题名称中只允许字母数字字符、数字和“-”字符。 此外，名称的开头不能是以下保留字：<ul><li>Microsoft</li><li>EventGrid</li><li>系统</li> | 请选择符合域名要求的其他域名。 |
| HttpStatusCode.BadRequest<br/>400 | 过期时间无效。 | 事件订阅的过期时间决定了事件订阅何时停用。 此值应是将来的有效日期时间值。| 确保事件订阅过期时间采用有效的日期时间格式，并设置为将来的时间。 |

## <a name="error-code-409"></a>错误代码：409
| 错误代码 | 错误消息 | 说明 | 建议的操作 |
| ---------- | ------------- | ----------- | -------------- | 
| HttpStatusCode.Conflict <br/>409 | 已存在具有指定名称的主题。 请选择其他主题名称。   | 自定义主题名称在单个 Azure 区域中应保持唯一，以确保正常完成发布操作。 同一名称可在不同的 Azure 区域中使用。 | 请为主题选择其他名称。 |
| HttpStatusCode.Conflict <br/> 409 | 已存在具有指定名称的域。 请选择其他域名。 | 域名在单个 Azure 区域中应保持唯一，以确保正常完成发布操作。 同一名称可在不同的 Azure 区域中使用。 | 请为该域选择其他名称。 |
| HttpStatusCode.Conflict<br/>409 | 已达配额限制。 有关这些限制的详细信息，请参阅 [Azure 事件网格限制](../azure-subscription-service-limits.md#event-grid-limits)。  | 每个 Azure 订阅可使用的 Azure 事件网格资源数量有限制。 已超过部分或全部配额，无法创建更多的资源。 |  请检查当前的资源用量，并删除任何不需要的资源。 如果仍需提高配额，请向 [aeg@microsoft.com](mailto:aeg@microsoft.com) 发送电子邮件并在其中指出所需的确切资源数。 |


## <a name="next-steps"></a>后续步骤
如需更多帮助，请在 [Stack Overflow 论坛](https://stackoverflow.com/questions/tagged/azure-eventgrid)中发布问题，或开具[支持票证](https://support.azure.cn/zh-cn/support/contact)。 
