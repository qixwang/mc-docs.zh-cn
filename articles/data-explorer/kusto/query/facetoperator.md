---
title: facet 运算符 - Azure 数据资源管理器 | Microsoft Docs
description: 本文介绍 Azure 数据资源管理器中的 facet 运算符。
services: data-explorer
author: orspod
ms.author: v-tawe
ms.reviewer: rkarlin
ms.service: data-explorer
ms.topic: reference
origin.date: 02/13/2020
ms.date: 08/06/2020
ms.openlocfilehash: d71ff0db582b17116d12ae47035217afbe5da52c
ms.sourcegitcommit: 7ceeca89c0f0057610d998b64c000a2bb0a57285
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/06/2020
ms.locfileid: "87841399"
---
# <a name="facet-operator"></a>facet 运算符

返回一组表（每个指定列一个表）。
每个表指定其列所采用的值的列表。
可以使用 `with` 子句创建附加表。

**语法**

T `| facet by` ColumnName [`, ` ...] [`with (` filterPipe `)`

**参数**

* ColumnName：输入中的列的名称（要汇总为输出表）。
* filterPipe：一个查询表达式，应用于输入表以生成一个输出。

**返回**

多个表：`with` 子句对应一个表，每个列对应一个表。

**示例**

```kusto
MyTable 
| facet by city, eventType 
    with (where timestamp > ago(7d) | take 1000)
```