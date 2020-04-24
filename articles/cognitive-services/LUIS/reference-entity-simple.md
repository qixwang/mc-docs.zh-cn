---
title: 简单实体类型 - LUIS
titleSuffix: Azure Cognitive Services
description: 简单实体描述来自机器学习上下文的单个概念。 使用简单实体时添加短语列表以改进结果。
services: cognitive-services
author: lingliw
manager: digimobile
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: reference
origin.date: 09/29/2019
ms.date: 10/31/2019
ms.author: v-lingwu
ms.openlocfilehash: 2560cc76bc263b1b7f92b87bc5ac108c3205b711
ms.sourcegitcommit: c1ba5a62f30ac0a3acb337fb77431de6493e6096
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2020
ms.locfileid: "75855192"
---
# <a name="simple-entity"></a>简单实体 

简单实体是描述单个概念的泛型实体，通过机器学习的上下文习得。 由于简单实体通常是名称，例如公司名称、产品名称或其他类别的名称，因此，在使用简单实体时，请添加一个短语列表，以提升所用名称的信号。 

**在以下情况下，非常适合使用此实体：**

* 数据格式不一致，但指示相同的事物。

![简单实体](./media/luis-concept-entities/simple-entity.png)

## <a name="example-json"></a>示例 JSON

`Bob Jones wants 3 meatball pho`

在之前的陈述中，`Bob Jones` 被标记为一个简单的 `Customer` 实体。

从终结点返回的数据包括实体名称、从陈述中发现的文本、所发现文本的位置，以及评分：

#### <a name="v2-prediction-endpoint-response"></a>[V2 预测终结点响应](#tab/V2)

```JSON
"entities": [
  {
  "entity": "bob jones",
  "type": "Customer",
  "startIndex": 0,
  "endIndex": 8,
  "score": 0.473899543
  }
]
```

#### <a name="v3-prediction-endpoint-response"></a>[V3 预测终结点响应](#tab/V3)

如果在查询字符串中设置了 `verbose=false`，则这是 JSON：

```json
"entities": {
    "Customer": [
        "Bob Jones"
    ]
}```

This is the JSON if `verbose=true` is set in the query string:

```json
"entities": {
    "Customer": [
        "Bob Jones"
    ],
    "$instance": {
        "Customer": [
            {
                "type": "Customer",
                "text": "Bob Jones",
                "startIndex": 0,
                "length": 9,
                "score": 0.9339134,
                "modelTypeId": 1,
                "modelType": "Entity Extractor",
                "recognitionSources": [
                    "model"
                ]
            }
        ]
    }
}
```

* * *

|数据对象|实体名称|值|
|--|--|--|
|简单实体|`Customer`|`bob jones`|