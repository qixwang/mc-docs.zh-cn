---
title: count()（聚合函数） - Azure 数据资源管理器 | Microsoft Docs
description: 本文介绍 Azure 数据资源管理器中的 count()（聚合函数）。
services: data-explorer
author: orspod
ms.author: v-tawe
ms.reviewer: alexans
ms.service: data-explorer
ms.topic: reference
origin.date: 06/21/2020
ms.date: 07/31/2020
ms.openlocfilehash: 9a9aa827955a9c929674a826814d8f8830424c61
ms.sourcegitcommit: 4e1bc2e9b2a12dbcc05c52db5dbd1ae290aeb18d
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/01/2020
ms.locfileid: "87509515"
---
# <a name="count-aggregation-function"></a>count()（聚合函数）

返回每个摘要组的记录数（如果没有分组就进行汇总，则返回总数）。

* 只能在 [summarize](summarizeoperator.md) 内的聚合上下文中使用
* 使用 [countif](countif-aggfunction.md) 聚合函数仅对某些谓词返回 `true` 的记录进行计数。

## <a name="syntax"></a>语法

summarize `count()`

## <a name="returns"></a>返回

返回每个摘要组的记录数（如果没有分组就进行汇总，则返回总数）。

## <a name="example"></a>示例

从字母 `W` 开始对州中的事件进行计数：

<!-- csl: https://help.kusto.chinacloudapi.cn/Samples -->
```kusto
StormEvents
| where State startswith "W"
| summarize Count=count() by State
```

|状态|计数|
|---|---|
|西弗吉尼亚州|757|
|怀俄明州|396|
|华盛顿州|261|
|威斯康星州|1850|
