---
title: Keyphrase 预生成实体 - LUIS
titleSuffix: Azure Cognitive Services
description: 本文包含了语言理解 (LUIS) 中的 keyPhrase 预构建实体信息。
services: cognitive-services
author: lingliw
manager: digimobile
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: conceptual
origin.date: 09/27/2019
ms.date: 10/31/2019
ms.author: v-lingwu
ms.openlocfilehash: 23d7032e75207350c9b81eeb80097d1dc03e37c2
ms.sourcegitcommit: c1ba5a62f30ac0a3acb337fb77431de6493e6096
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2020
ms.locfileid: "74884934"
---
# <a name="keyphrase-prebuilt-entity-for-a-luis-app"></a>LUIS 应用的 keyPhrase 预构建实体
keyPhrase 实体从话语中提取各种关键短语。 不需要将包含 keyPhrase 的示例话语添加到应用程序。 keyPhrase 实体作为[文本分析](luis-language-support.md#languages-supported)功能的一部分，在[许多区域性](../text-analytics/overview.md)中都受支持。 

## <a name="resolution-for-prebuilt-keyphrase-entity"></a>预构建 keyPhrase 实体的解析

查询返回以下实体对象：

`where is the educational requirements form for the development and engineering group`

#### <a name="v3-response"></a>[V3 响应](#tab/V3)

以下 JSON 的 `verbose` 参数设置为 `false`：

```json
"entities": {
    "keyPhrase": [
        "educational requirements",
        "development"
    ]
}
```
#### <a name="v3-verbose-response"></a>[V3 详细响应](#tab/V3-verbose)
以下 JSON 的 `verbose` 参数设置为 `true`：

```json
"entities": {
    "keyPhrase": [
        "educational requirements",
        "development"
    ],
    "$instance": {
        "keyPhrase": [
            {
                "type": "builtin.keyPhrase",
                "text": "educational requirements",
                "startIndex": 13,
                "length": 24,
                "modelTypeId": 2,
                "modelType": "Prebuilt Entity Extractor",
                "recognitionSources": [
                    "model"
                ]
            },
            {
                "type": "builtin.keyPhrase",
                "text": "development",
                "startIndex": 51,
                "length": 11,
                "modelTypeId": 2,
                "modelType": "Prebuilt Entity Extractor",
                "recognitionSources": [
                    "model"
                ]
            }
        ]
    }
}
```
#### <a name="v2-response"></a>[V2 响应](#tab/V2)

以下示例显示了 **builtin.keyPhrase** 实体的解析。

```json
"entities": [
    {
        "entity": "development",
        "type": "builtin.keyPhrase",
        "startIndex": 51,
        "endIndex": 61
    },
    {
        "entity": "educational requirements",
        "type": "builtin.keyPhrase",
        "startIndex": 13,
        "endIndex": 36
    }
]
```
* * * 

## <a name="next-steps"></a>后续步骤

详细了解 [V3 预测终结点](luis-migration-api-v3.md)。

了解[百分比](luis-reference-prebuilt-percentage.md)、[数字](luis-reference-prebuilt-number.md)和[存在时间](luis-reference-prebuilt-age.md)实体。




