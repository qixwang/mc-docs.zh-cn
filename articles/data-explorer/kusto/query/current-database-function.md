---
title: current_database() - Azure 数据资源管理器 | Microsoft Docs
description: 本文介绍 Azure 数据资源管理器中的 current_database()。
services: data-explorer
author: orspod
ms.author: v-tawe
ms.reviewer: rkarlin
ms.service: data-explorer
ms.topic: reference
origin.date: 02/13/2020
ms.date: 07/31/2020
ms.openlocfilehash: bb9c6ba9005a8916269514c63e143d5514626c20
ms.sourcegitcommit: 4e1bc2e9b2a12dbcc05c52db5dbd1ae290aeb18d
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/01/2020
ms.locfileid: "87509246"
---
# <a name="current_database"></a>current_database()

返回作用域中数据库的名称（如果未指定其他数据库，则将解析所有查询实体的数据库）。

## <a name="syntax"></a>语法

`current_database()`

## <a name="returns"></a>返回

作用域中数据库的名称作为 `string` 类型的值。

## <a name="example"></a>示例

```kusto
print strcat("Database in scope: ", current_database())
```