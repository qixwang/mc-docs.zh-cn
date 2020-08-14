---
title: 预览插件 - Azure 数据资源管理器 | Microsoft Docs
description: 本文介绍了 Azure 数据资源管理器中的预览插件。
services: data-explorer
author: orspod
ms.author: v-tawe
ms.reviewer: rkarlin
ms.service: data-explorer
ms.topic: reference
origin.date: 02/13/2020
ms.date: 08/06/2020
ms.openlocfilehash: 0cdf3abea0cda5da2850e95b7f6ecfdb5186d8ca
ms.sourcegitcommit: 7ceeca89c0f0057610d998b64c000a2bb0a57285
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/06/2020
ms.locfileid: "87841667"
---
# <a name="preview-plugin"></a>preview 插件

返回一个表，其中包含输入记录集中的指定行数以及输入记录集中的记录总数。

```kusto
T | evaluate preview(50)
```

**语法**

`T` `|` `evaluate` `preview(` *NumberOfRows* `)`

**返回**

`preview` 插件返回两个结果表：
* 最多包含指定行数的表。
  例如，上面的示例查询相当于运行 `T | take 50`。
* 只含有一行/列的表，用于保存输入记录集中的记录数。
  例如，上面的示例查询相当于运行 `T | count`。

**提示**

如果 `evaluate` 前面有一个包含复杂筛选器的表格式源，或者引用大多数源表列的筛选器，则最好使用 [`materialize`](materializefunction.md) 函数。 例如：

```kusto
let MaterializedT = materialize(T);
MaterializedT | evaluate preview(50)
```