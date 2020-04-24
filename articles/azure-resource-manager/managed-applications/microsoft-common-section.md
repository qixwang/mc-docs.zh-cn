---
title: Section UI 元素
description: 介绍了 Azure 门户的 Microsoft.Common.Section UI 元素。 用于对门户中的元素进行分组，以便部署托管应用程序。
author: rockboyfor
ms.topic: conceptual
origin.date: 06/27/2018
ms.date: 01/20/2020
ms.author: v-yeche
ms.openlocfilehash: 35b3251f4d6bfad14691455b93dfed36c3931947
ms.sourcegitcommit: c1ba5a62f30ac0a3acb337fb77431de6493e6096
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2020
ms.locfileid: "76170668"
---
# <a name="microsoftcommonsection-ui-element"></a>Microsoft.Common.Section UI 元素

一个将一个或多个元素分组到同一标题下的控件。

## <a name="ui-sample"></a>UI 示例

![Microsoft.Common.Section](./media/managed-application-elements/microsoft.common.section.png)

## <a name="schema"></a>架构

```json
{
  "name": "section1",
  "type": "Microsoft.Common.Section",
  "label": "Example section",
  "elements": [
    {
      "name": "text1",
      "type": "Microsoft.Common.TextBox",
      "label": "Example text box 1"
    },
    {
      "name": "text2",
      "type": "Microsoft.Common.TextBox",
      "label": "Example text box 2"
    }
  ],
  "visible": true
}
```

## <a name="remarks"></a>备注

- `elements` 必须至少具有一个元素，并且可以具有除 `Microsoft.Common.Section` 之外的所有元素类型。
- 此元素不支持 `toolTip` 属性。

## <a name="sample-output"></a>示例输出
若要访问 `elements` 中的元素的输出值，请使用 [basics()](create-uidefinition-functions.md#basics) 或 [steps()](create-uidefinition-functions.md#steps) 函数和点表示法：

```json
steps('configuration').section1.text1
```

`Microsoft.Common.Section` 类型的元素本身没有输出值。

## <a name="next-steps"></a>后续步骤

* 有关创建 UI 定义的简介，请参阅 [CreateUiDefinition 入门](create-uidefinition-overview.md)。
* 有关 UI 元素中的公用属性的说明，请参阅 [CreateUiDefinition 元素](create-uidefinition-elements.md)。

<!-- Update_Description: new article about microsoft common section -->
<!--NEW.date: 01/20/2020-->