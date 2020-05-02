---
title: Azure Cosmos DB API for MongoDB 中的索引编制
description: 概述了 Azure Cosmos DB 的用于 MongoDB 的 API 中的索引编制功能。
ms.service: cosmos-db
ms.subservice: cosmosdb-mongo
ms.devlang: nodejs
ms.topic: conceptual
origin.date: 04/03/2020
ms.date: 04/27/2020
author: rockboyfor
ms.author: v-yeche
ms.openlocfilehash: 5a8226b9236b97ecbb5425dde6fffc6079aa0d2d
ms.sourcegitcommit: f9c242ce5df12e1cd85471adae52530c4de4c7d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/24/2020
ms.locfileid: "82134466"
---
# <a name="indexing-using-azure-cosmos-dbs-api-for-mongodb"></a>使用 Azure Cosmos DB 的用于 MongoDB 的 API 进行索引编制

Azure Cosmos DB 的 API for MongoDB 利用 Cosmos DB 的核心索引管理功能。 本文档重点介绍如何使用 Azure Cosmos DB 的 API for MongoDB 添加索引。 你还可以阅读与所有 API 相关的 [Azure Cosmos DB 中的索引编制概述](index-overview.md)。

## <a name="indexing-for-version-36"></a>版本 3.6 的索引

系统始终会自动为 `_id` 字段编制索引，且该字段不能删除。 Azure Cosmos DB 的 API for MongoDB 对每个分片键自动强制实现 `_id` 字段的唯一性。

若要为其他字段编制索引，应该应用 MongoDB 索引管理命令。 与在 MongoDB 中一样，系统只会自动为 `_id` 字段编制索引。 此默认索引编制策略不同于 Azure Cosmos DB SQL API，后者在默认情况下会为所有字段编制索引。

要将排序应用于查询，必须对排序操作中使用的字段创建索引。

## <a name="index-types"></a>索引类型

### <a name="single-field"></a>单个字段

只能对任何单个字段创建索引。 单字段索引的排序顺序并不重要。 以下命令将对字段 `name` 创建索引：

`db.coll.createIndex({name:1})`

在适用的情况下，一个查询将利用多个单字段索引。 对于每个容器，最多可以创建 500 个单字段索引。

### <a name="compound-indexes-36"></a>复合索引 (3.6)

使用 3.6 线路协议的帐户支持复合索引。 在一个复合索引中最多可以包含 8 个字段。 与在 MongoDB 中不同，仅当查询需要一次对多个字段进行高效排序时，才应该创建复合索引。 对于包含多个不需要排序的筛选器的查询，应创建多个单字段索引，而不要创建单个复合索引。

以下命令对字段 `name` 和 `age` 创建复合索引：

`db.coll.createIndex({name:1,age:1})`

复合索引可用于一次对多个字段进行高效排序，例如：

`db.coll.find().sort({name:1,age:1})`

以上复合索引还可用于为一个对所有字段使用反向排序顺序的查询进行高效排序。 下面是一个示例：

`db.coll.find().sort({name:-1,age:-1})`

但是，复合索引中的路径顺序必须与查询完全匹配。 下面是一个需要其他复合索引的查询示例：

`db.coll.find().sort({age:1,name:1})`

### <a name="multikey-indexes"></a>多键索引

Azure Cosmos DB 创建多键索引来为数组中存储的内容编制索引。 如果为带有数组值的字段编制索引，则 Azure Cosmos DB 会自动为数组中的每个元素编制索引。

### <a name="geospatial-indexes"></a>空间索引

许多地理空间运算符可受益于地理空间索引。 Azure Cosmos DB 的 API for MongoDB 目前支持 `2dsphere` 索引。 尚不支持 `2d` 索引。

下面是对 `location` 字段创建地理空间索引的示例：

`db.coll.createIndex({ location : "2dsphere" })`

### <a name="text-indexes"></a>文本索引

目前不支持文本索引。 要对字符串运行文本搜索查询，应使用 [Azure 认知搜索](/search/search-howto-index-cosmosdb)与 Azure Cosmos DB 的集成。

