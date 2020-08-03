---
title: 容量策略 - Azure 数据资源管理器
description: 本文介绍 Azure 数据资源管理器中的容量策略。
services: data-explorer
author: orspod
ms.author: v-tawe
ms.reviewer: rkarlin
ms.service: data-explorer
ms.topic: reference
origin.date: 03/12/2020
ms.date: 07/01/2020
ms.openlocfilehash: 8228019a1453f788e3386afe662dd870862e1768
ms.sourcegitcommit: c17e965d4ffd82fd7cd86b2648fcb0053a65df00
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/20/2020
ms.locfileid: "86470365"
---
# <a name="capacity-policy"></a>容量策略

容量策略用于控制群集上数据管理操作的计算资源。

## <a name="the-capacity-policy-object"></a>容量策略对象

容量策略由以下内容组成：

- [IngestionCapacity](#ingestion-capacity)
- [ExtentsMergeCapacity](#extents-merge-capacity)
- [ExtentsPurgeRebuildCapacity](#extents-purge-rebuild-capacity)
- [ExportCapacity](#export-capacity)
- [ExtentsPartitionCapacity](#extents-partition-capacity)

## <a name="ingestion-capacity"></a>引入容量

| 属性                           | 类型   | 说明                                                                                                                                                                          |
| ---------------------------------- | ------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| ClusterMaximumConcurrentOperations | long   | 群集中并发引入操作数的最大值                                                                                                       |
| CoreUtilizationCoefficient         | Double | 计算引入容量时使用的核心百分比系数。 计算得出的结果将始终由 `ClusterMaximumConcurrentOperations` 进行标准化 |

群集总引入容量（由 [.show capacity](../management/diagnostics.md#show-capacity) 显示）的计算方式为：

Minimum(`ClusterMaximumConcurrentOperations`, `Number of nodes in cluster` _ Maximum(1, `Core count per node` _ `CoreUtilizationCoefficient`))

> [!Note]
> 在具有三个或以上节点的群集中，管理节点不参与引入操作。 `Number of nodes in cluster` 减少 1 个。

## <a name="extents-merge-capacity"></a>盘区合并容量

| 属性                           | 类型 | 说明                                                                                                  |
| ---------------------------------- | ---- | ------------------------------------------------------------------------------------------------------------ |
| MinimumConcurrentOperationsPerNode | long | 单个节点上并发盘区合并/重新生成操作数的最小值。 默认值为 1 |
| MaximumConcurrentOperationsPerNode | long | 单个节点上并发盘区合并/重新生成操作数的最大值。 默认值为 5 |

群集总盘区合并容量（由 [.show capacity](../management/diagnostics.md#show-capacity) 显示）的计算方式为：

`Number of nodes in cluster` x `Concurrent operations per node`

`Concurrent operations per node` 的有效值在 [`MinimumConcurrentOperationsPerNode`,`MaximumConcurrentOperationsPerNode`] 范围中由系统自动调整。

> [!Note]
>
> - 在具有三个或以上节点的群集中，管理节点不参与合并操作。 `Number of nodes in cluster` 减少 1 个。

## <a name="extents-purge-rebuild-capacity"></a>盘区清除重新生成容量

| 属性                           | 类型 | 说明                                                                                        |
| ---------------------------------- | ---- | -------------------------------------------------------------------------------------------------- |
| MaximumConcurrentOperationsPerNode | long | 单个节点上清除操作的并发重新生成盘区数的最大值 |

群集总盘区清除重新生成容量（由 [.show capacity](../management/diagnostics.md#show-capacity) 显示）的计算方式为：

`Number of nodes in cluster` x `MaximumConcurrentOperationsPerNode`

> [!Note]
> 在具有三个或以上节点的群集中，管理节点不参与合并操作。 `Number of nodes in cluster` 减少 1 个。

## <a name="export-capacity"></a>导出容量

| 属性                           | 类型   | 说明                                                                                                                                                                        |
| ---------------------------------- | ------ | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| ClusterMaximumConcurrentOperations | long   | 群集中并发导出操作数的最大值。                                                                                                       |
| CoreUtilizationCoefficient         | Double | 计算导出容量时使用的核心百分比系数。 计算得出的结果将始终由 `ClusterMaximumConcurrentOperations` 进行标准化。 |

群集总导出容量（由 [.show capacity](../management/diagnostics.md#show-capacity) 显示）的计算方式为：

Minimum(`ClusterMaximumConcurrentOperations`, `Number of nodes in cluster` _ Maximum(1, `Core count per node` _ `CoreUtilizationCoefficient`))

> [!Note]
> 在具有三个或以上节点的群集中，管理节点不参与导出操作。 `Number of nodes in cluster` 减少 1 个。

## <a name="extents-partition-capacity"></a>盘区分区容量

| 属性                           | 类型 | 说明                                                                                         |
| ---------------------------------- | ---- | --------------------------------------------------------------------------------------------------- |
| ClusterMinimumConcurrentOperations | long | 群集中并发盘区分区操作数的最小值。 默认值：1  |
| ClusterMaximumConcurrentOperations | long | 群集中并发盘区分区操作数的最大值。 默认值：16 |

群集总盘区分区容量（由 [.show capacity](../management/diagnostics.md#show-capacity) 显示）。

`Concurrent operations` 的有效值在 [`ClusterMinimumConcurrentOperations`,`ClusterMaximumConcurrentOperations`] 范围中由系统自动调整。

## <a name="defaults"></a>默认值

默认容量策略具有以下 JSON 表示形式：

```kusto
{
  "IngestionCapacity": {
    "ClusterMaximumConcurrentOperations": 512,
    "CoreUtilizationCoefficient": 0.75
  },
  "ExtentsMergeCapacity": {
    "MaximumConcurrentOperationsPerNode": 1
  },
  "ExtentsPurgeRebuildCapacity": {
    "MaximumConcurrentOperationsPerNode": 1
  },
  "ExportCapacity": {
    "ClusterMaximumConcurrentOperations": 100,
    "CoreUtilizationCoefficient": 0.25
  }
}
```

## <a name="control-commands"></a>控制命令

> [!WARNING]
> 更改容量策略之前，请咨询 Azure 数据资源管理器团队。

- 使用 [.show cluster policy capacity](capacity-policy.md#show-cluster-policy-capacity) 显示群集当前的容量策略。

- 使用 [.alter cluster policy capacity](capacity-policy.md#alter-cluster-policy-capacity) 更改群集的容量策略。

## <a name="throttling"></a>限制

Kusto 限制以下用户启动命令的并发请求数：

- 引入（包括[此处](../../ingest-data-overview.md)列出的所有命令）
  - [容量策略](#capacity-policy)中定义了限制。
- 清除
  - 当前全局固定为每个群集一个。
  - 清除重新生成容量在内部用于确定使用清除命令期间的并发重新生成操作数。 清除命令不会因此过程而被阻止/限制，但将以更快或更慢的速度运行，具体取决于清除重新生成容量。
- 导出
  - [容量策略](#capacity-policy)中定义了限制。

当群集检测到某个操作具有超过允许数量的并发操作时，它将使用 429“受限制”HTTP 代码进行响应。
回退后重试操作。
