---
title: Azure SQL 数据库资源限制 | Microsoft Docs
description: 本文概述了使用弹性池的单一数据库和弹性池的 Azure SQL 数据库资源限制。 它还提供了有关在达到或超过这些资源限制时会发生什么情况的信息。
services: sql-database
ms.service: sql-database
ms.subservice: single-database
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: WenJason
ms.author: v-jay
ms.reviewer: sashan,moslake,josack
origin.date: 11/19/2019
ms.date: 04/27/2020
ms.openlocfilehash: 87de5a828e329b26f65829d136df43af563720f8
ms.sourcegitcommit: a4a2521da9b29714aa6b511fc6ba48279b5777c8
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/24/2020
ms.locfileid: "82126943"
---
# <a name="sql-database-resource-limits"></a>SQL 数据库资源限制

本文概述了管理单一数据库和弹性池的 SQL 数据库服务器的 SQL 数据库资源限制。 其中提供了有关在达到或超出这些资源限制时会发生什么情况的信息。

> [!NOTE]
> 有关托管实例限制，请参阅[托管实例的 SQL 数据库资源限制](sql-database-managed-instance-resource-limits.md)。

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
> 随着数据库的数量接近每个 SQL 数据库服务器的限制，可能出现以下情况：
>
> - 对主数据库运行查询的延迟增加。  这包括资源利用率统计信息的视图，如 sys.resource_stats。
> - 管理操作和呈现门户视点（涉及枚举服务器中的数据库）的延迟增加。

> [!NOTE]
> 若要获取更高的 DTU/eDTU 配额、vCore 配额或超过默认数量的服务器，请在 Azure 门户中提交新的支持请求。

### <a name="storage-size"></a>存储大小

对于单一数据库资源存储大小，请参阅[基于 DTU 的资源限制](sql-database-dtu-resource-limits-single-databases.md)或[基于 vCore 的资源限制](sql-database-vcore-resource-limits-single-databases.md)，了解每个定价层的存储大小限制。

## <a name="what-happens-when-database-resource-limits-are-reached"></a>如果达到数据库资源限制，会发生什么？

### <a name="compute-dtus-and-edtus--vcores"></a>计算（DTU 和 eDTU/vCore）

如果数据库计算资源利用率（衡量依据为 DTU 和 eDTU 或 vCore）变高，查询延迟会增加，甚至可能会出现查询超时的现象。在上述情况下，服务可能对查询排队，并在资源可用时向查询提供资源以用于执行。
计算使用率变高时，风险缓解选项包括：

