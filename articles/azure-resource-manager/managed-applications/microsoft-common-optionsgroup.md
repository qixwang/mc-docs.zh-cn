---
title: OptionsGroup UI 元素
description: 介绍了 Azure 门户的 Microsoft.Common.OptionsGroup UI 元素。 使用户能够在部署托管应用程序时从可用选项中进行选择。
author: rockboyfor
ms.topic: conceptual
origin.date: 06/27/2018
ms.date: 01/20/2020
ms.author: v-yeche
ms.openlocfilehash: dbb9d1c45c5db9a873d3cd55f280a76c25d437ca
ms.sourcegitcommit: c1ba5a62f30ac0a3acb337fb77431de6493e6096
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2020
ms.locfileid: "76170677"
---
# <a name="microsoftcommonoptionsgroup-ui-element"></a>Microsoft.Common.OptionsGroup UI 元素

一个具有一行可用选项的选择控件。

## <a name="ui-sample"></a>UI 示例

![Microsoft.Common.OptionsGroup](./media/managed-application-elements/microsoft.common.optionsgroup.png)

## <a name="schema"></a>架构

```json
{
  "name": "element1",
  "type": "Microsoft.Common.OptionsGroup",
  "label": "Some options group",
  "defaultValue": "Value two",
  "toolTip": "",
  "constraints": {
    "allowedValues": [
      {
        "label": "Value one",
        "value": "one"
      },
      {
        "label": "Value two",
        "value": "two"
      }
    ],
    "required": true
  },
  "visible": true
}
```

## <a name="sample-output"></a>示例输出

```json
"two"
```

## <a name="remarks"></a>备注

- `constraints.allowedValues` 的标签是项的显示文本，并且当选中时，其值是元素的输出值。
- 如果已指定，则默认值必须是 `constraints.allowedValues` 中存在的一个标签。 如果未指定，则默认情况下将选择 `constraints.allowedValues` 中的第一个项。 默认值为 **null**。
- `constraints.allowedValues` 必须至少有一项。

## <a name="next-steps"></a>后续步骤

* 有关创建 UI 定义的简介，请参阅 [CreateUiDefinition 入门](create-uidefinition-overview.md)。
* 有关 UI 元素中的公用属性的说明，请参阅 [CreateUiDefinition 元素](create-uidefinition-elements.md)。

<!-- Update_Description: new article about microsoft common optionsgroup -->
<!--NEW.date: 01/20/2020-->