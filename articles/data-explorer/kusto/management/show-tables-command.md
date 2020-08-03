---
title: .show tables - Azure 数据资源管理器 | Microsoft Docs
description: 本文介绍 Azure 数据资源管理器中的 .show tables。
services: data-explorer
author: orspod
ms.author: v-tawe
ms.reviewer: rkarlin
ms.service: data-explorer
ms.topic: reference
origin.date: 02/04/2020
ms.date: 07/01/2020
ms.openlocfilehash: d0cb419b4dd151674202a129b2ef0ae47c715539
ms.sourcegitcommit: c17e965d4ffd82fd7cd86b2648fcb0053a65df00
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/20/2020
ms.locfileid: "86470443"
---
# <a name="show-tables"></a>.show tables

返回一个集合，其中包含数据库中的指定表或所有表。

需要[数据库查看者权限](../management/access-control/role-based-authorization.md)。

```kusto
.show tables
.show tables (T1, ..., Tn)
```

**输出**

| 输出参数 | 类型   | 说明                             |
| ---------------- | ------ | --------------------------------------- |
| TableName        | 字符串 | 表的名称。                  |
| DatabaseName     | String | 表所属的数据库。 |
| 文件夹           | String | 表的文件夹。                     |
| DocString        | String | 一个用来记录表的字符串。         |

**输出示例**

| 表名称 | 数据库名称 | 文件夹    | DocString                                 |
| ---------- | ------------- | --------- | ----------------------------------------- |
| Table1     | DB1           | 日志      | 包含服务日志                    |
| Table2     | DB1           | 报告 |
| Table3     | DB1           |           | 扩展信息                             |
| Table4     | DB2           | 指标   | 包含服务性能信息 |
