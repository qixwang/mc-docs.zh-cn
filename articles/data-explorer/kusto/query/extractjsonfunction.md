---
title: extractjson() - Azure 数据资源管理器 | Microsoft Docs
description: 本文介绍 Azure 数据资源管理器中的 extractjson()。
services: data-explorer
author: orspod
ms.author: v-tawe
ms.reviewer: rkarlin
ms.service: data-explorer
ms.topic: reference
origin.date: 02/13/2020
ms.date: 08/06/2020
ms.openlocfilehash: 96e08151ffb826a1978106232ef254e66a15e3b9
ms.sourcegitcommit: 7ceeca89c0f0057610d998b64c000a2bb0a57285
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/06/2020
ms.locfileid: "87841401"
---
# <a name="extractjson"></a>extractjson()

使用路径表达式获取 JSON 文本外指定的元素。 

（可选）将提取的字符串转换为特定类型。

```kusto
extractjson("$.hosts[1].AvailableMB", EventText, typeof(int))
```

**语法**

`extractjson(`*jsonPath*`,` *dataSource*`)` 

**参数**

* jsonPath：在 JSON 文档中定义访问器的 JsonPath 字符串。
* dataSource：JSON 文档。

**返回**

此函数对 dataSource（内含有效的 JSON 字符串）执行 JsonPath 查询，从而可选择根据第三个参数将该值转换为其他类型。

**示例**

`[`方括号`]`表示法和点 (`.`) 表示法等效：

```kusto
T 
| extend AvailableMB = extractjson("$.hosts[1].AvailableMB", EventText, typeof(int)) 

T
| extend AvailableMD = extractjson("$['hosts'][1]['AvailableMB']", EventText, typeof(int)) 
```

### <a name="json-path-expressions"></a>JSON 路径表达式

|||
|---|---|
|`$`|根对象|
|`@`|当前对象|
|`.` 或 `[ ]` | 子|
|`[ ]`|数组下标|

（目前未实现通配符、递归、联合或切片。）**


**性能提示**

* 使用 `extractjson()` 之前应用 where 子句
* 请考虑改用与 [extract](extractfunction.md) 匹配的正则表达式匹配项。 如果从模板生成 JSON，运行速度则更快并且高效。
* 如果需要从 JSON 提取多个值，请使用 `parse_json()`。
* 考虑将列类型声明为动态，以便在引入时分析 JSON。