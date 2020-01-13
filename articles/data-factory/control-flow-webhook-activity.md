---
title: Azure 数据工厂中的 Webhook 活动
description: Webhook 活动在使用用户指定的某些条件验证附加的数据集之前，不会继续执行管道。
services: data-factory
documentationcenter: ''
author: WenJason
ms.author: v-jay
manager: digimobile
ms.reviewer: maghan
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
origin.date: 03/25/2019
ms.date: 01/06/2020
ms.openlocfilehash: 089cf6c8a8fe1b0fd831d08a52980987dde3c0ea
ms.sourcegitcommit: 6a8bf63f55c925e0e735e830d67029743d2c7c0a
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/03/2020
ms.locfileid: "75624217"
---
# <a name="webhook-activity-in-azure-data-factory"></a>Azure 数据工厂中的 Webhook 活动
可以使用 Webhook 活动通过自定义代码控制管道的执行。 使用 Webhook 活动，客户可以调用终结点并传递回调 URL。 管道运行在继续下一个活动之前，等待调用回调。

## <a name="syntax"></a>语法

```json

{
    "name": "MyWebHookActivity",
    "type": "WebHook",
    "typeProperties": {
        "method": "POST",
        "url": "<URLEndpoint>",
        "headers": {
            "Content-Type": "application/json"
        },
        "body": {
            "key": "value"
        },
        "timeout": "00:03:00",
        "authentication": {
            "type": "ClientCertificate",
            "pfx": "****",
            "password": "****"
        }
    }
}

```


## <a name="type-properties"></a>Type 属性



属性 | 说明 | 允许的值 | 必须
-------- | ----------- | -------------- | --------
name | Webhook 活动的名称 | String | 是 |
type | 必须设置为 **WebHook**。 | String | 是 |
method | 目标终结点的 Rest API 方法。 | 字符串。 支持的类型：“POST” | 是 |
url | 目标终结点和路径 | 字符串（或带有 resultType 字符串的表达式）。 | 是 |
headers | 发送到请求的标头。 例如，若要在请求中设置语言和类型："headers" : { "Accept-Language": "en-us", "Content-Type": "application/json" }。 | 字符串（或带有 resultType 字符串的表达式） | 是，需要内容类型标头。 "headers":{ "Content-Type":"application/json"} |
body | 表示要发送到终结点的有效负载。 | 有效 JSON（或 resultType 为 JSON 的表达式）。 请参阅[请求有效负载架构](/data-factory/control-flow-web-activity#request-payload-schema)部分中的请求有效负载架构。 | 是 |
authentication | 用于调用该终结点的身份验证方法。 支持的类型为“Basic”或“ClientCertificate”。 有关详细信息，请参阅[身份验证](/data-factory/control-flow-web-activity#authentication)部分。 如果不需要身份验证，则排除此属性。 | 字符串（或带有 resultType 字符串的表达式） | 否 |
timeout | 活动将等待多长时间才能调用 &#39;callBackUri&#39;。 活动将等待多长时间才能调用“callBackUri”。 默认值为 10 分钟 (“00:10:00”)。 格式为 Timespan，即 d.hh:mm:ss | String | 否 |
回调时报告状态 | 允许用户报告 Webhook 活动的失败状态，这会将活动标记为“失败” | 布尔 | 否 |

## <a name="additional-notes"></a>附加说明

Azure 数据工厂会将正文中的附加属性“callBackUri”传递给 url 终结点，并且需要在达到指定超时值之前调用此 uri。 如果未调用此 uri，则活动将失败，其状态为“超时”。

仅当对自定义终结点的调用失败时，Webhook 活动本身才会失败。 可以将任何错误消息添加到回调正文中，并在后续活动中使用。

传递回回调 URI 的正文应该是有效 JSON。 必须将 Content-Type 标头设置为 `application/json`。

使用“回调时报告状态”选项时，必须在进行回调时将以下代码片段添加到正文中：

```
{
    "Output": {
        // output object will be used in activity output
        "testProp": "testPropValue"
    },
    "Error": {
        // Optional, set it when you want to fail the activity
        "ErrorCode": "testErrorCode",
        "Message": "error message to show in activity error"
    },
    "StatusCode": "403" // when status code is >=400, activity will be marked as failed
}
```



## <a name="next-steps"></a>后续步骤

查看数据工厂支持的其他控制流活动：

- [If Condition 活动](control-flow-if-condition-activity.md)
- [Execute Pipeline 活动](control-flow-execute-pipeline-activity.md)
- [For Each 活动](control-flow-for-each-activity.md)
- [Get Metadata 活动](control-flow-get-metadata-activity.md)
- [Lookup 活动](control-flow-lookup-activity.md)
- [Web 活动](control-flow-web-activity.md)
- [Until 活动](control-flow-until-activity.md)
