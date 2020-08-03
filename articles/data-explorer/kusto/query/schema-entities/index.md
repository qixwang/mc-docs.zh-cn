---
title: 实体 - Azure 数据资源管理器 | Microsoft Docs
description: 本文介绍 Azure 数据资源管理器中的实体。
services: data-explorer
author: orspod
ms.author: v-tawe
ms.reviewer: rkarlin
ms.service: data-explorer
ms.topic: reference
origin.date: 11/19/2019
ms.date: 07/31/2020
ms.openlocfilehash: 89d112b6804fc232b38332e15c2659eb88a8fecb
ms.sourcegitcommit: 4e1bc2e9b2a12dbcc05c52db5dbd1ae290aeb18d
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/01/2020
ms.locfileid: "87509235"
---
# <a name="entity-types"></a>实体类型

Kusto 查询在某个附加到 Kusto 群集的 Kusto 数据库的上下文中执行。 数据库中的数据按表排列，供查询引用。在表中，数据的组织形式是由列和行组成的矩形网格。 另外，查询可以引用数据库中的存储函数，这些函数是查询片段，可供重复使用。

* 群集是用于保存数据库的实体。
  群集没有名称，但可以引用，方法是：将 `cluster()` 特殊函数与群集的 URI 配合使用。
  例如，`cluster("https://help.kusto.chinacloudapi.cn")` 引用一个保存 `Samples` 数据库的群集。

* [数据库](./databases.md)是命名实体，用于保存表和存储函数。 所有 Kusto 查询都在某个数据库的上下文中运行，而该数据库的实体可以通过没有限定的查询引用。 另外，可以使用 [database() 特殊函数](../databasefunction.md)引用群集的其他数据库或者其他群集的数据库。 例如： `cluster("https://help.kusto.chinacloudapi.cn").database("Samples")`
  是一个通用引用，可以引用特定数据库。

* [表](./tables.md)是用于保存数据的命名实体。 表有一组排序的列，有零个或零个以上的数据行，每个行为表的每个列保存一个数据值。 表在引用时，可以只按名称引用（前提是它们位于数据库的查询上下文中），也可以使用数据库引用对其进行限定。 例如，`cluster("https://help.kusto.chinacloudapi.cn").database("Samples").StormEvents` 是一个通用引用，引用 `Samples` 数据库中的特定表。
  也可使用 [table() 特殊函数](../tablefunction.md)来引用表。

* [列](./columns.md)是命名实体，有一个[标量数据类型](../scalar-data-types/index.md)。
  在查询中引用列时，是相对于表格数据流引用的，该流位于引用列的特定运算符的上下文中。

* [存储函数](./stored-functions.md)是命名的实体，方便重用 Kusto 查询或查询部件。

* [外部表](./externaltables.md)是引用存储在 Kusto 数据库外部的数据的实体。
  外部表用于将数据从 Kusto 导出到外部存储，还可用于查询外部数据而无需将其引入到 Kusto 中。