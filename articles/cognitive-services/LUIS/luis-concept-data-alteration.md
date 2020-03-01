---
title: 数据更改 - LUIS
description: 了解如何在语言理解 (LUIS) 得出预测之前更改数据
author: lingliw
manager: digimobile
ms.topic: conceptual
origin.date: 02/11/2020
ms.date: 02/21/2020
ms.author: v-lingwu
ms.openlocfilehash: 0f169ecd492a0f2bee362901fd9cad8e99db2edf
ms.sourcegitcommit: d202f6fe068455461c8756b50e52acd4caf2d095
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/28/2020
ms.locfileid: "78154830"
---
# <a name="alter-utterance-data-before-or-during-prediction"></a>在预测之前或预测期间更改话语数据
LUIS 提供在预测之前或预测期间操作陈述的方法。 这些方法包括修复拼写，以及修复预生成 datetimeV2 的时区问题。 

## <a name="correct-spelling-errors-in-utterance"></a>更正陈述中的拼写错误

[!INCLUDE [Not supported in V3 API prediction endpoint](./includes/v2-support-only.md)]


终结点需要两个参数以进行拼写更正：

|Param|Value|
|--|--|
|`spellCheck`|boolean|
|`bing-spell-check-subscription-key`|[必应拼写检查 API V7](https://www.azure.cn/home/features/cognitive-services/spell-check/) 终结点密钥|

[必应拼写检查 API V7](https://www.azure.cn/home/features/cognitive-services/spell-check/) 检测到错误时，将一并从终结点返回原始陈述、已更正陈述和预测。

#### <a name="v2-prediction-endpoint-response"></a>[V2 预测终结点响应](#tab/V2)

```JSON
{
  "query": "Book a flite to London?",
  "alteredQuery": "Book a flight to London?",
  "topScoringIntent": {
    "intent": "BookFlight",
    "score": 0.780123
  },
  "entities": []
}
```

#### <a name="v3-prediction-endpoint-response"></a>[V3 预测终结点响应](#tab/V3)
 
```JSON
{
    "query": "Book a flite to London?",
    "prediction": {
        "normalizedQuery": "book a flight to london?",
        "topIntent": "BookFlight",
        "intents": {
            "BookFlight": {
                "score": 0.780123
            }
        },
        "entities": {},
    }
}
```

* * * 

### <a name="list-of-allowed-words"></a>允许的字词列表
LUIS 中使用的必应拼写检查 API 不支持在拼写检查更改期间要忽略的单词列表。 如果需要允许字词或首字母缩写词的列表，请在将话语发送到 LUIS 进行意向预测之前在客户端应用程序中处理话语。

## <a name="change-time-zone-of-prebuilt-datetimev2-entity"></a>更改预生成 datetimeV2 实体的时区
LUIS 应用使用预生成的 datetimeV2 实体时，可以在预测响应中返回日期/时间值。 请求的时区用于确定要返回的正确日期/时间。 如果请求在到达 LUIS 之前来自机器人或另一个集中式应用程序，则更正 LUIS 使用的时区。 

### <a name="endpoint-querystring-parameter"></a>终结点 querystring 参数
通过使用 `timezoneOffset` 参数将用户的时区添加到[终结点](https://go.microsoft.com/fwlink/?linkid=2092356)来更正时区。 要更改时间，则 `timezoneOffset` 的值应为正数或负数（以分钟为单位）。  

|Param|Value|
|--|--|
|`timezoneOffset`|正数或负数（以分钟为单位）|

### <a name="daylight-savings-example"></a>夏令时示例
如果需要返回的预生成 datetimeV2 来调整夏令时，则对于该[终结点](https://go.microsoft.com/fwlink/?linkid=2092356)查询应使用值为正数/负数（以分钟为单位）的 `timezoneOffset` querystring 参数。

#### <a name="v2-prediction-endpoint-request"></a>[V2 预测终结点请求](#tab/V2)

增加 60 分钟： 

https://{region}.api.cognitive.azure.cn/luis/v2.0/apps/{appId}?q=Turn the lights on?**timezoneOffset=60**&verbose={boolean}&spellCheck={boolean}&staging={boolean}&bing-spell-check-subscription-key={string}&log={boolean}

减去 60 分钟： 

https://{region}.api.cognitive.azure.cn/luis/v2.0/apps/{appId}?q=Turn the lights on?**timezoneOffset=-60**&verbose={boolean}&spellCheck={boolean}&staging={boolean}&bing-spell-check-subscription-key={string}&log={boolean}

#### <a name="v3-prediction-endpoint-request"></a>[V3 预测终结点请求](#tab/V3)

增加 60 分钟：

https://{region}.api.cognitive.azure.cn/luis/v3.0-preview/apps/{appId}/slots/production/predict?query=Turn the lights on?**timezoneOffset=60**&spellCheck={boolean}&bing-spell-check-subscription-key={string}&log={boolean}

减去 60 分钟： 

https://{region}.api.cognitive.azure.cn/luis/v3.0-preview/apps/{appId}/slots/production/predict?query=Turn the lights on?**timezoneOffset=-60**&spellCheck={boolean}&bing-spell-check-subscription-key={string}&log={boolean}

详细了解 [V3 预测终结点](luis-migration-api-v3.md)。

* * * 

## <a name="c-code-determines-correct-value-of-timezoneoffset"></a>C# 代码确定正确的 timezoneOffset 值
下面的 C# 代码使用 [TimeZoneInfo](https://docs.microsoft.com/dotnet/api/system.timezoneinfo) 类的 [FindSystemTimeZoneById](https://docs.microsoft.com/dotnet/api/system.timezoneinfo.findsystemtimezonebyid#examples) 方法基于系统时间来确定正确的 `timezoneOffset`：

```CSharp
// Get CST zone id
TimeZoneInfo targetZone = TimeZoneInfo.FindSystemTimeZoneById("Central Standard Time");

// Get local machine's value of Now
DateTime utcDatetime = DateTime.UtcNow;

// Get Central Standard Time value of Now
DateTime cstDatetime = TimeZoneInfo.ConvertTimeFromUtc(utcDatetime, targetZone);

// Find timezoneOffset
int timezoneOffset = (int)((cstDatetime - utcDatetime).TotalMinutes);
```
