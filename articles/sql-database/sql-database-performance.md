---
title: 监视和提高性能
description: Azure SQL 数据库提供的性能工具有助于发现可以提高当前查询性能的方面。
ms.service: sql-database
ms.subservice: performance
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: WenJason
ms.author: v-jay
ms.reviewer: jrasnik, carlrab
origin.date: 11/14/2019
ms.date: 12/16/2019
ms.openlocfilehash: 823fb7e0579fe0d13422a23f0d7f1b666c6cfd75
ms.sourcegitcommit: 4a09701b1cbc1d9ccee46d282e592aec26998bff
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/25/2019
ms.locfileid: "75336131"
---
# <a name="monitor-and-improve-performance"></a>监视和提高性能

Azure SQL 数据库可识别数据库中的潜在问题，并通过提供智能优化操作和建议来推荐可提高工作负荷性能的操作。

## <a name="performance-tuning-options"></a>性能优化选项

可供 Azure SQL 数据库使用的性能优化选项在“智能性能”下的数据库导航菜单上提供：

| 性能优化选项 | 单一数据库和共用数据库支持 | 实例数据库支持 |
| :----------------------------- | ----- | ----- |
| **性能概述** - 监视数据库的所有性能活动。 | 是 | 否 | 
| **性能建议** - 显示可提高工作负荷性能的性能建议。 | 是 | 否 | 
| **查询性能见解** - 显示数据库中资源消耗量最大的查询的性能。 | 是 | 否 | 
| **自动优化** - 使用 Azure SQL 数据库自动优化数据库性能。 | 是 | 否 | 

## <a name="performance-overview"></a>性能概述

此视图提供了数据库性能摘要，有助于进行性能优化和故障排除。 

![Azure SQL 数据库性能概述](./media/sql-database-performance/performance-overview-annotated.png)

* “建议”磁贴提供数据库的优化建议明细（如果建议超出 3 条，则将显示前 3 条）。  单击此磁贴可转到 **[性能建议](#performance-recommendations)** 。 
* “优化活动”  磁贴提供正在进行的和已完成的数据库优化操作摘要，可快速查看优化活动的历史记录。 单击此磁贴可转到数据库的完整优化历史记录视图。
* “自动优化”磁贴显示数据库的[自动优化配置](sql-database-automatic-tuning-enable.md)（已自动应用到数据库的优化选项）。  单击此磁贴可打开自动化配置对话框。
* “数据库查询”磁贴显示数据库的查询性能摘要（DTU 整体使用情况和排名靠前的资源消耗查询）。  单击此磁贴可转到 **[查询性能见解](#query-performance-insight)** 。

## <a name="performance-recommendations"></a>性能建议

此页提供可提高数据库性能的智能[优化建议](sql-database-advisor.md)。 此页显示以下类型的建议：

* 有关要创建或删除哪些索引的建议。
* 数据库中发现架构问题时提供的建议。
* 查询可从参数化查询中获益时提供的建议。

![Azure SQL 数据库性能建议](./media/sql-database-performance/performance-recommendations-annotated.png)

还可以查找有关以前应用的优化操作的完整历史记录。

在[查找和应用性能建议](sql-database-advisor-portal.md)一文中了解如何查找和应用性能建议。

## <a name="query-performance-insight"></a>Query Performance Insight

[查询性能见解](sql-database-query-performance.md)通过提供以下功能使排查数据库性能问题花费的时间更少：

* 深入洞察数据库资源 (DTU) 的消耗。 
* 排名靠前的 CPU 消耗查询，经过优化后可能会改善性能。 
* 可以向下钻取查询的详细信息。 

  ![Azure SQL 数据库的查询性能见解](./media/sql-database-performance/query-performance-insights-annotated.png)

在 **[如何使用查询性能见解](sql-database-query-performance.md)** 一文中了解有关此页面的详细信息。

## <a name="automatic-tuning"></a>自动优化

Azure SQL 数据库可以通过应用[性能建议](sql-database-advisor.md)自动优化数据库性能。 若要启用此功能，请阅读[如何启用自动优化](sql-database-automatic-tuning-enable.md)。

  ![Azure SQL 数据库的自动优化](./media/sql-database-performance/automatic-tuning-annotated.png)

若要了解详细信息，请阅读[自动优化文章](sql-database-automatic-tuning.md)。

## <a name="additional-resources"></a>其他资源

* [Azure SQL 数据库的单一数据库性能指导](sql-database-performance-guidance.md)
* [何时应使用弹性池？](sql-database-elastic-pool-guidance.md)
