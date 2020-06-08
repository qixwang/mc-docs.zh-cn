---
title: Azure Monitor 日志查询中的 resource() 表达式 | Microsoft Docs
description: resource 表达式在以资源为中心的 Azure Monitor 日志查询中使用，可以从多个资源检索数据。
ms.subservice: logs
ms.topic: conceptual
author: Johnnytechn
ms.author: v-johya
ms.date: 05/28/2020
ms.openlocfilehash: 05f217f0dd7396e273d2d5368885900b0c5b6751
ms.sourcegitcommit: be0a8e909fbce6b1b09699a721268f2fc7eb89de
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/29/2020
ms.locfileid: "84199887"
---
# <a name="resource-expression-in-azure-monitor-log-query"></a>Azure Monitor 日志查询中的 resource() 表达式

`resource` 表达式在[范围限定为资源](scope.md#query-scope)的 Azure Monitor 查询中使用，可以从其他资源检索数据。 


## <a name="syntax"></a>语法

`resource(`标识符`)`

## <a name="arguments"></a>参数

- 标识符：资源的资源 ID。

| 标识符 | 说明 | 示例
|:---|:---|:---|
| 资源 | 包括资源的数据。 | resource("/subscriptions/xxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourcesgroups/myresourcegroup/providers/microsoft.compute/virtualmachines/myvm") |
| 资源组或订阅 | 包括资源及其包含的所有资源的数据。  | resource("/subscriptions/xxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourcesgroups/myresourcegroup) |


## <a name="notes"></a>注释

* 你必须具有该资源的读取访问权限。


## <a name="examples"></a>示例

```Kusto
union (Heartbeat),(resource("/subscriptions/xxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourcesgroups/myresourcegroup/providers/microsoft.compute/virtualmachines/myvm").Heartbeat) | summarize count() by _ResourceId, TenantId
```
```Kusto
union (Heartbeat),(resource("/subscriptions/xxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourcesgroups/myresourcegroup).Heartbeat) | summarize count() by _ResourceId, TenantId
```


## <a name="next-steps"></a>后续步骤

- 有关查询范围的详细信息，请参阅 [Azure Monitor Log Analytics 中的日志查询范围和时间范围](scope.md)。

