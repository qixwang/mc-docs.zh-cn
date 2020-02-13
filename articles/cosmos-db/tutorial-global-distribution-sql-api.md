---
title: 教程：SQL API 的 Azure Cosmos DB 多区域分发教程
description: 教程：了解如何将 SQL API 与 .Net、Java、Python 和各种其他 SDK 配合使用来设置 Azure Cosmos DB 多区域分发
author: rockboyfor
ms.author: v-yeche
ms.service: cosmos-db
ms.topic: tutorial
origin.date: 11/05/2019
ms.date: 02/10/2020
ms.reviewer: sngun
ms.openlocfilehash: 00b17bfbe2aafeb51d19661c1ed8f62bb198a658
ms.sourcegitcommit: 925c2a0f6c9193c67046b0e67628d15eec5205c3
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/07/2020
ms.locfileid: "77067897"
---
# <a name="tutorial-set-up-azure-cosmos-db-multiple-region-distribution-using-the-sql-api"></a>教程：使用 SQL API 设置 Azure Cosmos DB 多区域分发

本文介绍如何使用 Azure 门户设置 Azure Cosmos DB 多区域分发，然后使用 SQL API 进行连接。

本文涵盖以下任务： 

> [!div class="checklist"]
> * 使用 Azure 门户配置多区域分发
> * 使用 [SQL API](sql-api-introduction.md) 配置多区域分发

<a name="portal"></a>
[!INCLUDE [cosmos-db-tutorial-global-distribution-portal](../../includes/cosmos-db-tutorial-global-distribution-portal.md)]

## <a name="connecting-to-a-preferred-region-using-the-sql-api"></a>使用 SQL API 连接到首选区域

为了利用[多区域分发](distribute-data-globally.md)，客户端应用程序可以指定要用于执行文档操作的区域优先顺序列表。 可通过设置连接策略来实现此目的。 SQL SDK 根据 Azure Cosmos DB 帐户配置、当前区域可用性和指定的优先顺序列表，选择最佳的终结点来执行写入和读取操作。

此优先顺序列表是在使用 SQL SDK 初始化连接时指定的。 SDK 接受可选参数“PreferredLocations”，这是 Azure 区域的顺序列表。

SDK 会自动将所有写入请求发送到当前写入区域。

所有读取请求将发送到 PreferredLocations 列表中的第一个可用区域。 如果请求失败，客户端会将请求转发到列表中的下一个区域，依此类推。

SDK 只会尝试读取 PreferredLocations 中指定的区域。 因此，例如，如果数据库帐户在四个区域中可用，但客户端只为 PreferredLocations 指定了两个读取（非写入）区域，那么将不会从 PreferredLocations 中未指定的读取区域为读取提供服务。 如果 PreferredLocations 中指定的读取区域不可用，将从写入区域为读取提供服务。

应用程序可以通过检查 SDK 1.8 和更高版本中提供的两个属性 - WriteEndpoint 和 ReadEndpoint - 来验证 SDK 选择的当前写入终结点和读取终结点。

如果未设置 PreferredLocations 属性，则会从当前写入区域为所有请求提供服务。

## <a name="net-sdk"></a>.NET SDK
无需进行任何代码更改即可使用该 SDK。 在此情况下，SDK 会自动将读取和写入请求定向到当前写入区域。

在 .NET SDK 1.8 和更高版本中，DocumentClient 构造函数的 ConnectionPolicy 参数有一个名为 Microsoft.Azure.Documents.ConnectionPolicy.PreferredLocations 的属性。 此属性的类型为 Collection `<string>` ，应包含区域名称的列表。 字符串值已根据 [Azure 区域][regions]页上的“区域名称”列设置格式，其第一个字符的前面和最后一个字符的后面均没有空格。

当前写入终结点和读取终结点分别在 DocumentClient.WriteEndpoint 和 DocumentClient.ReadEndpoint 中提供。

> [!NOTE]
> 不应将终结点 URL 视为长期不变的常量。 服务随时会更新这些 URL。 SDK 会自动处理这种更改。
>
>

<!--MOONCAKE: WestUS TO CHINANORTH, EastUS TO China East, NorthEuropen TO CHINA EAST 2-->

```csharp
// Getting endpoints from application settings or other configuration location
Uri accountEndPoint = new Uri(Properties.Settings.Default.GlobalDatabaseUri);
string accountKey = Properties.Settings.Default.GlobalDatabaseKey;

ConnectionPolicy connectionPolicy = new ConnectionPolicy();

//Setting read region selection preference
connectionPolicy.PreferredLocations.Add(LocationNames.ChinaNorth); // first preference
connectionPolicy.PreferredLocations.Add(LocationNames.ChinaEast); // second preference
connectionPolicy.PreferredLocations.Add(LocationNames.ChinaEast2); // third preference

// initialize connection
DocumentClient docClient = new DocumentClient(
    accountEndPoint,
    accountKey,
    connectionPolicy);

// connect to DocDB
await docClient.OpenAsync().ConfigureAwait(false);
```

