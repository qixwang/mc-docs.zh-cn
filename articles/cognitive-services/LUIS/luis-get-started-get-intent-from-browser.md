---
title: 快速入门：使用浏览器获取意向 - LUIS
titleSuffix: Azure Cognitive Services
description: 本快速入门在浏览器中使用可用的公共 LUIS 应用从会话文本中确定用户的意向。
services: cognitive-services
author: lingliw
manager: digimobile
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: quickstart
origin.date: 02/03/2020
ms.date: 02/21/2020
ms.author: v-lingwu
ms.openlocfilehash: 26557e16969d6d5f17a89397579bf3ec595eaf04
ms.sourcegitcommit: d202f6fe068455461c8756b50e52acd4caf2d095
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/28/2020
ms.locfileid: "78154548"
---
# <a name="quickstart-get-intent-with-a-browser"></a>快速入门：使用浏览器获取意向

若要了解 LUIS 预测终结点返回的内容，请在 Web 浏览器中查看预测结果。

## <a name="prerequisites"></a>先决条件

若要查询公共应用，需要：

* 自己的语言理解 (LUIS) 密钥。 如果你没有用于创建密钥的订阅，可以注册一个[免费帐户](https://azure.microsoft.com/free/)。 无法使用 LUIS 创作密钥。 
* 公共应用的 ID：`df67dcdb-c37d-46af-88e1-8b97951ca1c2`。 

## <a name="use-the-browser-to-see-predictions"></a>使用浏览器查看预测

1. 打开 Web 浏览器。
1. 使用以下完整 URL（请将 `YOUR-KEY` 替换为自己的 LUIS 创作或预测密钥）。 这些请求是 GET 请求，并包含授权（使用 LUIS 创作或预测密钥作为查询字符串参数）。

    #### <a name="v3-prediction-request"></a>[V3 预测请求](#tab/V3-1-1)
    
    
    **GET** 终结点（按槽）请求的 V3 URL 格式为：
    
    `
    https://{region}.api.cognitive.azure.cn/luis/prediction/v3.0/apps/df67dcdb-c37d-46af-88e1-8b97951ca1c2/slots/production/predict?query=turn on all lights&subscription-key=YOUR-KEY
    `

    #### <a name="v2-prediction-request"></a>[V2 预测请求](#tab/V2-1-2)
    
    **GET** 终结点请求的 V2 URL 格式为：
    
    `
    https://{region}.api.cognitive.azure.cn/luis/v2.0/apps/df67dcdb-c37d-46af-88e1-8b97951ca1c2?subscription-key=YOUR-KEY&q=turn on all lights
    `

1. 将该 URL 粘贴到浏览器窗口中，然后按 Enter。 浏览器显示的 JSON 结果指示 LUIS 将 `HomeAutomation.TurnOn` 意向检测为首要意向，并检测到值为 `on` 的 `HomeAutomation.Operation` 实体。

    #### <a name="v3-prediction-response"></a>[V3 预测响应](#tab/V3-2-1)

    ```JSON
    {
        "query": "turn on all lights",
        "prediction": {
            "topIntent": "HomeAutomation.TurnOn",
            "intents": {
                "HomeAutomation.TurnOn": {
                    "score": 0.5375382
                }
            },
            "entities": {
                "HomeAutomation.Operation": [
                    "on"
                ]
            }
        }
    }
    ```

    #### <a name="v2-prediction-response"></a>[V2 预测响应](#tab/V2-2-2)

    ```json
    {
        "query": "turn on all lights",
        "topScoringIntent": {
            "intent": "HomeAutomation.TurnOn",
            "score": 0.5375382
        },
        "entities": [
            {
                "entity": "on",
                "type": "HomeAutomation.Operation",
                "startIndex": 5,
                "endIndex": 6,
                "score": 0.724984169
            }
        ]
    }
    ```

    * * *

1. 要查看所有意图，请添加相应的查询字符串参数。

    #### <a name="v3-prediction-endpoint"></a>[V3 预测终结点](#tab/V3-3-1)

    将 `show-all-intents=true` 添加到查询字符串末尾可**显示所有意向**：

    `
    https://{region}.api.cognitive.azure.cn/luis/predict/v3.0/apps/df67dcdb-c37d-46af-88e1-8b97951ca1c2/slots/production/predict?query=turn on all lights&subscription-key=YOUR-KEY&show-all-intents=true
    `

    ```JSON
    {
        "query": "turn off the living room light",
        "prediction": {
            "topIntent": "HomeAutomation.TurnOn",
            "intents": {
                "HomeAutomation.TurnOn": {
                    "score": 0.5375382
                },
                "None": {
                    "score": 0.08687421
                },
                "HomeAutomation.TurnOff": {
                    "score": 0.0207554
                }
            },
            "entities": {
                "HomeAutomation.Operation": [
                    "on"
                ]
            }
        }
    }
    ```

    #### <a name="v2-prediction-endpoint"></a>[V2 预测终结点](#tab/V2)

    将 `verbose=true` 添加到查询字符串末尾可**显示所有意向**：

    `
    https://{region}.api.cognitive.azure.cn/luis/v2.0/apps/df67dcdb-c37d-46af-88e1-8b97951ca1c2?q=turn on all lights&subscription-key={your-key}&verbose=true
    `

    ```json
    {
        "query": "turn on all lights",
        "topScoringIntent": {
            "intent": "HomeAutomation.TurnOn",
            "score": 0.5375382
        },
        "intents": [
            {
                "intent": "HomeAutomation.TurnOn",
                "score": 0.5375382
            },
            {
                "intent": "None",
                "score": 0.08687421
            },
            {
                "intent": "HomeAutomation.TurnOff",
                "score": 0.0207554
            }
        ],
        "entities": [
            {
                "entity": "on",
                "type": "HomeAutomation.Operation",
                "startIndex": 5,
                "endIndex": 6,
                "score": 0.724984169
            }
        ]
    }
    ```


<!-- FIX - is the public app getting updated for the new prebuilt domain with entities? -->   

