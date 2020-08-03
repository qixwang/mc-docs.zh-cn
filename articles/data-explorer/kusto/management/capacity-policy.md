---
title: 容量策略控制命令 - Azure 数据资源管理器 | Microsoft Docs
description: 本文介绍了 Azure 数据资源管理器中的容量策略控制命令。
services: data-explorer
author: orspod
ms.author: v-tawe
ms.reviewer: rkarlin
ms.service: data-explorer
ms.topic: reference
origin.date: 03/02/2020
ms.date: 07/01/2020
ms.openlocfilehash: 8ed8435a6c4f93403577d37826bb080e53a8ae25
ms.sourcegitcommit: c17e965d4ffd82fd7cd86b2648fcb0053a65df00
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/20/2020
ms.locfileid: "86470373"
---
# <a name="capacity-policy-control-commands"></a>容量策略控制命令

以下控制命令可用于管理群集的[容量策略](../management/capacitypolicy.md)。

命令需要 [AllDatabasesAdmin](../management/access-control/role-based-authorization.md) 权限。

## <a name="show-cluster-policy-capacity"></a>显示群集策略容量

```kusto
.show cluster policy capacity
```

显示群集的当前容量策略。

**输出**

| 策略名称    | 实体名称 | 策略                                           | 子实体                       | 实体类型 |
| -------------- | ----------- | ------------------------------------------------ | ------------------------------------ | ----------- |
| CapacityPolicy |             | 表示策略的 JSON 格式字符串 | 群集中的数据库列表 | 群集     |

## <a name="alter-cluster-policy-capacity"></a>更改群集策略容量

```kusto
.alter cluster policy capacity @'{ ... capacity policy JSON representation ... }'
.alter-merge cluster policy capacity @'{ ... capacity policy partial-JSON representation ... }'
```

**注意**：对群集容量策略的更改可能最多需要 1 小时才能生效。

**示例：**

- 显式更改群集策略的所有属性：

```kusto
.alter cluster policy capacity
'{'
  '"IngestionCapacity": {'
    '"ClusterMaximumConcurrentOperations": 512,'
    '"CoreUtilizationCoefficient": 0.75'
  '},'
  '"ExtentsMergeCapacity": {'
    '"MaximumConcurrentOperationsPerNode": 1'
  '},'
  '"ExtentsPurgeRebuildCapacity": {'
    '"MaximumConcurrentOperationsPerNode": 1'
  '},'
  '"ExportCapacity": {'
    '"ClusterMaximumConcurrentOperations": 100,'
    '"CoreUtilizationCoefficient": 0.25'
  '},'
  '"ExtentsPartitionCapacity": {'
    '"ClusterMaximumConcurrentOperations": 4'
  '}'
'}'
```

- 更改群集级别策略的单个属性，并保持所有其他属性不变：

```kusto
.alter-merge cluster policy capacity
'{'
  '"ExtentsPartitionCapacity": {'
    '"MaximumConcurrentOperationsPerNode": 4'
  '}'
'}'
```
