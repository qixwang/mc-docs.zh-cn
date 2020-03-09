---
title: Azure 中的监视解决方案清单 | Microsoft Docs
description: Azure Monitor 中的监视解决方案是逻辑、可视化效果和数据采集规则的集合，提供围绕特定问题领域制定的指标。  本文提供了 Microsoft 提供的监视解决方案的列表以及有关其数据收集方法和频率的详细信息。
author: lingliw
manager: digimobile
ms.subservice: logs
ms.topic: conceptual
origin.date: 06/26/2018
ms.date: 8/4/2019
ms.author: v-lingwu
ms.openlocfilehash: a9678309e6ebecc3fc93a306e7a2e856f2af0115
ms.sourcegitcommit: b7fe28ec2de92b5befe61985f76c8d0216f23430
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/06/2020
ms.locfileid: "78850313"
---
# <a name="inventory-and-data-collection-details-for-monitoring-solutions-in-azure"></a>Azure 中的监视解决方案的清单和数据收集详细信息
[监视解决方案](solutions.md)利用 Azure 中的服务来提供特定应用程序或服务的更多操作见解。 监视解决方案通常收集日志数据并提供查询和视图，用于分析收集的数据。 可以在 Azure Monitor 中针对你使用的任何应用程序和服务添加监视解决方案。 这些解决方案是免费提供的，但收集数据可能会产生使用费。

本文包括了 Microsoft 提供的[监视解决方案](solutions.md)的列表以及指向其详细文档的链接。  它还提供了这些解决方案将数据收集到 Azure Monitor 中时采用的方法和频率的相关信息。  可以使用本文中的信息来了解可用的各种解决方案，并了解各种监视解决方案的数据流和连接要求。

[!INCLUDE [azure-monitor-log-analytics-rebrand](../../../includes/azure-monitor-log-analytics-rebrand.md)]

## <a name="list-of-monitoring-solutions"></a>监视解决方案的列表

下表列出了 Azure 中由 Microsoft 提供的[监视解决方案](solutions.md)的列表。 列中的条目意味着解决方案使用该方法将数据收集到 Azure Monitor 中。  如果解决方案没有选择任何列，则它将从另一个 Azure 服务直接写入到 Azure Monitor 中。 访问每个解决方案的链接可以查看其详细文档来了解更多信息。

各个列的说明如下：

- **Microsoft Monitoring Agent** - Windows 和 Linux 上用来运行来自 SCOM 的管理包和来自 Azure 的监视解决方案的代理。 在此配置中，代理直接连接到 Azure Monitor，无需连接到 Operations Manager 管理组。 
-  **Azure 存储** - 此解决方案从 Azure 存储帐户收集数据。 
- **是否需要 Operations Manager？** - 监视解决方案进行数据收集时需要一个已连接的 Operations Manager 管理组。 
- **收集频率** - 指定监视解决方案收集数据时采用的频率。 



| **监视解决方案** | **平台** | **Microsoft Monitoring Agent** | **Operations Manager 代理** | **Azure 存储** | **是否需要 Operations Manager？** | **通过管理组发送 Operations Manager 代理数据** | **收集频率** |
| --- | --- | --- | --- | --- | --- | --- | --- |
| [活动日志分析](../platform/activity-log-collect.md) | Azure | | | | | | 通知时 |
| [AD 评估](ad-assessment.md) |Windows |&#8226; |&#8226; | | |&#8226; |7 天 |
| [AD 复制状态](ad-replication-status.md) |Windows |&#8226; |&#8226; | | |&#8226; |5 天 |
| [代理运行状况](solution-agenthealth.md) | Windows 和 Linux | &#8226; | &#8226; | | | &#8226; | 1 分钟 |
| [警报管理](../platform/alert-management-solution.md) (Nagios) |Linux |&#8226; | | | | |到达时 |
| [警报管理](../platform/alert-management-solution.md) (Zabbix) |Linux |&#8226; | | | | |1 分钟 |
| [警报管理](../platform/alert-management-solution.md) (Operations Manager) |Windows | |&#8226; | |&#8226; |&#8226; |3 分钟 |
| [Azure Site Recovery](../../site-recovery/site-recovery-overview.md) | Azure | | | | | | 不适用 |
| [Application Insights 连接器（已弃用）](../platform/app-insights-connector.md) | Azure | | | |  |  | 通知时 |
| [Azure 应用程序网关分析](azure-networking-analytics.md) | Azure |  |  |  |  |  | 通知时 |
| **监视解决方案** | **平台** | **Microsoft Monitoring Agent** | **Operations Manager 代理** | **Azure 存储** | **是否需要 Operations Manager？** | **通过管理组发送 Operations Manager 代理数据** | **收集频率** |
| [Azure 网络安全组分析（已弃用）](azure-networking-analytics.md) | Azure |  |  |  |  |  | 通知时 |
| [备份](https://azure.microsoft.com/resources/templates/101-backup-oms-monitoring/) | Azure |  |  |  |  |  | 通知时 |
| [容量和性能（预览版）](capacity-performance.md) |Windows |&#8226; |&#8226; | | |&#8226; |到达时 |
| [容器](containers.md) | Windows 和 Linux | &#8226; | &#8226; |  |  |  | 3 分钟 |
| [密钥保管库分析](azure-key-vault.md) |Windows | | | | | |通知时 |
| [网络性能监视器](network-performance-monitor.md) | Windows | &#8226; | &#8226; |  |  |  | 每隔 5 秒钟进行 TCP 握手，每隔 3 分钟发送数据 |
| [Office 365 分析（预览版）](solution-office-365.md) |Windows | | | | | |通知时 |
| **监视解决方案** | **平台** | **Microsoft Monitoring Agent** | **Operations Manager 代理** | **Azure 存储** | **是否需要 Operations Manager？** | **通过管理组发送 Operations Manager 代理数据** | **收集频率** |
| [服务地图](service-map.md) | Windows 和 Linux | &#8226; | &#8226; |  |  |  | 15 秒 |
| [SQL 评估](sql-assessment.md) |Windows |&#8226; |&#8226; | | |&#8226; |7 天 |
| [SurfaceHub](surface-hubs.md) |Windows |&#8226; | | | | |到达时 |
| [System Center Operations Manager 评估（预览版）](scom-assessment.md) | Windows | &#8226; | &#8226; |  |  | &#8226; | 七天 |
| [升级准备情况](https://docs.microsoft.com/windows/deployment/upgrade/upgrade-readiness-get-started) | Windows | &#8226; |  |  |  |  | 2 天 |
| [VMware 监视（已弃用）](vmware.md) | Linux | &#8226; |  |  |  |  | 3 分钟 |
| [Wire Data 2.0（预览版）](wire-data.md) |Windows（2012 R2 / 8.1 或更高版本） |&#8226; |&#8226; | | | | 1 分钟 |




## <a name="next-steps"></a>后续步骤
* 了解如何[安装和使用监视解决方案](solutions.md)。
* 了解如何[创建查询](../log-query/log-query-overview.md)来分析监视解决方案收集的数据。
