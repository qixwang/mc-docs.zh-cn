---
title: 模式添加准确性 - LUIS
titleSuffix: Azure Cognitive Services
description: 在语言理解 (LUIS) 应用程序中添加模式模板，以提高预测准确性。
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: how-to
ms.date: 06/18/2020
ms.author: v-tawe
origin.date: 05/17/2020
ms.openlocfilehash: 5e5db9e1b934fdfa62aa56f03bdd88cbbd7d0cfa
ms.sourcegitcommit: 48b5ae0164f278f2fff626ee60db86802837b0b4
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/19/2020
ms.locfileid: "85102014"
---
# <a name="how-to-add-patterns-to-improve-prediction-accuracy"></a>如何添加模式以提高预测准确性
在 LUIS 应用接收到终结点话语后，请使用[模式](luis-concept-patterns.md)提高话语（按字词顺序和字词选择呈现模式）的预测准确性。 模式使用特定[语法](luis-concept-patterns.md#pattern-syntax)指示[实体](luis-concept-entity-types.md)、实体[角色](luis-concept-roles.md)和可选文本的位置。

> [!CAUTION]
> 模式仅包括机器学习实体父项，而不包括子实体。

## <a name="add-template-utterance-using-correct-syntax"></a>使用正确的语法添加模板言语

1. 登录到 [LUIS 门户](https://luis.azure.cn)，选择“订阅”**** 和“创作资源”**** 以查看分配给该创作资源的应用。
1. 在“我的应用”页上选择应用名称以打开应用****。
1. 在左侧面板中，选择“改进应用性能”下的“模式”**** ****。

1. 为模式选择正确意向。

1. 在模板文本框中，键入模板表述，然后按 Enter。 如果要输入实体名称，请使用正确的模式实体语法。 使用以 `{` 开头的实体语法。 实体列表随即显示。 选择正确的实体。

    > [!div class="mx-imgBorder"]
    > ![模式实体的屏幕截图](./media/luis-how-to-model-intent-pattern/patterns-3.png)

    如果实体中包含[角色](luis-concept-roles.md)，请在实体名称后用一个冒号 (`:`) 指示该角色，例如 `{Location:Origin}`。 实体的角色列表将以列表形式显示。 选择该角色，然后按 Enter。

    > [!div class="mx-imgBorder"]
    > ![包含角色的实体的屏幕截图](./media/luis-how-to-model-intent-pattern/patterns-4.png)

    选择正确的实体后，请完成模式输入，然后按 Enter。 模式输入完成后，请[培训](luis-how-to-train.md)应用。

    > [!div class="mx-imgBorder"]
    > ![包含两种实体类型的已输入模式的屏幕截图](./media/luis-how-to-model-intent-pattern/patterns-5.png)

## <a name="create-a-patternany-entity"></a>创建 pattern.any 实体

[Pattern.any](luis-concept-entity-types.md) 实体仅在[模式](luis-how-to-model-intent-pattern.md)中有效，在意向的示例言语中无效。 此类实体有助于 LUIS 查找不同长度和字词选择的实体结尾。 由于此实体会在模式中使用，因此，LUIS 可识别实体末尾在话语模板中的位置。

1. 登录到 [LUIS 门户](https://luis.azure.cn)，选择“订阅”**** 和“创作资源”**** 以查看分配给该创作资源的应用。
1. 在“我的应用”页上选择应用名称以打开应用****。
1. 在“生成”部分的左侧面板中选择“实体”，然后选择“+ 创建”**** **** ****。

1. 在“选择实体类型”对话框中，在“名称”框中输入实体名称，然后选择“Pattern.Any”作为“类型”，然后选择“创建”**** **** **** **** ****。



## <a name="train-your-app-after-changing-model-with-patterns"></a>在对模型进行模式更改后对应用进行培训
添加、编辑、删除或重新分配模式后，请针对更改来[培训](luis-how-to-train.md)和[发布](luis-how-to-publish-app.md)应用以影响终结点查询。

<a name="search-patterns"></a>
<a name="edit-a-pattern"></a>
<a name="reassign-individual-pattern-to-different-intent"></a>
<a name="reassign-several-patterns-to-different-intent"></a>
<a name="delete-a-single-pattern"></a>
<a name="delete-several-patterns"></a>
<a name="filter-pattern-list-by-entity"></a>
<a name="filter-pattern-list-by-intent"></a>
<a name="remove-entity-or-intent-filter"></a>
<a name="add-pattern-from-existing-utterance-on-intent-or-entity-page"></a>

## <a name="use-contextual-toolbar"></a>使用上下文工具栏

使用模式列表上方的上下文工具栏可以：

* 搜索模式
* 编辑模式
* 为其他意向重新分配一个模式
* 为其他意向重新分配多个模式
* 删除单个模式
* 删除多个模式
* 按实体筛选模式列表
* 按意向筛选模式列表
* 删除实体或意向筛选器
* 在意向或实体页上通过现有表述添加模式

## <a name="next-steps"></a>后续步骤

* 了解如何按照教程[生成模式](luis-tutorial-pattern.md)（使用 pattern.any 和角色）。
* 了解如何[培训](luis-how-to-train.md)应用。