## <a name="nodejsjavascript"></a>Node.js/JavaScript

> [!NOTE]
> 不应将终结点 URL 视为长期不变的常量。 服务随时会更新这些 URL。 SDK 会自动处理这种更改。
>
>

下面是 Node.js/Javascript 的代码示例。

<!--MOONCAKE: CORRECT FOR China East 2-->

```JavaScript
// Setting read region selection preference, in the following order -
// 1 - China North
// 2 - China East
// 3 - China East 2
const preferredLocations = ['China North', 'China East', 'China East 2'];

// initialize the connection
const client = new CosmosClient{ endpoint, key, connectionPolicy: { preferredLocations } });
```

## <a name="python-sdk"></a>Python SDK

以下代码演示如何使用 Python SDK 设置首选位置：

```python

connectionPolicy = documents.ConnectionPolicy()
connectionPolicy.PreferredLocations = ['China North', 'China East', 'China East 2']
client = cosmos_client.CosmosClient(ENDPOINT, {'masterKey': MASTER_KEY}, connectionPolicy)

```

## <a name="java-v2-sdk"></a>Java V2 SDK

以下代码演示如何使用 Java SDK 设置首选位置：

```java
ConnectionPolicy policy = new ConnectionPolicy();
policy.setUsingMultipleWriteLocations(true);
policy.setPreferredLocations(Arrays.asList("China East", "China North", "China East 2"));
AsyncDocumentClient client =
        new AsyncDocumentClient.Builder()
                .withMasterKeyOrResourceToken(this.accountKey)
                .withServiceEndpoint(this.accountEndpoint)
                .withConnectionPolicy(policy)
                .build();
```

<!--MOONCAKE: CORRECT FOR China East 2-->

## <a name="rest"></a>REST
数据库帐户在多个区域中可用后，客户端可以通过对以下 URI 执行 GET 请求来查询该帐户的可用性。

    https://{databaseaccount}.documents.azure.cn/

服务返回副本的区域及其对应 Azure Cosmos DB 终结点 URI 的列表。 当前写入区域会在响应中指示。 然后，客户端可为所有其他 REST API 请求选择适当的终结点，如下所示。

示例响应

<!--MOONCAKE: WestUS TO CHINANORTH, EastUS TO China East, NorthEuropen TO CHINA EAST 2-->

    {
        "_dbs": "//dbs/",
        "media": "//media/",
        "writableLocations": [
            {
                "Name": "China North",
                "DatabaseAccountEndpoint": "https://globaldbexample-chinanorth.documents.azure.cn:443/"
            }
        ],
        "readableLocations": [
            {
                "Name": "China East",
                "DatabaseAccountEndpoint": "https://globaldbexample-chinaeast.documents.azure.cn:443/"
            }
        ],
        "MaxMediaStorageUsageInMB": 2048,
        "MediaStorageUsageInMB": 0,
        "ConsistencyPolicy": {
            "defaultConsistencyLevel": "Session",
            "maxStalenessPrefix": 100,
            "maxIntervalInSeconds": 5
        },
        "addresses": "//addresses/",
        "id": "globaldbexample",
        "_rid": "globaldbexample.documents.azure.cn",
        "_self": "",
        "_ts": 0,
        "_etag": null
    }

* 所有 PUT、POST 和 DELETE 请求必须转到指示的写入 URI
* 所有 GET 和其他只读请求（例如查询）都可以转到客户端选择的任何终结点

向只读区域发出的写入请求会失败并出现 HTTP 错误代码 403（“禁止”）。

如果在客户端初始发现阶段过后写入区域发生更改，则向先前写入区域进行的后续写入会失败并出现 HTTP 错误代码 403（“禁止”）。 然后，客户端应再次对区域列表执行 GET 以获取更新的写入区域。

本教程到此结束。 阅读 [Azure Cosmos DB 中的一致性级别](consistency-levels.md)，了解如何管理多区域复制帐户的一致性。 有关 Azure Cosmos DB 中多区域数据库复制工作原理的详细信息，请参阅[使用 Cosmos DB 多区域分配数据](distribute-data-globally.md)。

## <a name="next-steps"></a>后续步骤

在本教程中已完成以下操作：

> [!div class="checklist"]
> * 使用 Azure 门户配置多区域分发
> * 使用 SQL API 配置多区域分发

现可继续学习下一个教程，了解如何使用 Azure Cosmos DB 本地模拟器在本地开发。

> [!div class="nextstepaction"]
> [通过模拟器在本地开发](local-emulator.md)

[regions]: https://status.azure.com/status/

<!-- Update_Description: update meta properties, wording update, update link -->