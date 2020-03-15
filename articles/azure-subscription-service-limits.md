---
title: Azure 订阅限制和配额
description: 提供常见的 Azure 订阅和服务限制、配额和约束的列表。 本文包括有关如何增加限制以及最大值的信息。
services: multiple
author: lingliw
manager: digimobile
tags: billing
ms.assetid: 60d848f9-ff26-496e-a5ec-ccf92ad7d125
ms.service: billing
ms.topic: article
origin.date: 05/30/2019
ms.date: 09/18/2019
ms.author: v-lingwu
ms.openlocfilehash: ebc049202e420d80e4b42db3ec1b993b6d6dfa27
ms.sourcegitcommit: 3c98f52b6ccca469e598d327cd537caab2fde83f
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/13/2020
ms.locfileid: "79291542"
---
# <a name="azure-subscription-and-service-limits-quotas-and-constraints"></a>Azure 订阅和服务限制、配额和约束
本文列出了一些最常见的 Azure 限制，有时也称为配额。 本文当前并不涵盖所有 Azure 服务。 随着时间的推移，此列表将进行扩展和更新，以涵盖更多服务。

若要详细了解 Azure 定价，请参阅 [Azure 定价概述](https://www.azure.cn/pricing/)。 在那里，可以使用[定价计算器](https://www.azure.cn/pricing/calculator/)来估算成本。 你还可以转到特定服务的定价详细信息页，例如 [Windows VM](https://www.azure.cn/pricing/details/virtual-machines/#Windows)。 有关帮助管理成本的提示，请参阅[通过 Azure 计费和成本管理来防止意外成本](/billing/billing-getting-started)。

> [!NOTE]
> 如果想要提高限制或配额，使其超出默认限制，可以[免费建立联机客户支持请求](azure-resource-manager/resource-manager-quota-errors.md)。 无法将限制提高到超过下表中显示的最大限制值。 如果没有最大限制列，则资源没有可调整的限制。
>
> [试用订阅](https://www.azure.cn/pricing/1rmb-trial-full)。
>

## <a name="limits-and-the-azure-resource-manager"></a>限制和 Azure 资源管理器
现在可以将多个 Azure 资源合并到单个 Azure 资源组中。 在使用资源组时，以前针对全局的限制会通过 Azure 资源管理器在区域级别进行管理。 有关 Azure 资源组的详细信息，请参阅 [Azure 资源管理器概述](azure-resource-manager/resource-group-overview.md)。

在下面的限制列表中，一个新表反映了在使用 Azure 资源管理器时的限制差异。 例如，会存在一个“订阅限制”表和一个“订阅数限制 - Azure 资源管理器”表   。 如果某个限制同时适用于这两种方案，它会仅显示在第一个表中。 除非另有说明，否则限制是跨所有区域的全局限制。

> [!NOTE]
> Azure 资源组中的资源配额是用户的订阅可以访问的每个区域，而不像服务管理配额那样是可以访问的每个订阅。 让我们以 vCPU 配额为例。 若要请求提高 vCPU 支持数目的配额，必须确定要在哪些区域中使用多少 vCPU。 然后针对所需的 Azure 资源组 vCPU 配额的数量和区域发出特定请求。 如果需要在西欧使用 30 个 vCPU 以在那里运行应用程序，则应专门在西欧请求 30 个 vCPU。 这不会增加任何其他区域的 vCPU 配额 - 西欧的配额为 30 个 vCPU。
> <!-- -->
> 因此，请考虑针对任意区域中的工作负荷来确定所需的 Azure 资源组配额， 然后在需将内容部署到其中的每个区域中请求相应的量。 若要了解如何确定特定区域的当前配额，请参阅[排查部署问题](resource-manager-common-deployment-errors.md)。
>
>

## <a name="service-specific-limits"></a>服务特定的限制
* [Active Directory](#active-directory-limits)
* [API 管理](#api-management-limits)
* [应用服务](#app-service-limits)
* [应用程序网关](#application-gateway-limits)
* [自动化](#automation-limits)
* [用于 Redis 的 Azure 缓存](#azure-cache-for-redis-limits)
* [Azure 云服务](#azure-cloud-services-limits)
* [Azure 认知服务](#azure-cognitive-services-limits)
* [Azure Cosmos DB](#azure-cosmos-db-limits)
* [Azure Database for MySQL](#azure-database-for-mysql)
* [Azure Database for PostgreSQL](#azure-database-for-postgresql)
* [Azure DNS](#azure-dns-limits)
* [Azure 防火墙](#azure-firewall-limits)
* [Azure Functions](#functions-limits)
* [Azure Kubernetes 服务](#azure-kubernetes-service-limits)
* [Azure Monitor](#azure-monitor-limits)
* [Azure Policy](#azure-policy-limits)
* [备份](#backup-limits)
* [批处理](#batch-limits)
* [容器注册表](#container-registry-limits)
* [Data Factory](#data-factory-limits)
* [数据库迁移服务](#database-migration-service-limits)
* [事件网格](#event-grid-limits)
* [事件中心](#event-hubs-limits)
* [IoT 中心](#iot-hub-limits)
* [IoT 中心设备预配服务](#iot-hub-device-provisioning-service-limits)
* [密钥保管库](#key-vault-limits)
* [媒体服务](#media-services-limits)
* [移动服务](#mobile-services-limits)
* [多重身份验证](#multi-factor-authentication-limits)
* [联网](#networking-limits)
  * [应用程序网关](#application-gateway-limits)
  * [Azure DNS](#azure-dns-limits)
  * [Azure 防火墙](#azure-firewall-limits)
  * [ExpressRoute](#expressroute-limits)
  * [负载均衡器](#load-balancer)
  * [网络观察程序](#network-watcher-limits)
  * [公共 IP 地址](#publicip-address)
  * [流量管理器](#traffic-manager-limits)
  * [虚拟网络](#networking-limits)
* [通知中心](#notification-hubs-limits)
* [资源组](#resource-group-limits)
* [基于角色的访问控制](#role-based-access-control-limits)
* [计划程序](#scheduler-limits)
* [服务总线](#service-bus-limits)
* [站点恢复](#site-recovery-limits)
* [SQL 数据库](#sql-database-limits)
* [SQL 数据仓库](#sql-data-warehouse-limits)
* [存储](#storage-limits)
* [流分析](#stream-analytics-limits)
* [订阅](#subscription-limits)
* [虚拟机](#virtual-machines-limits)
* [虚拟机规模集](#virtual-machine-scale-sets-limits)

### <a name="subscription-limits"></a>订阅限制
#### <a name="subscription-limits---azure-service-management-classic-deployment-model"></a>订阅限制 - Azure 服务管理（经典部署模型）
[!INCLUDE [azure-subscription-limits](../includes/azure-subscription-limits.md)]

#### <a name="subscription-limits---azure-resource-manager"></a>订阅数限制 - Azure Resource Manager
使用 Azure 资源管理器和 Azure 资源组时，以下限制适用。 Azure 资源管理器的未有变化的限制未列出。 请参阅上表了解这些限制。

有关资源管理器 API 读写限制的信息，请参阅[限制资源管理器请求](resource-manager-request-limits.md)。

[!INCLUDE [azure-subscription-limits-azure-resource-manager](../includes/azure-subscription-limits-azure-resource-manager.md)]

### <a name="resource-group-limits"></a>资源组限制
[!INCLUDE [azure-resource-groups-limits](../includes/azure-resource-groups-limits.md)]

### <a name="virtual-machines-limits"></a>虚拟机限制
#### <a name="virtual-machines-limits"></a>虚拟机限制
[!INCLUDE [azure-virtual-machines-limits](../includes/azure-virtual-machines-limits.md)]

#### <a name="virtual-machines-limits---azure-resource-manager"></a>虚拟机限制 - Azure Resource Manager
使用 Azure 资源管理器和 Azure 资源组时，以下限制适用。 Azure 资源管理器的未有变化的限制未列出。 请参阅上表了解这些限制。

[!INCLUDE [azure-virtual-machines-limits-azure-resource-manager](../includes/azure-virtual-machines-limits-azure-resource-manager.md)]

#### <a name="shared-image-gallery-limits"></a>共享映像库限制

使用共享映像库部署资源时，每个订阅存在限制：
- 每个区域的每个订阅限制为 100 个共享映像库
- 每个区域的每个订阅限制为 1,000 个映像定义
- 每个区域的每个订阅限制为 10,000 个映像版本

### <a name="virtual-machine-scale-sets-limits"></a>虚拟机规模集限制
[!INCLUDE [virtual-machine-scale-sets-limits](../includes/azure-virtual-machine-scale-sets-limits.md)]


### <a name="container-registry-limits"></a>容器注册表限制
下表详细介绍了“基本”、“标准”和“高级”[服务层级](./container-registry/container-registry-skus.md)的功能和限制。

[!INCLUDE [container-registry-limits](../includes/container-registry-limits.md)]

### <a name="azure-kubernetes-service-limits"></a>Azure Kubernetes 服务限制
[!INCLUDE [container-service-limits](../includes/container-service-limits.md)]


### <a name="networking-limits"></a>网络限制
[!INCLUDE [azure-virtual-network-limits](../includes/azure-virtual-network-limits.md)]

#### <a name="expressroute-limits"></a>ExpressRoute 限制
[!INCLUDE [expressroute-limits](../includes/expressroute-limits.md)]

#### <a name="application-gateway-limits"></a>应用程序网关限制

除非另有说明，否则下表适用于 v1、v2、标准和 WAF SKU。
[!INCLUDE [application-gateway-limits](../includes/application-gateway-limits.md)]

#### <a name="network-watcher-limits"></a>网络观察程序限制
[!INCLUDE [network-watcher-limits](../includes/network-watcher-limits.md)]

#### <a name="traffic-manager-limits"></a>流量管理器限制
[!INCLUDE [traffic-manager-limits](../includes/traffic-manager-limits.md)]

#### <a name="azure-dns-limits"></a>Azure DNS 限制
[!INCLUDE [dns-limits](../includes/dns-limits.md)]

#### <a name="azure-firewall-limits"></a>Azure 防火墙限制
[!INCLUDE [azure-firewall-limits](../includes/firewall-limits.md)]


### <a name="storage-limits"></a>存储限制
<!--like # storage accts -->
[!INCLUDE [azure-storage-account-limits-standard](../includes/azure-storage-account-limits-standard.md)]

有关存储帐户限制的详细信息，请参阅 [Azure 存储可伸缩性和性能目标](storage/common/storage-scalability-targets.md)。

#### <a name="storage-resource-provider-limits"></a>存储资源提供程序限制

[!INCLUDE [azure-storage-limits-azure-resource-manager](../includes/azure-storage-limits-azure-resource-manager.md)]

#### <a name="azure-blob-storage-limits"></a>Azure Blob 存储限制
[!INCLUDE [storage-blob-scale-targets](../includes/storage-blob-scale-targets.md)]

#### <a name="azure-files-limits"></a>Azure 文件限制
有关 Azure 文件限制的详细信息，请参阅 [Azure 文件可伸缩性和性能目标](storage/files/storage-files-scale-targets.md)。

[!INCLUDE [storage-files-scale-targets](../includes/storage-files-scale-targets.md)]


#### <a name="azure-queue-storage-limits"></a>Azure 队列存储限制
[!INCLUDE [storage-queues-scale-targets](../includes/storage-queues-scale-targets.md)]

#### <a name="azure-table-storage-limits"></a>Azure 表存储限制
[!INCLUDE [storage-tables-scale-targets](../includes/storage-tables-scale-targets.md)]

<!-- conceptual info about disk limits -- applies to unmanaged and managed -->
#### <a name="virtual-machine-disk-limits"></a>虚拟机磁盘限制
[!INCLUDE [azure-storage-limits-vm-disks](../includes/azure-storage-limits-vm-disks.md)]

有关详细信息，请参阅[虚拟机大小](virtual-machines/linux/sizes.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)。

#### <a name="managed-virtual-machine-disks"></a>托管虚拟机磁盘

[!INCLUDE [azure-storage-limits-vm-disks-managed](../includes/azure-storage-limits-vm-disks-managed.md)]

#### <a name="unmanaged-virtual-machine-disks"></a>非托管虚拟机磁盘

[!INCLUDE [azure-storage-limits-vm-disks-standard](../includes/azure-storage-limits-vm-disks-standard.md)]

[!INCLUDE [azure-storage-limits-vm-disks-premium](../includes/azure-storage-limits-vm-disks-premium.md)]

### <a name="azure-cloud-services-limits"></a>Azure 云服务限制
[!INCLUDE [azure-cloud-services-limits](../includes/azure-cloud-services-limits.md)]

### <a name="azure-cognitive-services-limits"></a>Azure 认知服务限制
[!INCLUDE [azure-cloud-services-limits](../includes/azure-cognitive-services-limits.md)]

### <a name="app-service-limits"></a>应用服务限制
以下应用服务限制包括 Web 应用、移动应用、API 应用的限制。

[!INCLUDE [azure-websites-limits](../includes/azure-websites-limits.md)]

### <a name="functions-limits"></a>Functions 限制
[!INCLUDE [functions-limits](../includes/functions-limits.md)]

### <a name="scheduler-limits"></a>计划程序限制
[!INCLUDE [scheduler-limits-table](../includes/scheduler-limits-table.md)]

### <a name="batch-limits"></a>批处理限制
[!INCLUDE [azure-batch-limits](../includes/azure-batch-limits.md)]
### <a name="azure-cosmos-db-limits"></a>Azure Cosmos DB 限制
有关 Azure Cosmos DB 的限制，请参阅 [Azure Cosmos DB 中的限制](cosmos-db/concepts-limits.md)。

### <a name="azure-database-for-mysql"></a>Azure Database for MySQL
有关 Azure Database for MySQL 限制，请参阅 [Azure Database for MySQL 中的限制](mysql/concepts-limits.md)。

### <a name="azure-database-for-postgresql"></a>Azure Database for PostgreSQL
有关 Azure Database for PostgreSQL 限制，请参阅 [Azure Database for PostgreSQL 中的限制](postgresql/concepts-limits.md)。

### <a name="media-services-limits"></a>媒体服务限制
[!INCLUDE [azure-mediaservices-limits](../includes/azure-mediaservices-limits.md)]
### <a name="mobile-services-limits"></a>移动服务限制
[!INCLUDE [mobile-services-limits](../includes/mobile-services-limits.md)]

### <a name="azure-monitor-limits"></a>Azure Monitor 限制

#### <a name="alerts"></a>警报

[!INCLUDE [monitoring-limits](../includes/azure-monitor-limits-alerts.md)]

#### <a name="action-groups"></a>操作组

[!INCLUDE [monitoring-limits](../includes/azure-monitor-limits-action-groups.md)]

#### <a name="log-queries-and-language"></a>日志查询和语言

[!INCLUDE [monitoring-limits](../includes/azure-monitor-limits-log-queries.md)]

#### <a name="log-analytics-workspaces"></a>Log Analytics 工作区

[!INCLUDE [monitoring-limits](../includes/azure-monitor-limits-workspaces.md)]

#### <a name="application-insights"></a>Application Insights

[!INCLUDE [monitoring-limits](../includes/azure-monitor-limits-app-insights.md)]




### <a name="notification-hubs-limits"></a>通知中心限制
[!INCLUDE [notification-hub-limits](../includes/notification-hub-limits.md)]

### <a name="event-hubs-limits"></a>事件中心限制
[!INCLUDE [azure-servicebus-limits](../includes/event-hubs-limits.md)]

### <a name="service-bus-limits"></a>服务总线限制
[!INCLUDE [azure-servicebus-limits](../includes/service-bus-quotas-table.md)]

### <a name="iot-hub-limits"></a>IoT 中心限制
[!INCLUDE [azure-iothub-limits](../includes/iot-hub-limits.md)]

### <a name="iot-hub-device-provisioning-service-limits"></a>IoT 中心设备预配服务限制
[!INCLUDE [azure-iotdps-limits](../includes/iot-dps-limits.md)]

### <a name="data-factory-limits"></a>数据工厂限制
[!INCLUDE [azure-data-factory-limits](../includes/azure-data-factory-limits.md)]

### <a name="database-migration-service-limits"></a>数据库迁移服务限制
[!INCLUDE [database-migration-service-limits](../includes/database-migration-service-limits.md)]

### <a name="stream-analytics-limits"></a>流分析限制
[!INCLUDE [stream-analytics-limits-table](../includes/stream-analytics-limits-table.md)]

### <a name="active-directory-limits"></a>Active Directory 限制
[!INCLUDE [AAD-service-limits](../includes/active-directory-service-limits-include.md)]

### <a name="event-grid-limits"></a>事件网格限制
[!INCLUDE [event-grid-limits](../includes/event-grid-limits.md)]

### <a name="azure-policy-limits"></a>Azure Policy 限制
[!INCLUDE [policy-limits](../includes/azure-policy-limits.md)]

### <a name="backup-limits"></a>备份限制
[!INCLUDE [azure-backup-limits](../includes/azure-backup-limits.md)]

### <a name="site-recovery-limits"></a>站点恢复限制
[!INCLUDE [site-recovery-limits](../includes/site-recovery-limits.md)]

### <a name="api-management-limits"></a>API 管理限制
[!INCLUDE [api-management-service-limits](../includes/api-management-service-limits.md)]

### <a name="azure-cache-for-redis-limits"></a>Azure Redis 缓存限制
[!INCLUDE [redis-cache-service-limits](../includes/redis-cache-service-limits.md)]

### <a name="key-vault-limits"></a>密钥保管库限制
[!INCLUDE [key-vault-limits](../includes/key-vault-limits.md)]

### <a name="multi-factor-authentication-limits"></a>多重身份验证限制
[!INCLUDE [azure-mfa-service-limits](../includes/azure-mfa-service-limits.md)]

### <a name="automation-limits"></a>自动化限制
[!INCLUDE [automation-limits](../includes/azure-automation-service-limits.md)]

### <a name="identity-manager-limits"></a>Identity Manager 限制
[!INCLUDE [automation-limits](~/includes/managed-identity-limits.md)]

### <a name="role-based-access-control-limits"></a>基于角色的访问控制限制
[!INCLUDE [role-based-access-control-limits](../includes/role-based-access-control-limits.md)]

### <a name="sql-database-limits"></a>SQL 数据库限制
有关 SQL 数据库的限制，请参阅[单一数据库的 SQL 数据库资源限制](sql-database/sql-database-vcore-resource-limits-single-databases.md)、[弹性池和共用数据库的 SQL 数据库资源限制](sql-database/sql-database-vcore-resource-limits-elastic-pools.md)以及[托管实例的 SQL 数据库资源限制](sql-database/sql-database-managed-instance-resource-limits.md)。

### <a name="sql-data-warehouse-limits"></a>SQL 数据仓库限制
有关 SQL 数据仓库限制，请参阅 [SQL 数据仓库资源限制](sql-data-warehouse/sql-data-warehouse-service-capacity-limits.md)。

## <a name="see-also"></a>另请参阅
- [了解 Azure 限制及如何提高限制](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/)
- [Azure 的虚拟机和云服务大小](virtual-machines/linux/sizes.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
- [Azure 云服务的大小](cloud-services/cloud-services-sizes-specs.md)
