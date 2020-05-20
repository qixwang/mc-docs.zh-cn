---
title: 描述符 - LUIS
titleSuffix: Azure Cognitive Services
description: 使用语言理解 (LUIS) 添加应用功能，可以改进对类别和模式的意向和实体的检测或预测
services: cognitive-services
author: lingliw
manager: digimobile
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: conceptual
origin.date: 11/14/2019
ms.date: 12/05/2019
ms.author: v-lingwu
ms.openlocfilehash: 3b405bf140bf4d6ceaed794cc20f179c157befc9
ms.sourcegitcommit: c1ba5a62f30ac0a3acb337fb77431de6493e6096
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2020
ms.locfileid: "74884617"
---
# <a name="use-descriptors-to-boost-signal-of-word-list"></a>使用描述符来增强字词列表的信号

可以将功能添加到 LUIS 应用以提高其准确性。 特征通过提供某些字词和短语是应用域词汇表的一部分的提示来帮助 LUIS。 

描述符（短语列表）包括一组值（词或短语），它们属于同一个类，并且必须以同样的方式处理它们（例如城市或产品名称）。 LUIS 对其中一个值的了解也自动应用到其他值。 此列表不同于包含匹配字词的[列表实体](reference-entity-list.md)（完全文本匹配）。

描述符添加到应用域的词汇中，作为这些字词的第二个 LUIS 信号。

## <a name="add-descriptor"></a>添加描述符

1. 单击“我的应用”页上的名称打开应用，单击“构建”，然后单击应用左侧面板中的“描述符”    。 

1. 在“描述符”页上，单击“+ 添加描述符”   。 
 
1. 在“创建新短语列表描述符”  对话框中，输入描述符的名称（例如 `Cities`）。 在“值”框中，键入描述符的值，例如 `Seattle`。 可以一次键入一个值或者用逗号分隔的一组值，然后按 Enter  。

    > [!div class="mx-imgBorder"]
    > ![添加描述符“Cities”](./media/luis-add-features/add-phrase-list-cities.png)

    为 LUIS 输入足够的值后，会显示建议。 对于建议的值，可以单击“+ 全部添加”，也可选择单个术语  。

1. 如果添加的描述符值是可交换使用的替代值，则让“这些值可以交换”保持选中状态  。

1. 选择“完成”  。 新描述符会添加到“描述符”页  。

<a name="edit-phrase-list"></a>
<a name="delete-phrase-list"></a>
<a name="deactivate-phrase-list"></a>

> [!Note]
> 可以在“描述符”  页上，删除或取消激活上下文工具栏中的描述符。