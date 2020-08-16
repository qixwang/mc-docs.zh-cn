---
title: 正则表达式 - Azure 数据资源管理器 | Microsoft Docs
description: 本文介绍 Azure 数据资源管理器中的正则表达式。
services: data-explorer
author: orspod
ms.author: v-tawe
ms.reviewer: rkarlin
ms.service: data-explorer
ms.topic: reference
origin.date: 12/09/2019
ms.date: 08/06/2020
ms.openlocfilehash: 120695f642e4454db2bbeaff0808648239629025
ms.sourcegitcommit: 7ceeca89c0f0057610d998b64c000a2bb0a57285
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/06/2020
ms.locfileid: "87841509"
---
# <a name="regular-expressions"></a>正则表达式

RE2 正则表达式语法描述 Kusto (re2) 使用的正则表达式库的语法。
Kusto 中有一些函数使用正则表达式执行字符串匹配、选择和提取

- [countof()](countoffunction.md)
- [extract()](extractfunction.md)
- [extract_all()](extractallfunction.md)
- [matches regex](datatypes-string-operators.md)
- [parse 运算符](parseoperator.md)
- [replace()](replacefunction.md)
- [trim()](trimfunction.md)
- [trimend()](trimendfunction.md)
- [trimstart()](trimstartfunction.md)

Kusto 支持的正则表达式语法是 [re2 库](https://github.com/google/re2/wiki/Syntax)的语法。 这些表达式必须在 Kusto 中编码为 `string` 文本，Kusto 的所有字符串引用规则都适用。 例如，正则表达式 `\A` 匹配一行的开头，并且在 Kusto 中指定为字符串文本 `"\\A"`（请注意“额外的”反斜杠 (`\`) 字符）。
