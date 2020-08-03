---
title: 数据分区策略管理 - Azure 数据资源管理器 | Microsoft Docs
description: 本文介绍 Azure 数据资源管理器中的数据分区策略管理。
services: data-explorer
author: orspod
ms.author: v-tawe
ms.reviewer: rkarlin
ms.service: data-explorer
ms.topic: reference
origin.date: 03/04/2020
ms.date: 07/01/2020
ms.openlocfilehash: bda83e295d57bc6276044a5a6891192b427a6508
ms.sourcegitcommit: c17e965d4ffd82fd7cd86b2648fcb0053a65df00
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/20/2020
ms.locfileid: "86470348"
---
# <a name="data-partitioning-policy-management"></a>数据分区策略管理

[此处](../management/partitioningpolicy.md)详细介绍了数据分区策略。

## <a name="show-policy"></a>显示策略

```kusto
.show table [table_name] policy partitioning
```

`.show` 命令显示应用于表的分区策略。

### <a name="output"></a>输出

| 策略名称      | 实体名称 | 策略                                  | 子实体 | 实体类型 |
| ---------------- | ----------- | --------------------------------------- | -------------- | ----------- |
| DataPartitioning | 表名称  | 策略对象的 JSON 序列化 | Null           | 表       |

## <a name="alter-and-alter-merge-policy"></a>alter 和 alter-merge policy

```kusto
.alter table [table_name] policy partitioning @'policy object, serialized as JSON'

.alter-merge table [table_name] policy partitioning @'partial policy object, serialized as JSON'
```

`.alter` 命令允许更改应用于表的分区策略。

此命令需要 [DatabaseAdmin](access-control/role-based-authorization.md) 权限。

对策略的更改可能最多需要 1 小时才能生效。

### <a name="examples"></a>示例

#### <a name="setting-a-policy-with-a-hash-partition-key"></a>使用哈希分区键设置策略

```kusto
.alter table [table_name] policy partitioning @'{'
  '"PartitionKeys": ['
    '{'
      '"ColumnName": "my_string_column",'
      '"Kind": "Hash",'
      '"Properties": {'
        '"Function": "XxHash64",'
        '"MaxPartitionCount": 256,'
      '}'
    '}'
  ']'
'}'
```

#### <a name="setting-a-policy-with-a-uniform-range-datetime-partition-key"></a>使用统一范围日期/时间分区键设置策略

```kusto
.alter table [table_name] policy partitioning @'{'
  '"PartitionKeys": ['
    '{'
      '"ColumnName": "my_datetime_column",'
      '"Kind": "UniformRange",'
      '"Properties": {'
        '"Reference": "1970-01-01T00:00:00",'
        '"RangeSize": "1.00:00:00"'
      '}'
    '}'
  ']'
'}'
```

#### <a name="setting-a-policy-with-both-kinds-of-partition-keys"></a>使用两种分区键设置策略

```kusto
.alter table [table_name] policy partitioning @'{'
  '"PartitionKeys": ['
    '{'
      '"ColumnName": "my_string_column",'
      '"Kind": "Hash",'
      '"Properties": {'
        '"Function": "XxHash64",'
        '"MaxPartitionCount": 256,'
      '}'
    '},'
    '{'
      '"ColumnName": "my_datetime_column",'
      '"Kind": "UniformRange",'
      '"Properties": {'
        '"Reference": "1970-01-01T00:00:00",'
        '"RangeSize": "1.00:00:00"'
      '}'
    '}'
  ']'
'}'
```

#### <a name="setting-a-specific-property-of-the-policy-explicitly-at-table-level"></a>在表级别显式设置策略的特定属性

要将策略的 `EffectiveDateTime` 设置为其他值，请使用以下命令：

```kusto
.alter-merge table [table_name] policy partitioning @'{"EffectiveDateTime":"2020-01-01"}'
```

## <a name="delete-policy"></a>删除策略

```kusto
.delete table [table_name] policy partitioning
```

`.delete` 命令删除给定表的分区策略。
