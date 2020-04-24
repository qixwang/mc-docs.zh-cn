---
title: PasswordBox UI 元素
description: 介绍了 Azure 门户的 Microsoft.Common.PasswordBox UI 元素。 使用户能够在部署托管应用程序时提供机密值。
author: rockboyfor
ms.topic: conceptual
origin.date: 06/27/2018
ms.date: 01/20/2020
ms.author: v-yeche
ms.openlocfilehash: 28e533fa1094cad1579748a500969855656ec11b
ms.sourcegitcommit: c1ba5a62f30ac0a3acb337fb77431de6493e6096
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2020
ms.locfileid: "76170672"
---
# <a name="microsoftcommonpasswordbox-ui-element"></a>Microsoft.Common.PasswordBox UI 元素

一个可以用来提供和确认密码的控件。

## <a name="ui-sample"></a>UI 示例

![Microsoft.Common.PasswordBox](./media/managed-application-elements/microsoft.common.passwordbox.png)

## <a name="schema"></a>架构

```json
{
  "name": "element1",
  "type": "Microsoft.Common.PasswordBox",
  "label": {
    "password": "Password",
    "confirmPassword": "Confirm password"
  },
  "toolTip": "",
  "constraints": {
    "required": true,
    "regex": "^[a-zA-Z0-9]{8,}$",
    "validationMessage": "Password must be at least 8 characters long, contain only numbers and letters"
  },
  "options": {
    "hideConfirmation": false
  },
  "visible": true
}
```

## <a name="sample-output"></a>示例输出

```json
"p4ssw0rd"
```

## <a name="remarks"></a>备注

- 此元素不支持 `defaultValue` 属性。
- 有关 `constraints` 的实现详细信息，请参阅 [Microsoft.Common.TextBox](microsoft-common-textbox.md)。
- 如果 `options.hideConfirmation` 设置为 **true**，则用于确认用户密码的第二个文本框会被隐藏。 默认值是 **false**秒。

## <a name="next-steps"></a>后续步骤

* 有关创建 UI 定义的简介，请参阅 [CreateUiDefinition 入门](create-uidefinition-overview.md)。
* 有关 UI 元素中的公用属性的说明，请参阅 [CreateUiDefinition 元素](create-uidefinition-elements.md)。

<!-- Update_Description: new article about microsoft common passwordbox -->
<!--NEW.date: 01/20/2020-->