---
title: Azure 中逻辑服务器的资源限制
description: 本文概述了 Azure SQL 数据库和 Azure Synapse Analytics 所使用的 Azure 中逻辑服务器的资源限制。 它还提供了有关在达到或超过这些资源限制时会发生什么情况的信息。
services: sql-database
ms.service: sql-database
ms.subservice: single-database
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: WenJason
ms.author: v-jay
ms.reviewer: sashan,moslake,josack
origin.date: 06/10/2020
ms.date: 07/13/2020
ms.openlocfilehash: 047b79d969c6c8c68b44297c3233c6db96fb7d13
ms.sourcegitcommit: fa26665aab1899e35ef7b93ddc3e1631c009dd04
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/10/2020
ms.locfileid: "86227920"
---
# <a name="resource-limits-for-azure-sql-database-and-azure-synapse-analytics-servers"></a>Azure SQL 数据库和 Azure Synapse Analytics 服务器的资源限制
[!INCLUDE[appliesto-sqldb-asa](../includes/appliesto-sqldb-asa.md)]

本文概述了 Azure SQL 数据库和 Azure Synapse Analytics 所使用的逻辑服务器的资源限制。 其中提供了有关在达到或超出这些资源限制时会发生什么情况的信息。

> [!NOTE]
> 有关 Azure SQL 托管实例限制，请参阅[托管实例的 SQL 数据库资源限制](../managed-instance/resource-limits.md)。

## <a name="maximum-resource-limits"></a>最大资源限制

| 资源 | 限制 |
| :--- | :--- |
| 每个服务器的数据库数 | 5000 |
| 任意区域中每个订阅的服务器默认数量 | 20 个 |
| 任意区域中每个订阅的服务器数上限 | 200 |  
| 每个服务器的 DTU/eDTU 配额 | 54,000 |  
| 每个服务器/实例的 vCore 配额 | 540 |
| 每个服务器的最大池数 | 受限于 DTU 或 vCore 数。 例如，如果每个池是 1000 个 DTU，则一个服务器可以支持 54 个池。|
|||

> [!IMPORTANT]
> 随着数据库的数量接近每个服务器的限制，可能出现以下情况：
>
> - 对主数据库运行查询的延迟增加。  这包括资源利用率统计信息的视图，如 sys.resource_stats。
> - 管理操作和呈现门户视点（涉及枚举服务器中的数据库）的延迟增加。

> [!NOTE]
> 若要获取更高的 DTU/eDTU 配额、vCore 配额或超过默认数量的服务器，请在 Azure 门户中提交新的支持请求。

### <a name="storage-size"></a>存储大小

对于单一数据库资源存储大小，请参阅[基于 DTU 的资源限制](resource-limits-dtu-single-databases.md)或[基于 vCore 的资源限制](resource-limits-vcore-single-databases.md)，了解每个定价层的存储大小限制。

## <a name="what-happens-when-database-resource-limits-are-reached"></a>如果达到数据库资源限制，会发生什么？

### <a name="compute-cpu"></a>计算 CPU

当数据库计算 CPU 使用率变高时，查询延迟也会增加，查询甚至可能超时。在上述情况下，服务可能对查询排队，并在资源可用时向查询提供资源以用于执行。
计算使用率变高时，风险缓解选项包括：

