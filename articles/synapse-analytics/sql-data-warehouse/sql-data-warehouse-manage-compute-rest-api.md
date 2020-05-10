---
title: 使用 REST API 进行暂停、恢复、缩放
description: 通过 REST API 管理 Azure Synapse Analytics 数据仓库中的计算能力。
services: synapse-analytics
author: WenJason
manager: digimobile
ms.service: synapse-analytics
ms.topic: conceptual
ms.subservice: ''
origin.date: 03/29/2019
ms.date: 05/11/2020
ms.author: v-jay
ms.reviewer: igorstan
ms.custom: seo-lt-2019, azure-synapse
ms.openlocfilehash: d7a967f9b0b98e3f1689a5f9dc3b47bb218b300f
ms.sourcegitcommit: f8d6fa25642171d406a1a6ad6e72159810187933
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/28/2020
ms.locfileid: "82198672"
---
# <a name="rest-apis-for-azure-sql-data-warehouse"></a>Azure SQL 数据仓库的 REST API

用于管理 Azure Synapse Analytics 数据仓库中的计算的 REST API。

## <a name="scale-compute"></a>缩放计算

若要更改数据仓库单位，请使用[创建或更新数据库](https://docs.microsoft.com/rest/api/sql/databases/createorupdate?toc=/synapse-analytics/sql-data-warehouse/toc.json&bc=/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json) REST API。 以下示例将托管在服务器 MyServer 上的数据库 MySQLDW 的数据仓库单位设置为 DW1000。 该服务器位于名为 ResourceGroup1 的 Azure 资源组中。

```
PATCH https://management.chinacloudapi.cn/subscriptions/{subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.Sql/servers/{server-name}/databases/{database-name}?api-version=2014-04-01-preview HTTP/1.1
Content-Type: application/json; charset=UTF-8

{
    "properties": {
        "requestedServiceObjectiveName": DW1000
    }
}
```

## <a name="pause-compute"></a>暂停计算

若要暂停数据库，请使用[暂停数据库](https://docs.microsoft.com/rest/api/sql/databases/pause?toc=/synapse-analytics/sql-data-warehouse/toc.json&bc=/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json) REST API。 以下示例暂停 Server01 服务器上托管的 Database02 数据库。 该服务器位于名为 ResourceGroup1 的 Azure 资源组中。

```
POST https://management.chinacloudapi.cn/subscriptions/{subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.Sql/servers/{server-name}/databases/{database-name}/pause?api-version=2014-04-01-preview HTTP/1.1
```

## <a name="resume-compute"></a>恢复计算

若要启动数据库，请使用[恢复数据库](https://docs.microsoft.com/rest/api/sql/databases/resume?toc=/synapse-analytics/sql-data-warehouse/toc.json&bc=/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json) REST API。 以下示例启动 Server01 服务器上托管的 Database02 数据库。 该服务器位于名为 ResourceGroup1 的 Azure 资源组中。 

```
POST https://management.chinacloudapi.cn/subscriptions/{subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.Sql/servers/{server-name}/databases/{database-name}/resume?api-version=2014-04-01-preview HTTP/1.1
```

## <a name="check-database-state"></a>检查数据库状态

> [!NOTE]
> 当前 Check database state 可能会在数据库完成联机工作流时返回 ONLINE，从而导致连接错误。 如果使用此 API 调用触发连接尝试，则可能需要在应用程序代码中添加 2 到 3 分钟的延迟。

```
GET https://management.chinacloudapi.cn/subscriptions/{subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.Sql/servers/{server-name}/databases/{database-name}?api-version=2014-04-01 HTTP/1.1
```

## <a name="get-maintenance-schedule"></a>获取维护计划

检查已为数据仓库设置的维护计划。

```
GET https://management.chinacloudapi.cn/subscriptions/{subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.Sql/servers/{server-name}/databases/{database-name}/maintenanceWindows/current?maintenanceWindowName=current&api-version=2017-10-01-preview HTTP/1.1

```

## <a name="set-maintenance-schedule"></a>设置维护计划

设置和更新现有数据仓库的维护计划。

```
PUT https://management.chinacloudapi.cn/subscriptions/{subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.Sql/servers/{server-name}/databases/{database-name}/maintenanceWindows/current?maintenanceWindowName=current&api-version=2017-10-01-preview HTTP/1.1

{
    "properties": {
        "timeRanges": [
                {
                                "dayOfWeek": Saturday,
                                "startTime": 00:00,
                                "duration": 08:00,
                },
                {
                                "dayOfWeek": Wednesday
                                "startTime": 00:00,
                                "duration": 08:00,
                }
                ]
    }
}

```

## <a name="next-steps"></a>后续步骤

有关详细信息，请参阅[管理计算](sql-data-warehouse-manage-compute-overview.md)。
