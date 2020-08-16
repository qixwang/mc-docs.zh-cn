---
title: has_any 运算符 - Azure 数据资源管理器
description: 本文介绍 Azure 数据资源管理器中的 has_any 运算符。
services: data-explorer
author: orspod
ms.author: v-tawe
ms.reviewer: rkarlin
ms.service: data-explorer
ms.topic: reference
origin.date: 08/11/2019
ms.date: 08/06/2020
ms.openlocfilehash: 835c1d8dacff7366297cc05f7872eed52d506ca7
ms.sourcegitcommit: 7ceeca89c0f0057610d998b64c000a2bb0a57285
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/06/2020
ms.locfileid: "87841745"
---
# <a name="has_any-operator"></a>has_any 运算符

`has_any` 运算符基于提供的一组值进行筛选。

```kusto
Table1 | where col has_any ('value1', 'value2')
```

**语法**

*T* `|` `where` *col* `has_any` `(`*list of scalar expressions*`)`   
*T* `|` `where` *col* `has_any` `(`*tabular expression*`)`   
 
**参数**

* T - 其记录待筛选的表格输入。
* col - 要筛选的列。
* list of expressions - 以逗号分隔的表格、标量或文本表达式的列表  
* tabular expression - 包含一组值的表格表达式（如果 expression 有多个列，则使用第一列）

**返回**

其谓词为 `true` 的 T 中的行

**备注**

* 表达式列表最多可生成 `10,000` 个值。    
* 对于表格表达式，会选择结果集的第一列。   

**示例：**  

`has_any` 运算符的简单用法：  

<!-- csl: https://help.kusto.chinacloudapi.cn/Samples -->
```kusto
StormEvents 
| where State has_any ("CAROLINA", "DAKOTA", "NEW") 
| summarize count() by State
```

|状态|count_|
|---|---|
|NEW YORK|1750|
|NORTH CAROLINA|1721|
|SOUTH DAKOTA|1567|
|NEW JERSEY|1044|
|SOUTH CAROLINA|915|
|NORTH DAKOTA|905|
|NEW MEXICO|527|
|NEW HAMPSHIRE|394|


使用动态数组：

<!-- csl: https://help.kusto.chinacloudapi.cn/Samples -->
```kusto
let states = dynamic(['south', 'north']);
StormEvents 
| where State has_any (states)
| summarize count() by State
```

|状态|count_|
|---|---|
|NORTH CAROLINA|1721|
|SOUTH DAKOTA|1567|
|SOUTH CAROLINA|915|
|NORTH DAKOTA|905|
|大西洋南部|193|
|ATLANTIC NORTH|188|
