---
title: 分片策略管理 - Azure 数据资源管理器 | Microsoft Docs
description: 本文介绍 Azure 数据资源管理器中的分片策略管理。
services: data-explorer
author: orspod
ms.author: v-tawe
ms.reviewer: rkarlin
ms.service: data-explorer
ms.topic: reference
origin.date: 02/13/2020
ms.date: 07/01/2020
ms.openlocfilehash: 94c8028d9e3386bd9db182095483e966c504107b
ms.sourcegitcommit: c17e965d4ffd82fd7cd86b2648fcb0053a65df00
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/20/2020
ms.locfileid: "86470540"
---
# <a name="sharding-policy-management"></a>分片策略管理

## <a name="show-policy"></a>显示策略

```kusto
.show table [table_name] policy sharding

.show table * policy sharding

.show database [database_name] policy sharding
```

`Show` 策略显示数据库或表的分片策略。 如果给定的名称为“\*”，它将显示给定实体类型（数据库或表）的所有策略。

### <a name="output"></a>输出

| 策略名称           | 实体名称           | 策略                                        | 子实体                  | 实体类型      |
| --------------------- | --------------------- | --------------------------------------------- | ------------------------------- | ---------------- |
| ExtentsShardingPolicy | 数据库/表名称 | 表示策略的 json 格式字符串 | 表列表（对于数据库） | 数据库/表 |

## <a name="alter-policy"></a>更改策略

### <a name="examples"></a>示例

下面的示例返回实体更新后的盘区分片策略，并将数据库或表指定为限定名称作为其输出。

#### <a name="setting-all-properties-of-the-policy-explicitly-at-table-level"></a>在表级别显式设置策略的所有属性

```kusto
.alter table [table_name] policy sharding
@'{ "MaxRowCount": 750000, "MaxExtentSizeInMb": 1024, "MaxOriginalSizeInMb": 2048}'
```

#### <a name="setting-all-properties-of-the-policy-explicitly-at-database-level"></a>在数据库级别显式设置策略的所有属性

```kusto
.alter database [database_name] policy sharding
@'{ "MaxRowCount": 750000, "MaxExtentSizeInMb": 1024, "MaxOriginalSizeInMb": 2048}'
```

#### <a name="setting-the-_default_-sharding-policy-at-database-level"></a>在数据库级别设置默认分片策略

```kusto
.alter database [database_name] policy sharding @'{}'
```

#### <a name="altering-a-single-property-of-the-policy-at-database-level"></a>在数据库级别更改策略的单个属性

所有其他属性保持不变。

```kusto
.alter-merge database [database_name] policy sharding
@'{ "MaxExtentSizeInMb": 1024}'
```

#### <a name="altering-a-single-property-of-the-policy-at-table-level"></a>在表级别更改策略的单个属性

所有其他属性保持不变

```kusto
.alter-merge table [table_name] policy sharding
@'{ "MaxRowCount": 750000}'
```

## <a name="delete-policy"></a>删除策略

```kusto
.delete table [table_name] policy sharding

.delete database [database_name] policy sharding
```

此命令删除给定实体的当前分片策略。
