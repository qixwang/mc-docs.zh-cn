---
title: Azure Cosmos DB 的 Azure 资源管理器模板
description: 使用 Azure 资源管理器模板创建和配置 Azure Cosmos DB。
author: rockboyfor
ms.service: cosmos-db
ms.topic: conceptual
origin.date: 05/08/2020
ms.date: 07/06/2020
ms.author: v-yeche
ms.openlocfilehash: 5fae28a6550e247206922f6b964f39b3e0324d7e
ms.sourcegitcommit: f5484e21fa7c95305af535d5a9722b5ab416683f
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/24/2020
ms.locfileid: "85321576"
---
<!--Verify successfully-->
# <a name="azure-resource-manager-templates-for-azure-cosmos-db"></a>用于 Azure Cosmos DB 的 Azure 资源管理器模板

下表包含 Azure Cosmos DB 的 Azure 资源管理器模板链接：

## <a name="core-sql-api"></a>Core (SQL) API

|**模板**|**说明**|
|---|---|
|[创建具有自动缩放吞吐量的 Azure Cosmos 帐户、数据库和容器](manage-sql-with-resource-manager.md#create-autoscale) | 此模板在两个区域（具有自动缩放吞吐量的数据库和容器）中创建一个 Core (SQL) API 帐户。 |
|[创建具有分析存储的 Azure Cosmos 帐户、数据库和容器](manage-sql-with-resource-manager.md#create-analytical-store) | 此模板在一个区域中创建一个 Core (SQL) API 帐户，其中包含配置了启用分析 TTL 的容器和手动或自动缩放吞吐量选项。 |
|[创建具有标准（手动）吞吐量的 Azure Cosmos 帐户、数据库和容器](manage-sql-with-resource-manager.md#create-manual) | 此模板在两个区域（具有标准吞吐量的数据库和容器）中创建一个 Core (SQL) API 帐户。 |
|[创建包含存储过程、触发器和 UDF 的 Azure Cosmos 帐户、数据库和容器](manage-sql-with-resource-manager.md#create-sproc) | 此模板在两个区域（具有存储过程、触发器和容器的 UDF）中创建了一个 Core (SQL) API 帐户。 |

<!--Not Available on |[Create a private endpoint for an existing Azure Cosmos account](how-to-configure-private-endpoints.md#create-a-private-endpoint-by-using-a-resource-manager-template) |  This template creates a private endpoint for an existing Azure Cosmos Core (SQL) API account in an existing virtual network. |-->
<!--Not Available on |[Create a free-tier Azure Cosmos account](manage-sql-with-resource-manager.md#free-tier) |  This template creates an Azure Cosmos DB Core (SQL) API account on free-tier. |-->

## <a name="mongodb-api"></a>MongoDB API

|**模板**|**说明**|
|---| ---|
|[创建具有自动缩放吞吐量的 Azure Cosmos 帐户、数据库和集合](manage-mongodb-with-resource-manager.md#create-autoscale) | 此模板在两个区域（具有两个共享数据库级自动缩放吞吐量的容器）中使用 Azure Cosmos DB API for MongoDB 创建一个帐户。 |
|[创建具有标准（手动）吞吐量的 Azure Cosmos 帐户、数据库和集合](manage-mongodb-with-resource-manager.md#create-manual) | 此模板在两个区域（具有两个共享数据库级标准吞吐量的容器）中使用 Azure Cosmos DB API for MongoDB 创建一个帐户。 |

## <a name="cassandra-api"></a>Cassandra API

|**模板**|**说明**|
|---| ---|
|[创建具有自动缩放吞吐量的 Azure Cosmos 帐户、密钥空间和表](manage-cassandra-with-resource-manager.md#create-autoscale) | 此模板在两个区域（具有自动缩放吞吐量的密钥空间和表）中创建一个 Cassandra API 帐户。 |
|[创建具有标准（手动）吞吐量的 Azure Cosmos 帐户、密钥空间和表](manage-cassandra-with-resource-manager.md#create-manual) | 此模板在两个区域（具有手动吞吐量的密钥空间和表）中创建一个 Cassandra API 帐户。 |

## <a name="gremlin-api"></a>Gremlin API

|**模板**|**说明**|
|---| ---|
|[创建具有自动缩放吞吐量的 Azure Cosmos 帐户、数据库和图](manage-gremlin-with-resource-manager.md#create-autoscale) | 此模板在两个区域（具有自动缩放吞吐量的数据库和图）中创建一个 Gremlin API 帐户。 |
|[创建具有标准（手动）吞吐量的 Azure Cosmos 帐户、数据库和图](manage-gremlin-with-resource-manager.md#create-manual) | 此模板在两个区域（具有标准吞吐量的数据库和图）中创建一个 Gremlin API 帐户。 |

## <a name="table-api"></a>表 API

|**模板**|**说明**|
|---| ---|
|[创建具有自动缩放吞吐量的 Azure Cosmos 帐户和表](manage-table-with-resource-manager.md#create-autoscale) | 此模板在两个区域中创建一个 Table API 帐户，并创建一个具有自动缩放吞吐量的表。 |
|[创建具有标准（手动）吞吐量的 Azure Cosmos 帐户和表](manage-table-with-resource-manager.md#create-manual) | 此模板在两个区域中创建一个 Table API 帐户，并创建一个具有标准吞吐量的表。 |

<!--Not Available on [Azure Resource Manager reference for Azure Cosmos DB](https://docs.microsoft.com/azure/templates/microsoft.documentdb/allversions)-->

<!-- Update_Description: update meta properties, wording update, update link -->