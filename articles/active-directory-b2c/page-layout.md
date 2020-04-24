---
title: 页面布局版本
titleSuffix: Azure AD B2C
description: 有关自定义策略中 UI 自定义的页面布局版本历史记录。
services: active-directory-b2c
author: mmacy
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: reference
ms.date: 02/20/2020
ms.author: v-junlch
ms.subservice: B2C
ms.openlocfilehash: 0a53e20bc16a5b75248e184bf4f9b497f2f1fa25
ms.sourcegitcommit: c1ba5a62f30ac0a3acb337fb77431de6493e6096
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2020
ms.locfileid: "77531336"
---
# <a name="page-layout-versions"></a>页面布局版本

页面布局包会定期更新，其中包括页面元素的修复和改进。 以下更改日志详述了在每个版本中引入的更改。

[!INCLUDE [active-directory-b2c-public-preview](../../includes/active-directory-b2c-public-preview.md)]

## <a name="200"></a>2.0.0

- 自断言页面 (`selfasserted`)
  - 在自定义策略中添加了对[显示控件](display-controls.md)的支持。

## <a name="120"></a>1.2.0

- 所有页面
  - 辅助功能修复
  - 现在可以在 HTML 标记中添加 `data-preload="true"` 特性来控制 CSS 和 JavaScript 的加载顺序。
    - 在加载 HTML 模板的同时加载链接的 CSS 文件，这样它就不会在加载文件之间“闪烁”。
    - 控制在页面加载前提取和执行 `script` 标记的顺序。
  - 电子邮件字段现在为 `type=email`，移动键盘将提供正确的建议
  - 对 Chrome 转换的支持
- “统一和自断言”页
  - “用户名/电子邮件”和“密码”字段现在使用 `form` HTML 元素，以允许 Edge 和 Internet Explorer (IE) 正确保存此信息。

## <a name="110"></a>1.1.0

- 异常页面 (globalexception)
  - 辅助功能修复
  - 删除了当策略中没有协定时会出现的默认消息
  - 删除了默认的 CSS
- MFA 页面 (multifactor)
  - 删除了“确认代码”按钮
  - 代码的输入字段现在只接受最多六 (6) 个字符的输入
  - 当用户输入 6 位代码时，该页面会自动尝试验证输入的代码，不需用户单击任何按钮
  - 如果代码错误，则会自动清除输入字段
  - 如果三 (3) 次尝试都输入了不正确的代码，B2C 会将一个错误发送回信赖方
  - 辅助功能修复
  - 删除了默认的 CSS
- 自断言页面 (selfasserted)
  - 删除了取消警报
  - 错误元素的 CSS 类
  - 显示/隐藏错误逻辑得到了改进
  - 删除了默认的 CSS
- 统一 SSP (unifiedssp)
  - 添加了“使我保持登录状态”(KMSI) 控件

## <a name="100"></a>1.0.0

- 初始版本

<!-- Update_Description: wording update -->