- 提高数据库或弹性池的计算大小，为数据库提供更多计算资源。 请参阅[缩放单一数据库资源](single-database-scale.md)和[缩放弹性池资源](elastic-pool-scale.md)。
- 优化查询，以降低每个查询的 CPU 资源利用率。 有关详细信息，请参阅[查询优化/提示](performance-guidance.md#query-tuning-and-hinting)。

### <a name="storage"></a>存储

当使用的数据库空间到达上限时，将无法进行增加数据大小的数据库插入和更新操作，客户端会收到[错误消息](troubleshoot-common-errors-issues.md)。 SELECT 和 DELETE 语句不受影响。

空间使用率变高时，风险缓解选项包括：

- 提高数据库或弹性池的大小上限，或添加更多存储资源。 请参阅[缩放单一数据库资源](single-database-scale.md)和[缩放弹性池资源](elastic-pool-scale.md)。
- 如果数据库在弹性池内，可选择将数据库移出弹性池，从而避免与其他数据库共享存储空间。
- 收缩数据库来回收未使用的空间。 有关详细信息，请参阅[管理 Azure SQL 数据库中的文件空间](file-space-manage.md)

### <a name="sessions-and-workers-requests"></a>会话和辅助角色（请求）

会话和辅助角色的数目上限由服务层级和计算大小（DTU/eDTU 或 vCore）决定。 当到达会话或辅助角色上限时，新的请求将被拒绝，客户端将收到错误消息。 虽然应用程序可以轻松地控制可用的连接数，但并行辅助角色数通常更难以估计和控制。 在负荷高峰期，当数据库资源达到上限，辅助角色由于查询运行时间较长、阻塞链大或查询并行度过高而堆积时，这种情况尤其突出。

会话或辅助角色使用率变高时，风险缓解选项包括：

- 提高数据库或弹性池的服务层级或计算大小。 请参阅[缩放单一数据库资源](single-database-scale.md)和[缩放弹性池资源](elastic-pool-scale.md)。
- 如果争用计算资源造成了辅助角色使用率上升，请优化查询，以降低每项查询的资源使用率。 有关详细信息，请参阅[查询优化/提示](performance-guidance.md#query-tuning-and-hinting)。
- 减小 [MAXDOP](https://docs.microsoft.com/sql/database-engine/configure-windows/configure-the-max-degree-of-parallelism-server-configuration-option#Guidelines)（最大并行度）设置。
- 优化查询工作负荷，以减少查询受阻的发生次数和持续时间。

### <a name="memory"></a>内存

与其他资源（CPU、辅助角色、存储）不同，达到内存限制不会对查询性能产生负面影响，也不会导致错误和失败。 正如[内存管理体系结构指南](https://docs.microsoft.com/sql/relational-databases/memory-management-architecture-guide)中详细描述的那样，SQL Server 数据库引擎通常使用所有可用内存，这是设计使然。 内存主要用于缓存数据，以避免更昂贵的存储访问。 因此，较高的内存利用率通常会提高查询性能，因为从内存中读取的速度更快，而从存储中读取的速度更慢。

在数据库引擎启动之后，当工作负载开始从存储中读取数据时，数据库引擎会积极地在内存中缓存数据。 经过这一初始增长期之后，通常可看到 [sys.dm_db_resource_stats](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/sys-dm-db-resource-stats-azure-sql-database) 中的 `avg_memory_usage_percent` 和 `avg_instance_memory_percent` 列接近或等于 100%，尤其是对于非空闲且不能完全装入内存的数据库。

除了数据缓存之外，内存还用于数据库引擎的其他组件。 当有内存需求且所有可用内存已被数据缓存占用时，数据库引擎将动态收缩数据缓存大小以使内存可供其他组件使用，并在其他组件释放内存时动态增加数据缓存。

在极少数情况下，要求十分高的工作负载可能会导致内存不足，从而导致内存不足错误。 这可能发生在内存使用率介于 0% 和 100% 之间的任何级别。 

遇到内存不足错误时，缓解选项包括：
- 提高数据库或弹性池的服务层级或计算大小。 请参阅[缩放单一数据库资源](single-database-scale.md)和[缩放弹性池资源](elastic-pool-scale.md)。
- 优化查询和配置以降低内存使用率。 下表中介绍了常见的解决方案。

|解决方案|说明|
| :----- | :----- |
|减少内存授予的大小|有关内存授予的详细信息，请参阅[了解 SQL Server 内存授予](https://techcommunity.microsoft.com/t5/sql-server/understanding-sql-server-memory-grant/ba-p/383595)博客文章。 避免过大内存授予的一个常见解决方案是使[统计信息](https://docs.microsoft.com/sql/relational-databases/statistics/statistics)保持最新状态。 这可使查询引擎更准确地估计内存消耗，从而避免内存授予过大。</br></br>在使用兼容级别 140 及更高级别的数据库中，数据库引擎可使用[批处理模式内存授予反馈](https://docs.microsoft.com/sql/relational-databases/performance/intelligent-query-processing?view=sql-server-ver15#batch-mode-memory-grant-feedback)自动调整内存授予大小。 在使用兼容级别 150 及更高级别的数据库中，数据库引擎类似地使用[行模式内存授予反馈](https://docs.microsoft.com/sql/relational-databases/performance/intelligent-query-processing?view=sql-server-ver15#row-mode-memory-grant-feedback)，用于更常见的行模式查询。 此内置功能有助于避免由于内存授予过大而导致的内存不足错误。|
|减小查询计划缓存的大小|数据库引擎在内存中缓存查询计划，以避免为每次查询执行编译查询计划。 若要避免由于仅使用一次的缓存计划而导致的查询计划缓存膨胀，请启用 OPTIMIZE_FOR_AD_HOC_WORKLOADS [数据库范围内的配置](https://docs.microsoft.com/sql/t-sql/statements/alter-database-scoped-configuration-transact-sql)。|
|减小锁定内存的大小|数据库引擎将内存用于[锁定](https://docs.microsoft.com/sql/relational-databases/sql-server-transaction-locking-and-row-versioning-guide#Lock_Engine)。 如果可能，请避免可能获取大量锁定并导致高锁定内存消耗的大型事务。|


## <a name="resource-consumption-by-user-workloads-and-internal-processes"></a>用户工作负荷和内部进程的资源消耗量

将在 [sys.dm_db_resource_stats](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/sys-dm-db-resource-stats-azure-sql-database?view=azuresqldb-current) 和 [sys.resource_stats](https://docs.microsoft.com/sql/relational-databases/system-catalog-views/sys-resource-stats-azure-sql-database?view=azuresqldb-current) 视图的 `avg_cpu_percent` 和 `avg_memory_usage_percent` 列中报告每个数据库中的用户工作负荷的 CPU 和内存消耗量。 对于弹性池，将在 [sys.elastic_pool_resource_stats](https://docs.microsoft.com/sql/relational-databases/system-catalog-views/sys-elastic-pool-resource-stats-azure-sql-database) 视图中报告池级别的资源消耗量。 对于池级别的[单一数据库](/azure-monitor/platform/metrics-supported#microsoftsqlserversdatabases)和[弹性池](/azure-monitor/platform/metrics-supported#microsoftsqlserverselasticpools)，还会通过 Azure Monitor 指标 `cpu_percent` 报告用户工作负荷的 CPU 消耗量。

Azure SQL 数据库需要使用计算资源来实现核心服务功能，例如高可用性和灾难恢复、数据库备份和还原、监视、查询存储、自动优化，等等。对于这些内部进程，系统会从总体资源中为其留出有限的一部分特定资源，使剩余的资源可供用户工作负荷使用。 当内部进程不使用计算资源时，系统会将其提供给用户工作负载使用。

将在 [sys.dm_db_resource_stats](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/sys-dm-db-resource-stats-azure-sql-database?view=azuresqldb-current) 和 [sys.resource_stats](https://docs.microsoft.com/sql/relational-databases/system-catalog-views/sys-resource-stats-azure-sql-database?view=azuresqldb-current) 视图的 `avg_instance_cpu_percent` 和 `avg_instance_memory_percent` 列中报告用户工作负载和内部进程的总 CPU 和内存消耗量。 对于池级别的[单一数据库](/azure-monitor/platform/metrics-supported#microsoftsqlserversdatabases)和[弹性池](/azure-monitor/platform/metrics-supported#microsoftsqlserverselasticpools)，还会通过 Azure Monitor 指标 `sqlserver_process_core_percent` 和 `sqlserver_process_memory_percent` 报告此数据。

在性能监视和故障排除上下文中，必须考虑用户 CPU 消耗量（`avg_cpu_percent`、`cpu_percent`），以及用户工作负载和内部进程的 CPU 总消耗量（`avg_instance_cpu_percent`、`sqlserver_process_core_percent`） 。

用户 CPU 消耗量的计算值为每个服务目标中用户工作负荷限制的一个百分比。 用户 CPU 利用率为 100% 表示用户工作负荷达到了服务目标的限制。 但是，当 CPU 总消耗量达到 70-100% 范围时，即使报告的用户 CPU 消耗量明显低于 100%，也可能会看到用户工作负载吞吐量保持平稳，但查询延迟增大 。 对适度分配的计算资源但相对密集的用户工作负荷使用较小的服务目标时，更有可能会发生这种情况。 当内部进程临时需要更多的资源时（例如，在创建数据库的新副本时），使用较小的服务目标也可能发生这种情况。

当 CPU 总消耗量较高时，缓解措施与前面所述相同，也包括增大服务目标和/或优化用户工作负载。

## <a name="next-steps"></a>后续步骤

- 有关常规 Azure 限制的相关信息，请参阅 [Azure 订阅和服务限制、配额和约束](../../azure-resource-manager/management/azure-subscription-service-limits.md)。
- 有关 DTU 和 eDTU 的信息，请参阅 [DTU 和 eDTU](purchasing-models.md#dtu-based-purchasing-model)。
- 有关 tempdb 大小限制的信息，请参阅 [Azure SQL 数据库中的 TempDB](https://docs.microsoft.com/sql/relational-databases/databases/tempdb-database#tempdb-database-in-sql-database)。
 