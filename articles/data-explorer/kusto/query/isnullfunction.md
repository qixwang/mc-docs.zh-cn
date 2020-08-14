---
title: isnull() - Azure 数据资源管理器
description: 本文介绍 Azure 数据资源管理器中的 isnull()。
services: data-explorer
author: orspod
ms.author: v-tawe
ms.reviewer: rkarlin
ms.service: data-explorer
ms.topic: reference
origin.date: 02/13/2020
ms.date: 08/06/2020
ms.openlocfilehash: fba4cbccb5286fa98fdbb44eddbe5a86dfcd7c9e
ms.sourcegitcommit: 7ceeca89c0f0057610d998b64c000a2bb0a57285
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/06/2020
ms.locfileid: "87841628"
---
# <a name="isnull"></a>isnull()

计算其唯一参数并返回一个 `bool` 值，指示该参数的计算结果是否为 null 值。

```kusto
isnull(parse_json("")) == true
```

**语法**

`isnull(`Expr`)`

**返回**

true 或 false，具体取决于值是否为 null。

**备注**

* `string` 值不能为 null。 使用 [isempty](./isemptyfunction.md) 可确定类型 `string` 的值是否为空。

|x                |`isnull(x)`|
|-----------------|-----------|
|`""`             |`false`    |
|`"x"`            |`false`    |
|`parse_json("")`  |`true`     |
|`parse_json("[]")`|`false`    |
|`parse_json("{}")`|`false`    |

**示例**

```kusto
T | where isnull(PossiblyNull) | count
```