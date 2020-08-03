---
title: 沙盒策略 - Azure 数据资源管理器
description: 本文介绍 Azure 数据资源管理器中的沙盒策略。
services: data-explorer
author: orspod
ms.author: v-tawe
ms.reviewer: rkarlin
ms.service: data-explorer
ms.topic: reference
origin.date: 03/24/2020
ms.date: 07/01/2020
ms.openlocfilehash: 575ac171e5d88f9ed5a552aa03475a0c2402cf87
ms.sourcegitcommit: c17e965d4ffd82fd7cd86b2648fcb0053a65df00
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/20/2020
ms.locfileid: "86470420"
---
# <a name="sandbox-policy"></a>沙盒策略

Azure 数据资源管理器会在[沙盒](../concepts/sandboxes.md)中运行某些插件，这些插件的可用资源受到限制和控制，以确保安全性并进行资源管理。

沙盒在 Kusto 引擎节点上运行。 它们的某些限制在沙盒策略中定义，其中的每种沙盒类型都可以有自己的策略。

沙盒策略在群集级别进行管理，并影响群集中的所有节点。

若要更改策略，你需要 [AllDatabasesAdmin](../management/access-control/role-based-authorization.md) 权限。

## <a name="the-policy-object"></a>策略对象

沙盒策略具有以下属性。

- **SandboxKind**：定义沙盒的类型（如 `PythonExecution`、`RExecution`）。
- **IsEnabled**：定义此类型的沙箱是否可以在群集的节点上运行。
- **TargetCountPerNode**：定义允许在群集节点上运行的此类型的沙盒数。
  - 值可以是每个节点的处理器数的一到两倍。
  - 默认值为 16。
- **MaxCpuRatePerSandbox**：将最大 CPU 使用率定义为单个沙盒可以使用的所有可用核心的百分比。
  - 值可以介于 1 到 100 之间。
  - 默认值为 50。
- **MaxMemoryMbPerSandbox**：定义单个沙盒可以使用的最大内存量（以 MB 为单位）。
  - 值可以介于 200 到 65536 (64GB) 之间。
  - 默认值为 20480 (20GB)。

## <a name="example"></a>示例

以下策略为 `PythonExecution` 和 `RExecution` 沙盒设置不同的限制：

```json
[
  {
    "SandboxKind": "PythonExecution",
    "IsEnabled": true,
    "TargetCountPerNode": 4,
    "MaxCpuRatePerSandbox": 55,
    "MaxMemoryMbPerSandbox": 65536
  },
  {
    "SandboxKind": "RExecution",
    "IsEnabled": true,
    "TargetCountPerNode": 2,
    "MaxCpuRatePerSandbox": 50,
    "MaxMemoryMbPerSandbox": 10240
  }
]
```

> [!NOTE]
>
> - 对沙盒策略的更改将应用于从应用更改时开始创建的沙盒。 在策略更改之前预分配的沙盒会继续按照以前的策略限制来运行，直到在查询中使用它们为止。
> - 在策略更改生效之前，可能会有最多五分钟的延迟，因为群集节点会定期轮询策略更改。

## <a name="next-steps"></a>后续步骤

使用[沙盒策略控制命令](../management/sandbox-policy.md)管理群集的沙盒策略。
