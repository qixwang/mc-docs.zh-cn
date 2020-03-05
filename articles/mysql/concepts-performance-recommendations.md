---
title: 性能建议 - Azure Database for MySQL
description: 本文介绍 Azure Database for MySQL 中的“性能建议”功能
author: WenJason
ms.author: v-jay
ms.service: mysql
ms.topic: conceptual
origin.date: 12/02/2019
ms.date: 03/02/2020
ms.openlocfilehash: bac6ef940d2a60d8c8c56a91bb910ea97a8028f1
ms.sourcegitcommit: d202f6fe068455461c8756b50e52acd4caf2d095
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/28/2020
ms.locfileid: "78155374"
---
# <a name="performance-recommendations-in-azure-database-for-mysql"></a>Azure Database for MySQL 中的性能建议

> [!NOTE] 
> 将要查看的是 Azure Database for MySQL 的新服务。 若要查看经典 MySQL Database for Azure 的文档，请访问[此页](https://docs.azure.cn/zh-cn/mysql-database-on-azure/)。

**适用于：** Azure Database for MySQL 5.7

性能建议功能会通过分析数据库来创建自定义的建议，以提高性能。 为了生成建议，分析将查看各种数据库特征（包括架构）。 启用服务器上的[查询存储](concepts-query-store.md)即可充分利用性能建议功能。 如果性能架构为 OFF，则打开查询存储会启用 performance_schema 以及此功能所需的一部分性能架构检测。 实施任何性能建议后，应测试性能以评估这些更改的影响。

## <a name="permissions"></a>权限

使用性能建议功能运行分析所需的  “所有者”或“参与者”  权限。

## <a name="performance-recommendations"></a>性能建议

[性能建议](concepts-performance-recommendations.md)功能跨服务器分析工作负载以标识可能会提高性能的索引。

从 MySQL 服务器的 Azure 门户页菜单栏的“智能性能”  部分打开**性能建议**。

![性能建议登陆页面](./media/concepts-performance-recommendations/performance-recommendations-page.png)

选择“分析”  并选择数据库就会开始分析。 分析可能需要几分钟的时间才能完成，具体取决于工作负载。 分析完成后，门户中将出现通知。 分析会执行数据库的深层检查。 建议在非高峰期执行分析。

 “建议”窗口会显示找到的建议的列表，以及生成该建议的相关查询 ID。 有了查询 ID，就可以使用 [mysql.query_store](concepts-query-store.md#mysqlquery_store) 视图来详细了解查询。

![性能建议新页](./media/concepts-performance-recommendations/performance-recommendations-result.png)

不会自动应用建议。 若要应用建议，请复制查询文本并从所选的客户端中运行。 记住通过测试和监视来评估建议。

## <a name="recommendation-types"></a>建议类型

目前仅支持“创建索引”  建议。

### <a name="create-index-recommendations"></a>创建索引建议

 “创建索引”建议建议使用新索引来加快工作负载中最常运行或最耗时的查询。 此建议类型需要启用[查询存储](concepts-query-store.md)。 查询存储收集查询信息并提供详细的查询运行时和频率统计信息，供分析用来提出建议。

## <a name="next-steps"></a>后续步骤
- 详细了解如何在 Azure Database for MySQL 中进行[监视和优化](concepts-monitoring.md)。