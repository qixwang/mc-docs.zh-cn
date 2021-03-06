---
title: Azure 订阅限制和配额
description: 提供常见的 Azure 订阅和服务限制、配额和约束的列表。 本文包括有关如何增加限制以及最大值的信息。
ms.topic: conceptual
author: rockboyfor
origin.date: 06/04/2020
ms.date: 08/10/2020
ms.testscope: no
ms.testdate: 08/03/2020
ms.author: v-yeche
ms.openlocfilehash: 8af500192c7d1533c28d0fd3338ab5347aa2ade9
ms.sourcegitcommit: 3cf647177c22b24f76236c57cae19482ead6a283
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/10/2020
ms.locfileid: "88029678"
---
# <a name="azure-subscription-and-service-limits-quotas-and-constraints"></a>Azure 订阅和服务限制、配额和约束

本文列出了一些最常见的 Azure 限制，有时也称为配额。

若要详细了解 Azure 定价，请参阅 [Azure 定价概述](https://www.azure.cn/pricing/)。 在那里，可以使用[定价计算器](https://www.azure.cn/pricing/calculator/)来估算成本。 你还可以转到特定服务的定价详细信息页，例如 [Windows VM](https://www.azure.cn/pricing/details/virtual-machines/)。 有关帮助管理成本的提示，请参阅[通过 Azure 计费和成本管理来防止意外成本](https://docs.azure.cn/billing/billing-getting-started)。

<!--CORRECT ON https://docs.azure.cn/billing/billing-getting-started-->

## <a name="managing-limits"></a>管理限制

> [!NOTE]
> 某些服务的限制可调整。
>
> 当某个服务的限制不可调整时，下面的表会使用表头“限制”。 在这些情况下，默认值和最大限制相同。
>
> 如果可以调整限制，则表中包含“默认限制”和“最大限制”表头。 可以将限制提高到默认限制以上，但不能超过最大限制。
>
> 如果想要提高限制或配额，使其超出默认限制，可以[免费建立联机客户支持请求](../templates/error-resource-quota.md)。

[试用订阅](https://www.azure.cn/pricing/1rmb-trial-full)。

某些限制在区域级别进行管理。

让我们以 vCPU 配额为例。 若要请求提高 vCPU 支持数目的配额，必须确定要在哪些区域中使用多少 vCPU。 然后针对所需的 Azure 资源组 vCPU 配额的数量和区域发出特定请求。 如果需要在中国北部使用 30 个 vCPU 在那里运行应用程序，请专门在中国北部请求 30 个 vCPU。 这不会增加任何其他区域的 vCPU 配额 - 只有中国北部有 30 vCPU 配额。

因此，请考虑针对任意区域中的工作负荷来确定所需的 Azure 资源组配额， 然后在需将内容部署到其中的每个区域中请求相应的量。 有关如何确定特定区域的当前配额的帮助，请参阅[解决资源配额错误](../templates/error-resource-quota.md)。

## <a name="general-limits"></a>一般限制

有关资源名称的限制，请参阅 [Azure 资源的命名规则和限制](resource-name-rules.md)。

有关资源管理器 API 读写限制的信息，请参阅[限制资源管理器请求](request-limits-and-throttling.md)。

### <a name="management-group-limits"></a>管理组限制

以下限制适用于[管理组](../../governance/management-groups/overview.md)。

[!INCLUDE [management-group-limits](../../../includes/management-group-limits.md)]

### <a name="subscription-limits"></a>订阅限制

使用 Azure 资源管理器和 Azure 资源组时，以下限制适用。

[!INCLUDE [azure-subscription-limits-azure-resource-manager](../../../includes/azure-subscription-limits-azure-resource-manager.md)]

### <a name="resource-group-limits"></a>资源组限制

[!INCLUDE [azure-resource-groups-limits](../../../includes/azure-resource-groups-limits.md)]

## <a name="active-directory-limits"></a>Active Directory 限制

[!INCLUDE [AAD-service-limits](../../../includes/active-directory-service-limits-include.md)]

## <a name="api-management-limits"></a>API 管理限制

[!INCLUDE [api-management-service-limits](../../../includes/api-management-service-limits.md)]

## <a name="app-service-limits"></a>应用服务限制

以下应用服务限制包括 Web 应用、移动应用、API 应用的限制。

[!INCLUDE [azure-websites-limits](../../../includes/azure-websites-limits.md)]

## <a name="automation-limits"></a>自动化限制

[!INCLUDE [automation-limits](../../../includes/azure-automation-service-limits.md)]

## <a name="azure-cache-for-redis-limits"></a>Azure Redis 缓存限制

[!INCLUDE [redis-cache-service-limits](../../../includes/redis-cache-service-limits.md)]

## <a name="azure-cloud-services-limits"></a>Azure 云服务限制

[!INCLUDE [azure-cloud-services-limits](../../../includes/azure-cloud-services-limits.md)]

## <a name="azure-cognitive-search-limits"></a>Azure 认知搜索限制

定价层决定了搜索服务的容量和限制。 层包括：

* “免费”层为多租户服务，可与其他 Azure 订阅用户共享，旨在用于评估和小型开发项目。
* 基本层为规模较小的生产工作负荷提供专用计算资源，并为高可用查询工作负荷提供最多 3 个副本。
* “标准”层（包括 S1、S2、S3 和 S3 高密度）适用于较大型生产工作负荷。 “标准”层内有多个级别，以便可以选择与工作负荷配置文件最匹配的资源配置。

**基于订阅的限制**

[!INCLUDE [azure-search-limits-per-subscription](../../../includes/azure-search-limits-per-subscription.md)]

**基于搜索服务的限制**

[!INCLUDE [azure-search-limits-per-service](../../../includes/azure-search-limits-per-service.md)]

若要详细了解更细粒度级别的限制（例如文档大小、每秒查询数、密钥数、请求数和响应数），请参阅 [Azure 认知搜索中的服务限制](../../search/search-limits-quotas-capacity.md)。

## <a name="azure-cognitive-services-limits"></a>Azure 认知服务限制

[!INCLUDE [azure-cognitive-services-limits](../../../includes/azure-cognitive-services-limits.md)]

## <a name="azure-cosmos-db-limits"></a>Azure Cosmos DB 限制

有关 Azure Cosmos DB 的限制，请参阅 [Azure Cosmos DB 中的限制](../../cosmos-db/concepts-limits.md)。

## <a name="azure-data-explorer-limits"></a>Azure 数据资源管理器限制

[!INCLUDE [azure-data-explorer-limits](../../../includes/data-explorer-limits.md)]

## <a name="azure-database-for-mysql"></a>Azure Database for MySQL

有关 Azure Database for MySQL 限制，请参阅 [Azure Database for MySQL 中的限制](../../mysql/concepts-limits.md)。

## <a name="azure-database-for-postgresql"></a>Azure Database for PostgreSQL

有关 Azure Database for PostgreSQL 限制，请参阅 [Azure Database for PostgreSQL 中的限制](../../postgresql/concepts-limits.md)。

## <a name="azure-functions-limits"></a>Azure Functions 限制

[!INCLUDE [functions-limits](../../../includes/functions-limits.md)]

有关详细信息，请参阅 [Functions 托管计划比较](../../azure-functions/functions-scale.md)。

<!--Not Avaialble on #hosting-plans-comparison-->

## <a name="azure-kubernetes-service-limits"></a>Azure Kubernetes 服务限制

[!INCLUDE [container-service-limits](../../../includes/container-service-limits.md)]

## <a name="azure-machine-learning-limits"></a>Azure 机器学习限制

可在[“Azure 机器学习配额”页](../../machine-learning/how-to-manage-quotas.md)中找到 Azure 机器学习计算配额的最新值

<!--Not Available on ## Azure Maps limits-->

<a name="azure-monitor-limits"></a>
## <a name="azure-monitor-limits"></a>Azure Monitor 限制

### <a name="alerts"></a>警报

[!INCLUDE [monitoring-limits](../../../includes/azure-monitor-limits-alerts.md)]

### <a name="action-groups"></a>操作组

[!INCLUDE [monitoring-limits](../../../includes/azure-monitor-limits-action-groups.md)]

### <a name="log-queries-and-language"></a>日志查询和语言

[!INCLUDE [monitoring-limits](../../../includes/azure-monitor-limits-log-queries.md)]

### <a name="log-analytics-workspaces"></a>Log Analytics 工作区

[!INCLUDE [monitoring-limits](../../../includes/azure-monitor-limits-workspaces.md)]

### <a name="application-insights"></a>Application Insights

[!INCLUDE [monitoring-limits](../../../includes/azure-monitor-limits-app-insights.md)]

## <a name="azure-policy-limits"></a>Azure Policy 限制

[!INCLUDE [policy-limits](../../../includes/azure-policy-limits.md)]

## <a name="azure-signalr-service-limits"></a>Azure SignalR 服务限制

[!INCLUDE [signalr-service-limits](../../../includes/signalr-service-limits.md)]

## <a name="backup-limits"></a>备份限制

[!INCLUDE [azure-backup-limits](../../../includes/azure-backup-limits.md)]

## <a name="batch-limits"></a>批处理限制

[!INCLUDE [azure-batch-limits](../../../includes/azure-batch-limits.md)]

## <a name="classic-deployment-model-limits"></a>经典部署模型限制

如果使用经典部署模型而不是 Azure 资源管理器部署模型，则以下限制适用。

[!INCLUDE [azure-subscription-limits](../../../includes/azure-subscription-limits.md)]

## <a name="container-instances-limits"></a>容器实例限制

[!INCLUDE [container-instances-limits](../../../includes/container-instances-limits.md)]

## <a name="container-registry-limits"></a>容器注册表限制

下表详细介绍了“基本”、“标准”和“高级”[服务层级](../../container-registry/container-registry-skus.md)的功能和限制。

[!INCLUDE [container-registry-limits](../../../includes/container-registry-limits.md)]

## <a name="content-delivery-network-limits"></a>内容分发网络限制

[!INCLUDE [cdn-limits](../../../includes/cdn-limits.md)]

## <a name="data-factory-limits"></a>数据工厂限制

[!INCLUDE [azure-data-factory-limits](../../../includes/azure-data-factory-limits.md)]

<!--Not Available on ## Data Lake Analytics limits-->
<!--Not Available on ## Data Lake Store limits-->
<!--Not Available on ## Data Share limits-->

## <a name="database-migration-service-limits"></a>数据库迁移服务限制

[!INCLUDE [database-migration-service-limits](../../../includes/database-migration-service-limits.md)]

<!--Not Available on## Digital Twins limits-->

## <a name="event-grid-limits"></a>事件网格限制

[!INCLUDE [event-grid-limits](../../../includes/event-grid-limits.md)]

## <a name="event-hubs-limits"></a>事件中心限制

[!INCLUDE [azure-servicebus-limits](../../../includes/event-hubs-limits.md)]

## <a name="identity-manager-limits"></a>Identity Manager 限制

[!INCLUDE [automation-limits](~/includes/managed-identity-limits.md)]

## <a name="iot-central-limits"></a>IoT Central 限制
[!INCLUDE [iot-central-limits](../../../includes/iot-central-limits.md)]

## <a name="iot-hub-limits"></a>IoT 中心限制

[!INCLUDE [azure-iothub-limits](../../../includes/iot-hub-limits.md)]

## <a name="iot-hub-device-provisioning-service-limits"></a>IoT 中心设备预配服务限制

[!INCLUDE [azure-iotdps-limits](../../../includes/iot-dps-limits.md)]

## <a name="key-vault-limits"></a>密钥保管库限制

[!INCLUDE [key-vault-limits](../../../includes/key-vault-limits.md)]

## <a name="media-services-limits"></a>媒体服务限制

[!INCLUDE [azure-mediaservices-limits](../../../includes/media-servieces-limits-quotas-constraints.md)]

### <a name="media-services-v2-legacy"></a>媒体服务 v2（旧版）

若要了解特定于媒体服务 v2（旧版）的限制，请参阅[媒体服务 v2（旧版）](../../media-services/previous/media-services-quotas-and-limitations.md)

## <a name="mobile-services-limits"></a>移动服务限制

[!INCLUDE [mobile-services-limits](../../../includes/mobile-services-limits.md)]

## <a name="multi-factor-authentication-limits"></a>多重身份验证限制

[!INCLUDE [azure-mfa-service-limits](../../../includes/azure-mfa-service-limits.md)]

## <a name="networking-limits"></a>网络限制

[!INCLUDE [azure-virtual-network-limits](../../../includes/azure-virtual-network-limits.md)]

### <a name="expressroute-limits"></a>ExpressRoute 限制

[!INCLUDE [expressroute-limits](../../../includes/expressroute-limits.md)]

### <a name="virtual-wan-limits"></a>虚拟 WAN 限制

[!INCLUDE [virtual-wan-limits](../../../includes/virtual-wan-limits.md)]

### <a name="application-gateway-limits"></a>应用程序网关限制

除非另有说明，否则下表适用于 v1、v2、标准和 WAF SKU。
[!INCLUDE [application-gateway-limits](../../../includes/application-gateway-limits.md)]

### <a name="network-watcher-limits"></a>网络观察程序限制

[!INCLUDE [network-watcher-limits](../../../includes/network-watcher-limits.md)]

<a name="private-link-limits"></a>

<!--Not Available on Private Link

### <a name="private-link-limits"></a><span data-ttu-id="2f8e4-200">专用链接限制</span><span class="sxs-lookup"><span data-stu-id="2f8e4-200">Private Link limits</span></span>

[!INCLUDE [private-link-limits](../../../includes/private-link-limits.md)]

-->

### <a name="traffic-manager-limits"></a>流量管理器限制

[!INCLUDE [traffic-manager-limits](../../../includes/traffic-manager-limits.md)]

### <a name="azure-bastion-limits"></a>Azure Bastion 限制

[!INCLUDE [Azure Bastion limits](../../../includes/bastion-limits.md)]

### <a name="azure-dns-limits"></a>Azure DNS 限制

[!INCLUDE [dns-limits](../../../includes/dns-limits.md)]

### <a name="azure-firewall-limits"></a>Azure 防火墙限制

[!INCLUDE [azure-firewall-limits](../../../includes/firewall-limits.md)]

<!--Not Available on ### Azure Front Door Service limits-->

## <a name="notification-hubs-limits"></a>通知中心限制

[!INCLUDE [notification-hub-limits](../../../includes/notification-hub-limits.md)]

## <a name="role-based-access-control-limits"></a>基于角色的访问控制限制

[!INCLUDE [role-based-access-control-limits](../../../includes/role-based-access-control-limits.md)]

## <a name="service-bus-limits"></a>服务总线限制

[!INCLUDE [azure-servicebus-limits](../../../includes/service-bus-quotas-table.md)]

## <a name="site-recovery-limits"></a>站点恢复限制

[!INCLUDE [site-recovery-limits](../../../includes/site-recovery-limits.md)]

## <a name="sql-database-limits"></a>SQL 数据库限制

有关 SQL 数据库的限制，请参阅[单一数据库的 SQL 数据库资源限制](../../azure-sql/database/resource-limits-vcore-single-databases.md)、[弹性池和共用数据库的 SQL 数据库资源限制](../../azure-sql/database/resource-limits-vcore-elastic-pools.md)以及 [SQL 托管实例的 SQL 数据库资源限制](../../azure-sql/managed-instance/resource-limits.md)。

## <a name="azure-synapse-analytics-limits"></a>Azure Synapse Analytics 限制

有关 Azure Synapse Analytics 限制，请参阅 [Azure Synapse 资源限制](../../synapse-analytics/sql-data-warehouse/sql-data-warehouse-service-capacity-limits.md)。

## <a name="storage-limits"></a>存储限制

<!--like # storage accts -->

[!INCLUDE [azure-storage-account-limits-standard](../../../includes/azure-storage-account-limits-standard.md)]

有关标准存储帐户限制的详细信息，请参阅[标准存储帐户的可伸缩性目标](../../storage/common/scalability-targets-standard-account.md)。

### <a name="storage-resource-provider-limits"></a>存储资源提供程序限制

[!INCLUDE [azure-storage-limits-azure-resource-manager](../../../includes/azure-storage-limits-azure-resource-manager.md)]

### <a name="azure-blob-storage-limits"></a>Azure Blob 存储限制

[!INCLUDE [storage-blob-scale-targets](../../../includes/storage-blob-scale-targets.md)]

### <a name="azure-files-limits"></a>Azure 文件限制

有关 Azure 文件限制的详细信息，请参阅 [Azure 文件可伸缩性和性能目标](../../storage/files/storage-files-scale-targets.md)。

[!INCLUDE [storage-files-scale-targets](../../../includes/storage-files-scale-targets.md)]

<!--Not Available on ### Azure File Sync limits-->

### <a name="azure-queue-storage-limits"></a>Azure 队列存储限制

[!INCLUDE [storage-queues-scale-targets](../../../includes/storage-queues-scale-targets.md)]

### <a name="azure-table-storage-limits"></a>Azure 表存储限制

[!INCLUDE [storage-tables-scale-targets](../../../includes/storage-tables-scale-targets.md)]

<!-- conceptual info about disk limits -- applies to unmanaged and managed -->

### <a name="virtual-machine-disk-limits"></a>虚拟机磁盘限制

[!INCLUDE [azure-storage-limits-vm-disks](../../../includes/azure-storage-limits-vm-disks.md)]

有关详细信息，请参阅[虚拟机大小](../../virtual-machines/linux/sizes.md?toc=%2fvirtual-machines%2flinux%2ftoc.json)。

#### <a name="disk-encryption-sets"></a>磁盘加密集

每个区域、每个订阅有 50 个磁盘加密集的限制。 有关更多信息，请参阅 [Linux](/virtual-machines/linux/disk-encryption#restrictions) 或 [Windows](/virtual-machines/windows/disk-encryption#restrictions) 虚拟机的加密文档。 如果需要增加配额，请与 Azure 支持部门联系。

### <a name="managed-virtual-machine-disks"></a>托管虚拟机磁盘

[!INCLUDE [azure-storage-limits-vm-disks-managed](../../../includes/azure-storage-limits-vm-disks-managed.md)]

### <a name="unmanaged-virtual-machine-disks"></a>非托管虚拟机磁盘

[!INCLUDE [azure-storage-limits-vm-disks-standard](../../../includes/azure-storage-limits-vm-disks-standard.md)]

[!INCLUDE [azure-storage-limits-vm-disks-premium](../../../includes/azure-storage-limits-vm-disks-premium.md)]

<!--Not Available on ## StorSimple System limits-->

## <a name="stream-analytics-limits"></a>流分析限制

[!INCLUDE [stream-analytics-limits-table](../../../includes/stream-analytics-limits-table.md)]

## <a name="virtual-machines-limits"></a>虚拟机限制

### <a name="virtual-machines-limits"></a>虚拟机限制

[!INCLUDE [azure-virtual-machines-limits](../../../includes/azure-virtual-machines-limits.md)]

<a name="limits-and-the-azure-resource-manager"></a>
### <a name="virtual-machines-limits---azure-resource-manager"></a>虚拟机限制 - Azure Resource Manager

使用 Azure 资源管理器和 Azure 资源组时，以下限制适用。

[!INCLUDE [azure-virtual-machines-limits-azure-resource-manager](../../../includes/azure-virtual-machines-limits-azure-resource-manager.md)]

### <a name="shared-image-gallery-limits"></a>共享映像库限制

使用共享映像库部署资源时，每个订阅存在限制：

- 每个区域的每个订阅限制为 100 个共享映像库
- 每个区域的每个订阅限制为 1,000 个映像定义
- 每个区域的每个订阅限制为 10,000 个映像版本

## <a name="virtual-machine-scale-sets-limits"></a>虚拟机规模集限制

[!INCLUDE [virtual-machine-scale-sets-limits](../../../includes/azure-virtual-machine-scale-sets-limits.md)]

## <a name="see-also"></a>另请参阅

* [了解 Azure 限制及如何提高限制](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/)
* [Azure 的虚拟机和云服务大小](../../virtual-machines/linux/sizes.md?toc=%2fvirtual-machines%2flinux%2ftoc.json)
* [Azure 云服务的大小](../../cloud-services/cloud-services-sizes-specs.md)
* [Azure 资源的命名规则和限制](resource-name-rules.md)

<!-- Update_Description: update meta properties, wording update, update link -->