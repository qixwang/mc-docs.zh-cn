---
title: project-rename 运算符 - Azure 数据资源管理器
description: 本文介绍 Azure 数据资源管理器中的 project-rename 运算符。
services: data-explorer
author: orspod
ms.author: v-tawe
ms.reviewer: rkarlin
ms.service: data-explorer
ms.topic: reference
origin.date: 02/13/2020
ms.date: 08/06/2020
ms.openlocfilehash: ee7b23ebdf9f12c7ef46e6769338f9a398c27fe2
ms.sourcegitcommit: 7ceeca89c0f0057610d998b64c000a2bb0a57285
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/06/2020
ms.locfileid: "87841631"
---
# <a name="project-rename-operator"></a>project-rename 运算符

对结果输出中的列重命名。

```kusto
T | project-rename new_column_name = column_name
```

**语法**

*T* `| project-rename` *NewColumnName* = *ExistingColumnName* [`,` ...]

**参数**

* *T*：输入表。
* NewColumnName：列的新名称。 
* ExistingColumnName：列的现有名称。 

**返回**

一个表，其列的顺序与现有表中的顺序相同，但列已重命名。


**示例**

<!-- csl: https://help.kusto.chinacloudapi.cn/Samples -->
```kusto
print a='a', b='b', c='c'
|  project-rename new_b=b, new_a=a
```

|new_a|new_b|c|
|---|---|---|
|a|b|c|
