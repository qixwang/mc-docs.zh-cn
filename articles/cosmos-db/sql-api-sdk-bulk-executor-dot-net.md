---
title: Azure Cosmos DB：批量执行程序 .NET API、SDK 和资源
description: 了解有关批量执行程序 .NET API 和 SDK 的所有信息。
author: rockboyfor
ms.service: cosmos-db
ms.subservice: cosmosdb-sql
ms.devlang: dotnet
ms.topic: reference
origin.date: 01/16/2020
ms.date: 02/10/2020
ms.author: v-yeche
ms.openlocfilehash: bd4165e3b489eeb8e721dd045a0e66e2a217a860
ms.sourcegitcommit: c1ba5a62f30ac0a3acb337fb77431de6493e6096
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2020
ms.locfileid: "77028727"
---
# <a name="net-bulk-executor-library-download-information"></a>.NET 批量执行程序库：下载信息 

> [!div class="op_single_selector"]
> * [.NET](sql-api-sdk-dotnet.md)
> * [.NET 更改源](sql-api-sdk-dotnet-changefeed.md)
> * [.NET Core](sql-api-sdk-dotnet-core.md)
> * [Node.js](sql-api-sdk-node.md)
> * [异步 Java](sql-api-sdk-async-java.md)
> * [Java](sql-api-sdk-java.md)
> * [Python](sql-api-sdk-python.md)
> * [REST](https://docs.microsoft.com/rest/api/cosmos-db/)
> * [REST 资源提供程序](https://docs.microsoft.com/rest/api/cosmos-db-resource-provider/)
> * [SQL](sql-api-query-reference.md)
> * [批量执行程序 - .NET](sql-api-sdk-bulk-executor-dot-net.md)
> * [批量执行程序 - Java](sql-api-sdk-bulk-executor-java.md)

| |  |
|---|---|
| **说明**| .NET 批量执行程序库允许客户端应用程序对 Azure Cosmos DB 帐户执行批量操作。 此库提供了 BulkImport、BulkUpdate 和 BulkDelete 命名空间。 BulkImport 模块可以批量以优化方式引入文档，以便最大程度地使用为集合配置的吞吐量。 BulkUpdate 模块可以作为修补程序批量更新 Azure Cosmos 容器中的现有数据。 BulkDelete 模块可以批量以优化方式删除文档，以便最大程度地使用为集合配置的吞吐量。|
|**SDK 下载**| [NuGet](https://www.nuget.org/packages/Microsoft.Azure.CosmosDB.BulkExecutor/) |
| **GitHub 中的批量执行程序库**| [GitHub](https://github.com/Azure/azure-cosmosdb-bulkexecutor-dotnet-getting-started)|
|**API 文档**|[ 参考文档](https://docs.microsoft.com/dotnet/api/microsoft.azure.cosmosdb.bulkexecutor?view=azure-dotnet)|
|**入门**|[批量执行程序库 .NET SDK 入门](bulk-executor-dot-net.md)|
| **当前受支持的框架**| Azure .NET Framework 4.5.2、4.6.1 和 .NET Standard 2.0 |

> [!NOTE]
> 如果使用批量执行程序，请参阅 [.NET SDK](tutorial-sql-api-dotnet-bulk-import.md) 的最新版本 3.x，其中内置了批量执行程序。 

<!-- Not Available on ## Release notes-->

## <a name="next-steps"></a>后续步骤

若要了解批量执行程序 Java 库，请参阅以下文章：

[Java 批量执行程序库 SDK 和发行信息](sql-api-sdk-bulk-executor-java.md)

<!--Update_Description: update meta properties, wording update-->