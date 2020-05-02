---
title: 用于管理 Azure Cosmos DB API for MongoDB 中的数据的 MongoDB 扩展命令
description: 本文介绍如何使用 MongoDB 扩展命令来管理 Azure Cosmos DB's API for MongoDB 中存储的数据。
author: rockboyfor
ms.service: cosmos-db
ms.topic: conceptual
origin.date: 03/26/2019
ms.date: 04/27/2020
ms.author: v-yeche
ms.openlocfilehash: c5ae5adb5ef76b66b6b46c4e30190e9cf5a53715
ms.sourcegitcommit: f9c242ce5df12e1cd85471adae52530c4de4c7d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/24/2020
ms.locfileid: "82134529"
---
# <a name="use-mongodb-extension-commands-to-manage-data-stored-in-azure-cosmos-dbs-api-for-mongodb"></a>使用 MongoDB 扩展命令管理 Azure Cosmos DB's API for MongoDB 中存储的数据 

Azure Cosmos DB 是世纪互联提供的多区域分布式多模型数据库服务。 可通过任何开源 [MongoDB 客户端驱动程序](https://docs.mongodb.org/ecosystem/drivers)来与 Azure Cosmos DB's API for MongoDB 通信。 Azure Cosmos DB's API for MongoDB 允许按照 MongoDB [线路协议](https://docs.mongodb.org/manual/reference/mongodb-wire-protocol)使用现有的客户端驱动程序。

使用 Azure Cosmos DB's API for MongoDB，可以享用 Cosmos DB 的多种优势，例如多区域分发、自动分片、高可用性、延迟保证、自动化、静态加密、备份，等等，同时可以保护 MongoDB 应用方面的投资。

## <a name="mongodb-protocol-support"></a>MongoDB 协议支持

默认情况下，Azure Cosmos DB's API for MongoDB 与 MongoDB 服务器版本 3.2 兼容。有关详细信息，请参阅[支持的功能和语法](mongodb-feature-support.md)。 在 MongoDB 版本 3.4 中添加的功能或查询运算符目前在 Azure Cosmos DB's API for MongoDB 中以预览版提供。 针对 Azure Cosmos DB API for MongoDB 中存储的数据执行 CRUD 操作时，以下扩展命令支持特定的 Azure Cosmos DB 功能：

* [创建数据库](#create-database)
* [更新数据库](#update-database)
* [获取数据库](#get-database)
* [创建集合](#create-collection)
* [更新集合](#update-collection)
* [获取集合](#get-collection)

<a name="create-database"></a>
## <a name="create-database"></a>创建数据库

“创建数据库”扩展命令可创建新的 MongoDB 数据库。 数据库名称取自该命令所针对的数据库上下文。 CreateDatabase 命令的格式如下：

```
{
  customAction: "CreateDatabase",
  offerThroughput: <Throughput that you want to provision on the database>
}
```

下表描述了该命令中的参数：

|**字段**|**类型** |**说明** |
|---------|---------|---------|
| customAction   |  string  |   自定义命令的名称，必须是“CreateDatabase”。      |
| offerThroughput | int  | 对数据库设置的预配吞吐量。 此参数是可选的。 |

### <a name="output"></a>输出

返回默认的自定义命令响应。 有关输出中的参数，请参阅自定义命令的[默认输出](#default-output)。

### <a name="examples"></a>示例

**创建数据库**

若要创建名为“test”的数据库，请使用以下命令：

```shell
use test
db.runCommand({customAction: "CreateDatabase"});
```

**创建具有吞吐量的数据库**

若要创建名为“test”、预配吞吐量为 1000 RU 的数据库，请使用以下命令：

```shell
use test
db.runCommand({customAction: "CreateDatabase", offerThroughput: 1000 });
```

<a name="update-database"></a>
## <a name="update-database"></a>更新数据库

“更新数据库”扩展命令可更新与指定的数据库相关联的属性。 目前只能更新“offerThroughput”属性。

```
{
  customAction: "UpdateDatabase",
  offerThroughput: <New throughput that you want to provision on the database> 
}
```

下表描述了该命令中的参数：

|**字段**|**类型** |**说明** |
|---------|---------|---------|
| customAction    |    string     |   自定义命令的名称。 必须是“UpdateDatabase”。      |
|  offerThroughput   |  int       |     要对数据库设置的新预配吞吐量。    |

### <a name="output"></a>输出

返回默认的自定义命令响应。 有关输出中的参数，请参阅自定义命令的[默认输出](#default-output)。

### <a name="examples"></a>示例

**更新与数据库关联的预配吞吐量**

若要将名为“test”的数据库的预配吞吐量更新为 1200 RU，请使用以下命令：

```shell
use test
db.runCommand({customAction: "UpdateDatabase", offerThroughput: 1200 });
```

<a name="get-database"></a>
## <a name="get-database"></a>获取数据库

“获取数据库”扩展命令返回数据库对象。 数据库名称取自该命令所针对的数据库上下文。

```
{
  customAction: "GetDatabase"
}
```

下表描述了该命令中的参数：

|**字段**|**类型** |**说明** |
|---------|---------|---------|
|  customAction   |   string      |   自定义命令的名称。 必须是“GetDatabase”|

### <a name="output"></a>输出

如果该命令成功，则响应包含带有以下字段的文档：

|**字段**|**类型** |**说明** |
|---------|---------|---------|
|  `ok`   |   `int`     |   响应的状态。 1 == 成功。 0 == 失败。      |
| `database`    |    `string`        |   数据库的名称。      |
|   `provisionedThroughput`  |    `int`      |    对数据库设置的预配吞吐量。 这是一个可选的响应参数。     |

如果该命令失败，则返回默认的自定义命令响应。 有关输出中的参数，请参阅自定义命令的[默认输出](#default-output)。

### <a name="examples"></a>示例

**获取数据库**

若要获取名为“test”的数据库的数据库对象，请使用以下命令：

```shell
use test
db.runCommand({customAction: "GetDatabase"});
```

<a name="create-collection"></a>
## <a name="create-collection"></a>创建集合

“创建集合”扩展命令可创建新的 MongoDB 集合。 数据库名称取自该命令所针对的数据库上下文。 CreateCollection 命令的格式如下：

```
{
  customAction: "CreateCollection",
  collection: <Collection Name>,
  offerThroughput: <Throughput that you want to provision on the collection>,
  shardKey: <Shard key path>  
}
```

下表描述了该命令中的参数：

| **字段** | **类型** | **必需** | **说明** |
|---------|---------|---------|---------|
| customAction | string | 必须 | 自定义命令的名称。 必须是“CreateCollection”。|
| collection | string | 必须 | 集合的名称。 不允许使用特殊字符。|
| offerThroughput | int | 可选* | 要在数据库上设置的预配吞吐量。 如果未提供此参数，它将默认为最小值（400 RU/秒）。 *若要指定超过 10,000 RU/秒的吞吐量，需要使用 `shardKey` 参数。|
| shardKey | string | 可选* | 分片集合的分片键的路径。 如果在 `offerThroughput` 中设置的 RU/秒超过 10,000，则需要此参数。  如果指定了此参数，则插入的所有文档都需要此值。 |

### <a name="output"></a>输出

返回默认的自定义命令响应。 有关输出中的参数，请参阅自定义命令的[默认输出](#default-output)。

### <a name="examples"></a>示例

**创建未分片集合**

若要创建名为“testCollection”、预配吞吐量为 1000 RU 的未分片集合，请使用以下命令： 

```shell
use test
db.runCommand({customAction: "CreateCollection", collection: "testCollection", offerThroughput: 1000});
``` 

**创建分片集合**

若要创建名为“testCollection”、预配吞吐量为 1000 RU、shardkey 属性为“a.b”的分片集合，请使用以下命令：

```shell
use test
db.runCommand({customAction: "CreateCollection", collection: "testCollection", offerThroughput: 1000, shardKey: "a.b" });
```

<a name="update-collection"></a>
## <a name="update-collection"></a>更新集合

“更新集合”扩展命令可更新与指定的集合相关联的属性。

```
{
  customAction: "UpdateCollection",
  collection: <Name of the collection that you want to update>,
  offerThroughput: <New throughput that you want to provision on the collection> 
}
```

下表描述了该命令中的参数：

|**字段**|**类型** |**说明** |
|---------|---------|---------|
|  customAction   |   string      |   自定义命令的名称。 必须是“UpdateCollection”。      |
|  collection   |   string      |   集合的名称。       |
| offerThroughput   |int|   要对集合设置的预配吞吐量。|

## <a name="output"></a>输出

返回默认的自定义命令响应。 有关输出中的参数，请参阅自定义命令的[默认输出](#default-output)。

### <a name="examples"></a>示例

**更新与集合关联的预配吞吐量**

若要将名为“testCollection”的集合的预配吞吐量更新为 1200 RU，请使用以下命令：

```shell
use test
db.runCommand({customAction: "UpdateCollection", collection: "testCollection", offerThroughput: 1200 });
```

<a name="get-collection"></a>
## <a name="get-collection"></a>获取集合

“获取集合”自定义命令返回集合对象。

```
{
  customAction: "GetCollection",
  collection: <Name of the collection>
}
```

下表描述了该命令中的参数：

|**字段**|**类型** |**说明** |
|---------|---------|---------|
| customAction    |   string      |   自定义命令的名称。 必须是“GetCollection”。      |
| collection    |    string     |    集合的名称。     |

### <a name="output"></a>输出

如果该命令成功，则响应包含带有以下字段的文档

|**字段**|**类型** |**说明** |
|---------|---------|---------|
|  `ok`   |    `int`     |   响应的状态。 1 == 成功。 0 == 失败。      |
| `database`    |    `string`     |   数据库的名称。      |
| `collection`    |    `string`     |    集合的名称。     |
|  `shardKeyDefinition`   |   `document`      |  用作分片键的索引规范文档。 这是一个可选的响应参数。       |
|  `provisionedThroughput`   |   `int`      |    要对集合设置的预配吞吐量。 这是一个可选的响应参数。     |

如果该命令失败，则返回默认的自定义命令响应。 有关输出中的参数，请参阅自定义命令的[默认输出](#default-output)。

### <a name="examples"></a>示例

**获取集合**

若要获取名为“testCollection”的集合的集合对象，请使用以下命令：

```shell
use test
db.runCommand({customAction: "GetCollection", collection: "testCollection"});
```

<a name="default-output"></a>
## <a name="default-output-of-a-custom-command"></a>自定义命令的默认输出

如果未指定，则自定义响应包含带有以下字段的文档：

|**字段**|**类型** |**说明** |
|---------|---------|---------|
|  `ok`   |    `int`     |   响应的状态。 1 == 成功。 0 == 失败。      |
| `code`    |   `int`      |   仅当命令失败时才返回（即，正常 == 0）。 包含 MongoDB 错误代码。 这是一个可选的响应参数。      |
|  `errMsg`   |  `string`      |    仅当命令失败时才返回（即，正常 == 0）。 包含用户友好的错误消息。 这是一个可选的响应参数。      |

## <a name="next-steps"></a>后续步骤

接下来，可以继续学习以下 Azure Cosmos DB 概念： 

* [Azure Cosmos DB 中的索引](../cosmos-db/index-policy.md)
* [利用生存时间使 Azure Cosmos DB 中的数据自动过期](../cosmos-db/time-to-live.md)

<!-- Update_Description: update meta properties, wording update, update link -->
