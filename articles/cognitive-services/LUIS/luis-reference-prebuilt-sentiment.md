---
title: 情绪分析 - LUIS
titleSuffix: Azure Cognitive Services
description: 如果配置了情绪分析，LUIS json 响应会包含情绪分析内容。
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: reference
ms.date: 06/18/2020
ms.author: v-tawe
origin.date: 10/22/2019
ms.openlocfilehash: 78e2c371568ed5ea5cd297443b7bfb1d443549c9
ms.sourcegitcommit: 48b5ae0164f278f2fff626ee60db86802837b0b4
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/19/2020
ms.locfileid: "85102029"
---
# <a name="sentiment-analysis"></a>情绪分析
如果配置了情绪分析，LUIS json 响应会包含情绪分析内容。 请在[文本分析](/cognitive-services/text-analytics/)文档中详细了解情绪分析。


## <a name="resolution-for-sentiment"></a>情绪解决方法

情绪数据是一个介于 0 到 1 之间的分数，指示数据的正面情绪（分数接近 1）或负面情绪（分数接近 0）。

#### <a name="english-language"></a>[英语](#tab/english)

当区域性为 `en-us` 时，响应为：

```JSON
"sentimentAnalysis": {
  "label": "positive",
  "score": 0.9163064
}
```

#### <a name="other-languages"></a>[其他语言](#tab/other-languages)

对于所有其他区域性，响应为：

```JSON
"sentimentAnalysis": {
  "score": 0.9163064
}
```
* * *


