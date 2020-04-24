---
title: 应用程序设置 - LUIS
titleSuffix: Azure Cognitive Services
description: Azure 认知服务语言理解应用的应用程序设置存储在应用和门户中。
services: cognitive-services
author: lingliw
manager: digimobile
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: conceptual
origin.date: 11/12/2019
ms.date: 12/05/2019
ms.author: v-lingwu
ms.openlocfilehash: a690fbb807e0fa81d8004d20fed9dbfea2065b61
ms.sourcegitcommit: c1ba5a62f30ac0a3acb337fb77431de6493e6096
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2020
ms.locfileid: "74982043"
---
# <a name="application-settings"></a>应用程序设置

这些应用程序设置存储在[导出的](https://{region}.dev.cognitive.azure.cn/docs/services/5890b47c39e2bb17b84a55ff/operations/5890b47c39e2bb052c5b9c40)应用中，并使用 REST API 进行[更新](https://{region}.dev.cognitive.azure.cn/docs/services/5890b47c39e2bb17b84a55ff/operations/versions-update-application-version-settings)。 更改应用版本设置会将应用训练状态重置为“未训练”。

|设置|默认值|说明|
|--|--|--|
|NormalizePunctuation|True|删除标点。|
|NormalizeDiacritics|True|删除音调符号。|

## <a name="diacritics-normalization"></a>音调符号规范化 

在 `settings` 参数中针对 LUIS JSON 应用文件的音调符号打开话语规范化。

```JSON
"settings": [
    {"name": "NormalizeDiacritics", "value": "true"}
] 
```

以下话语显示了音调符号规范化如何影响话语：

|音调符号设置为 false 时|音调符号设置为 true 时|
|--|--|
|`quiero tomar una piña colada`|`quiero tomar una pina colada`|
|||


## <a name="punctuation-normalization"></a>标点规范化

在 `settings` 参数中针对 LUIS JSON 应用文件的标点打开话语规范化。

```JSON
"settings": [
    {"name": "NormalizePunctuation", "value": "true"}
] 
```

以下话语显示了标点如何影响话语：

|当标点设置为 False 时|当标点设置为 True 时|
|--|--|
|`Hmm..... I will take the cappuccino`|`Hmm I will take the cappuccino`|
|||

### <a name="punctuation-removed"></a>标点已删除

将 `NormalizePunctuation` 设置为 true 时，将删除以下标点符号。

|标点|
|--|
|`-`| 
|`.`| 
|`'`|
|`"`|
|`\`|
|`/`|
|`?`|
|`!`|
|`_`|
|`,`|
|`;`|
|`:`|
|`(`|
|`)`|
|`[`|
|`]`|
|`{`|
|`}`|
|`+`|
|`¡`|
