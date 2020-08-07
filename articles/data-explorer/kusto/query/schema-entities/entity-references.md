---
title: 实体引用 - Azure 数据资源管理器
description: 本文介绍 Azure 数据资源管理器中的实体引用。
services: data-explorer
author: orspod
ms.author: v-tawe
ms.reviewer: rkarlin
ms.service: data-explorer
ms.topic: reference
origin.date: 02/13/2020
ms.date: 07/31/2020
ms.openlocfilehash: 6472432216a463e9e1fe4ae48f7f1997511bb087
ms.sourcegitcommit: 4e1bc2e9b2a12dbcc05c52db5dbd1ae290aeb18d
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/01/2020
ms.locfileid: "87509237"
---
# <a name="entity-references"></a>实体引用

在查询中使用 Kusto 架构实体的名称来引用它们。 有效的实体名称包括数据库、表、列和存储函数  。 群集不能通过其名称进行引用。
如果实体容器在当前上下文中是明确的，请使用实体名称而无需附加限定条件。 例如，当对名为 `DB` 的数据库运行查询时，可以通过名称 `T` 引用该数据库中名为 `T` 的表。

如果实体容器在上下文中不可用，或者你希望从与上下文中的容器不同的容器中引用实体，请使用实体的限定名称。
此名称是实体名称与容器名称（可能还有该容器的容器的名称，依此类推）的串联。 通过这种方式，针对数据库 `DB` 运行的查询可以通过使用 `database("DB1").T1` 引用同一群集的不同数据库 `DB1` 中的表 `T1`。 如果查询要引用另一个群集中的表，可通过使用 `cluster("https://C2.kusto.chinacloudapi.cn/").database("DB2").T2` 之类的方法来实现此目的。

实体引用也可以使用实体友好名称，前提是它在实体容器的上下文中是唯一的。 有关详细信息，请参阅[实体友好名称](./entity-names.md#entity-pretty-names)。

## <a name="wildcard-matching-for-entity-names"></a>实体名称的通配符匹配

在某些上下文中，可以使用通配符 (`*`) 来匹配全部或部分实体名称。 例如，以下查询引用当前数据库中的所有表以及数据库 `DB` 中名称以 `T` 开头的所有表：

```kusto
union *, database("DB1").T*
```

> [!NOTE]
> 通配符匹配不能匹配以美元符号 (`$`) 开头的实体名称。
此类名称是系统保留的。

## <a name="next-steps"></a>后续步骤

* [架构实体类型](https://docs.microsoft.com/azure/data-explorer/kusto/query/schema-entities/)
* [架构实体名称](https://docs.microsoft.com/azure/data-explorer/kusto/query/schema-entities/entity-names)