- 提高数据库或弹性池的计算大小，为数据库提供更多计算资源。 请参阅[缩放单一数据库资源](sql-database-single-database-scale.md)和[缩放弹性池资源](sql-database-elastic-pool-scale.md)。
- 优化查询，以降低每个查询的资源占用。 有关详细信息，请参阅[查询优化/提示](sql-database-performance-guidance.md#query-tuning-and-hinting)。

### <a name="storage"></a>存储

当使用的数据库空间到达上限时，将无法进行增加数据大小的数据库插入和更新操作，客户端会收到[错误消息](troubleshoot-connectivity-issues-microsoft-azure-sql-database.md)。 SELECT 和 DELETE 语句不受影响。

空间使用率变高时，风险缓解选项包括：

- 提高数据库或弹性池的大小上限，或添加更多存储资源。 请参阅[缩放单一数据库资源](sql-database-single-database-scale.md)和[缩放弹性池资源](sql-database-elastic-pool-scale.md)。
- 如果数据库在弹性池内，那么可选择将数据库移出弹性池，从而避免与其他数据库共享存储空间。
- 收缩数据库来回收未使用的空间。 有关详细信息，请参阅[管理 Azure SQL 数据库中的文件空间](sql-database-file-space-management.md)

### <a name="sessions-and-workers-requests"></a>会话和辅助角色（请求）

会话和辅助角色的数目上限由服务层级和计算大小（DTU/eDTU 或 vCore）决定。 当到达会话或辅助角色上限时，新的请求将被拒绝，客户端将收到错误消息。 虽然应用程序可以轻松地控制可用的连接数，但并行辅助角色数通常更难以估计和控制。 在负荷高峰期，当数据库资源达到上限，辅助角色由于查询运行时间较长、阻塞链大或查询并行度过高而堆积时，这种情况尤其突出。

会话或辅助角色使用率变高时，风险缓解选项包括：

- 提高数据库或弹性池的服务层级或计算大小。 请参阅[缩放单一数据库资源](sql-database-single-database-scale.md)和[缩放弹性池资源](sql-database-elastic-pool-scale.md)。
- 如果争用计算资源造成了辅助角色使用率上升，请优化查询，以降低每项查询的资源使用率。 有关详细信息，请参阅[查询优化/提示](sql-database-performance-guidance.md#query-tuning-and-hinting)。
- 减小 [MAXDOP](https://docs.microsoft.com/sql/database-engine/configure-windows/configure-the-max-degree-of-parallelism-server-configuration-option#Guidelines)（最大并行度）设置。
- 优化查询工作负荷，以减少查询受阻的发生次数和持续时间。

### <a name="resource-consumption-by-user-workloads-and-internal-processes"></a>用户工作负荷和内部进程的资源消耗量

将在 [sys.dm_db_resource_stats](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/sys-dm-db-resource-stats-azure-sql-database?view=azuresqldb-current) 和 [sys.resource_stats](https://docs.microsoft.com/sql/relational-databases/system-catalog-views/sys-resource-stats-azure-sql-database?view=azuresqldb-current) 视图的 `avg_cpu_percent` 和 `avg_memory_usage_percent` 列中报告每个数据库中的用户工作负荷的 CPU 和内存消耗量。 对于弹性池，将在 [sys.elastic_pool_resource_stats](https://docs.microsoft.com/sql/relational-databases/system-catalog-views/sys-elastic-pool-resource-stats-azure-sql-database) 视图中报告池级别的资源消耗量。 对于池级别的[单一数据库](/azure-monitor/platform/metrics-supported#microsoftsqlserversdatabases)和[弹性池](/azure-monitor/platform/metrics-supported#microsoftsqlserverselasticpools)，还会通过 Azure Monitor 指标 `cpu_percent` 报告用户工作负荷的 CPU 消耗量。

Azure SQL 数据库需要使用计算资源来实现核心服务功能，例如高可用性和灾难恢复、数据库备份和还原、监视、查询存储、自动优化，等等。对于这些内部进程，系统会从总体资源中为其留出有限的一部分特定资源，使剩余的资源可供用户工作负荷使用。 当内部进程不使用计算资源时，系统会将其提供给用户工作负荷使用。

将在 [sys.dm_db_resource_stats](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/sys-dm-db-resource-stats-azure-sql-database?view=azuresqldb-current) 和 [sys.resource_stats](https://docs.microsoft.com/sql/relational-databases/system-catalog-views/sys-resource-stats-azure-sql-database?view=azuresqldb-current) 视图的 `avg_instance_cpu_percent` 和 `avg_instance_memory_percent` 列内，报告托管单一数据库或弹性池的 SQL Server 实例上用户工作负荷与内部进程的 CPU 和内存总消耗量。 对于池级别的[单一数据库](/azure-monitor/platform/metrics-supported#microsoftsqlserversdatabases)和[弹性池](/azure-monitor/platform/metrics-supported#microsoftsqlserverselasticpools)，还会通过 Azure Monitor 指标 `sqlserver_process_core_percent` 和 `sqlserver_process_memory_percent` 报告此数据。

在性能监视和故障排除上下文中，必须考虑用户 CPU 消耗量（`avg_cpu_percent`、`cpu_percent`），以及用户工作负荷和内部进程的 CPU 总消耗量（`avg_instance_cpu_percent`、`sqlserver_process_core_percent`）   。

用户 CPU 消耗量的计算值为每个服务目标中用户工作负荷限制的一个百分比  。 用户 CPU 利用率为 100% 表示用户工作负荷达到了服务目标的限制  。 但是，当 CPU 总消耗量达到 70-100% 范围时，即使报告的用户 CPU 消耗量明显低于 100%，也可能会看到用户工作负荷吞吐量保持平稳，但查询延迟增大   。 对适度分配的计算资源但相对密集的用户工作负荷使用较小的服务目标时，更有可能会发生这种情况。 当内部进程临时需要更多的资源时（例如，在创建数据库的新副本时），使用较小的服务目标也可能发生这种情况。

当 CPU 总消耗量较高时，缓解措施与前面所述相同，也包括增大服务目标和/或优化用户工作负荷  。

## <a name="next-steps"></a>后续步骤

- 有关常规 Azure 限制的相关信息，请参阅 [Azure 订阅和服务限制、配额和约束](../azure-resource-manager/management/azure-subscription-service-limits.md)。
- 有关 DTU 和 eDTU 的信息，请参阅 [DTU 和 eDTU](sql-database-purchase-models.md#dtu-based-purchasing-model)。
- 有关 tempdb 大小限制的信息，请参阅 [Azure SQL 数据库中的 TempDB](https://docs.microsoft.com/sql/relational-databases/databases/tempdb-database#tempdb-database-in-sql-database)。