## <a name="index-properties"></a>索引属性

以下操作对于遵守线路协议版本 3.6 的帐户和遵守早期线路协议版本的帐户都是常用的。 你还可以详细了解[支持的索引和已编制索引的属性](mongodb-feature-support-36.md#indexes-and-index-properties)。

### <a name="unique-indexes"></a>唯一索引

对于编制了索引的字段，[唯一索引](unique-keys.md)用于确保没有两个或两个以上的文档包含同一值。

>[!Important]
> 创建唯一索引的前提是集合为空（不含文档）。

以下命令在字段“student_id”上创建唯一索引：

```shell
globaldb:PRIMARY> db.coll.createIndex( { "student_id" : 1 }, {unique:true} )
{
        "_t" : "CreateIndexesResponse",
        "ok" : 1,
        "createdCollectionAutomatically" : false,
        "numIndexesBefore" : 1,
        "numIndexesAfter" : 4
}
```

对于分片的集合，创建唯一索引需要提供分片（分区）键。 换言之，在分片集合上的所有唯一索引都是复合索引，其中的一个字段是分区键。

以下命令使用字段 student_id 和 university 上的唯一索引创建分片集合 ```coll```（分片键为 ```university```）：

```shell
globaldb:PRIMARY> db.runCommand({shardCollection: db.coll._fullName, key: { university: "hashed"}});
{
        "_t" : "ShardCollectionResponse",
        "ok" : 1,
        "collectionsharded" : "test.coll"
}
globaldb:PRIMARY> db.coll.createIndex( { "student_id" : 1, "university" : 1 }, {unique:true})
{
        "_t" : "CreateIndexesResponse",
        "ok" : 1,
        "createdCollectionAutomatically" : false,
        "numIndexesBefore" : 3,
        "numIndexesAfter" : 4
}
```

在上面的示例中，如果省略 ```"university":1``` 子句，则会返回包含以下消息的错误：

```"cannot create unique index over {student_id : 1.0} with shard key pattern { university : 1.0 }"```

### <a name="ttl-indexes"></a>TTL 索引

若要在特定集合中启用文档过期，需创建[“TTL 索引”（生存时间索引）](../cosmos-db/time-to-live.md)。 TTL 索引是 _ts 字段上的索引，其值为“expireAfterSeconds”。

示例：

```JavaScript
globaldb:PRIMARY> db.coll.createIndex({"_ts":1}, {expireAfterSeconds: 10})
```

前一命令会导致 ```db.coll``` 集合中未在过去 10 秒内修改的任何文档被删除。

> [!NOTE]
> _ts 是特定于 Azure Cosmos DB 的字段，不可从 MongoDB 客户端访问  。 它是一个保留（系统）属性，其中包含文档上一次修改的时间戳。

## <a name="track-the-index-progress"></a>跟踪索引进度

Azure Cosmos DB 的 API for MongoDB 版本 3.6 帐户支持使用 `currentOp()` 命令来跟踪数据库实例上的索引进度。 此命令返回一个文档，其中包含有关数据库实例上正在进行的操作的信息。 `currentOp` 命令用于跟踪本机 MongoDB 中所有正在进行的操作，而在 Azure Cosmos DB 的 API for MongoDB 中，此命令仅支持跟踪索引操作。

下面这些示例演示如何使用 `currentOp` 命令来跟踪索引进度：

* 获取集合的索引进度：

    ```shell
    db.currentOp({"command.createIndexes": <collectionName>, "command.$db": <databaseName>})
    ```

* 获取数据库中所有集合的索引进度：

    ```shell
    db.currentOp({"command.$db": <databaseName>})
    ```

* 获取 Azure Cosmos 帐户中所有数据库和集合的索引进度：

    ```shell
    db.currentOp({"command.createIndexes": { $exists : true } })
    ```

索引进度详细信息包含当前索引操作的进度百分比。 以下示例显示索引进度的不同阶段的输出文档格式：

1. 如果针对“foo”集合与“bar”数据库执行了索引操作，且该操作的索引编制完成百分比为 60%，那么该操作将有以下输出文档。 `Inprog[0].progress.total` 显示目标完成进度为 100。

    ```json
    {
        "inprog" : [
        {
                ………………...
                "command" : {
                        "createIndexes" : foo
                        "indexes" :[ ],
                        "$db" : bar
                },
                "msg" : "Index Build (background) Index Build (background): 60 %",
                "progress" : {
                        "done" : 60,
                        "total" : 100
                },
                …………..…..
        }
        ],
        "ok" : 1
    }
    ```

2. 对于刚刚针对“foo”集合与“bar”数据库启动的索引操作，输出文档在进度达到可度量的级别之前，可能会一直显示 0% 进度。

    ```json
    {
        "inprog" : [
        {
                ………………...
                "command" : {
                        "createIndexes" : foo
                        "indexes" :[ ],
                        "$db" : bar
                },
                "msg" : "Index Build (background) Index Build (background): 0 %",
                "progress" : {
                        "done" : 0,
                        "total" : 100
                },
                …………..…..
        }
        ],
       "ok" : 1
    }
    ```

3. 正在进行的索引操作完成后，输出文档将显示正在进行的操作为空。

    ```json
    {
      "inprog" : [],
      "ok" : 1
    }
    ```

### <a name="background-index-updates"></a>后台索引更新

无论为 Background 索引属性指定了什么值，索引更新始终会在后台完成  。 索引更新以低于其他数据库操作的优先级消耗 RU。 因此，索引更改不会导致在执行写入、更新或删除时出现停机。

添加新索引时，查询会立即利用该索引。 这意味着，查询可能不会返回所有匹配的结果，且返回结果时不会返回任何错误。 完成索引转换后，查询结果将是一致的。 可以[跟踪索引进度](#track-the-index-progress)。

## <a name="migrating-collections-with-indexes"></a>迁移带索引的集合

目前，创建唯一索引的前提是集合不含文档。 常用 MongoDB 迁移工具会尝试在导入数据后创建唯一索引。 若要避免此问题，建议用户手动创建相应的集合和唯一索引，而不是让迁移工具来创建（对于 ```mongorestore```，可以通过在命令行中使用 `--noIndexRestore` 标志来实现此行为）。

## <a name="indexing-for-version-32"></a>版本 3.2 的索引

对于与 MongoDB 线路协议版本 3.2 兼容的 Azure Cosmos DB 帐户，可用的索引编制功能和默认值是不同的。 可以[检查帐户的版本](mongodb-feature-support-36.md#protocol-support)。 可以通过提出[支持请求](https://support.azure.cn/support/support-azure/)升级到版本 3.6。

如果你使用的是版本 3.2，请阅读此本部分，其中概述了版本 3.2 与版本 3.6 之间的重要差别。

### <a name="dropping-the-default-indexes-32"></a>删除默认索引 (3.2)

与 Azure Cosmos DB 的 API for MongoDB 版本 3.6 不同，版本 3.2 默认会为每个属性编制索引。 可以使用以下命令删除集合 ```coll``` 的这些默认索引：

```JavaScript
> db.coll.dropIndexes()
{ "_t" : "DropIndexesResponse", "ok" : 1, "nIndexesWas" : 3 }
```

删除默认索引后，可以像在版本 3.6 中那样添加更多索引。

### <a name="compound-indexes-32"></a>复合索引 (3.2)

复合索引包含对文档多个字段的引用。 若要创建复合索引，请通过提出[支持请求](https://support.azure.cn/support/support-azure/)升级到版本 3.6。

## <a name="next-steps"></a>后续步骤

* [Azure Cosmos DB 中的索引](../cosmos-db/index-policy.md)
* [利用生存时间使 Azure Cosmos DB 中的数据自动过期](../cosmos-db/time-to-live.md)

<!-- Update_Description: update meta properties, wording update, update link -->