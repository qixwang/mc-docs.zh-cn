---
title: Azure Cosmos DB - SQL .NET Standard API、SDK 和资源
description: 了解有关 SQL API 和 .NET SDK 的所有信息，包括发布日期、停用日期以及 Azure Cosmos DB .NET SDK 各版本之间的更改。
author: rockboyfor
ms.service: cosmos-db
ms.subservice: cosmosdb-sql
ms.devlang: dotnet
ms.topic: reference
origin.date: 05/11/2020
ms.date: 07/06/2020
ms.author: v-yeche
ms.openlocfilehash: 5fefd2cc073cd8806acddcf8579014648b4e0929
ms.sourcegitcommit: f5484e21fa7c95305af535d5a9722b5ab416683f
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/24/2020
ms.locfileid: "85320798"
---
# <a name="azure-cosmos-db-net-sdk-v3-for-sql-api-download-and-release-notes"></a>适用于 SQL API 的 Azure Cosmos DB .NET SDK v3：下载和发行说明
> [!div class="op_single_selector"]
> * [.NET SDK v3](sql-api-sdk-dotnet-standard.md)
> * [.NET SDK v2](sql-api-sdk-dotnet.md)
> * [.NET Core SDK v2](sql-api-sdk-dotnet-core.md)
> * [.NET 更改源 SDK v2](sql-api-sdk-dotnet-changefeed.md)
> * [Node.js](sql-api-sdk-node.md)
> * [Java SDK v4](sql-api-sdk-java-v4.md)
> * [Async Java SDK v2](sql-api-sdk-async-java.md)
> * [Sync Java SDK v2](sql-api-sdk-java.md)
> * [Python](sql-api-sdk-python.md)
> * [REST](https://docs.microsoft.com/rest/api/cosmos-db/)
> * [REST 资源提供程序](https://docs.microsoft.com/rest/api/cosmos-db-resource-provider/)
> * [SQL](sql-api-query-reference.md)
> * [批量执行工具 - .NET v2](sql-api-sdk-bulk-executor-dot-net.md)
> * [批量执行程序 - Java](sql-api-sdk-bulk-executor-java.md)

| |  |
|---|---|
|**SDK 下载**|[NuGet](https://www.nuget.org/packages/Microsoft.Azure.Cosmos/)|
|**API 文档**|[ 参考文档](https://docs.azure.cn/dotnet/api/overview/cosmosdb?view=azure-dotnet)|
|**示例**|[.NET代码示例](https://github.com/Azure/azure-cosmos-dotnet-v3/tree/master/Microsoft.Azure.Cosmos.Samples/Usage)|
|**入门**|[Azure Cosmos DB .NET SDK 入门](sql-api-get-started.md)|
|**Web 应用教程**|[使用 Azure Cosmos DB 开发 Web 应用程序](sql-api-dotnet-application.md)|
|**当前受支持的框架**|[Microsoft .NET Standard 2.0](https://docs.microsoft.com//dotnet/standard/net-standard)|

预览功能被视为单独的分支，在准备就绪之前不会包含在正式版本中。 每个预览版本都列出了所有已启用的附加功能。

该格式基于[保留 Changelog](https://keepachangelog.com/en/1.0.0/)，而此项目遵循[语义化版本控制](https://semver.org/spec/v2.0.0.html)。

## <a name="unreleased"></a>未发布

### <a name="added"></a>已添加

- [#1578](https://github.com/Azure/azure-cosmos-dotnet-v3/pull/1578) 查询：添加优化以访问流缓冲区

### <a name="fixed"></a>已修复

- [#1578](https://github.com/Azure/azure-cosmos-dotnet-v3/pull/1578) ApplicationRegion：修复 ApplicationRegion 以确保故障转移方案使用正确的顺序

<a name="3.9.1"></a>
## <a name="391---2020-05-19"></a>[3.9.1](https://www.nuget.org/packages/Microsoft.Azure.Cosmos/3.9.1) - 2020-05-19
<a name="3.9.1-preview"></a>
## <a name="391-preview---2020-05-19"></a>[3.9.1-preview](https://www.nuget.org/packages/Microsoft.Azure.Cosmos/3.9.1-preview) - 2020-05-19

### <a name="fixed"></a>已修复
- [#1539](https://github.com/Azure/azure-cosmos-dotnet-v3/pull/1539) CosmosException 和诊断：修复 ToString()，使其不因重试而呈指数增长。 通过 PR [#1189](https://github.com/Azure/azure-cosmos-dotnet-v3/pull/1189) 在 3.7.0 中引入。

<a name="3.9.0"></a>
## <a name="390---2020-05-18"></a>[3.9.0](https://www.nuget.org/packages/Microsoft.Azure.Cosmos/3.9.0) - 2020-05-18

### <a name="added"></a>已添加

- [#1356](https://github.com/Azure/azure-cosmos-dotnet-v3/pull/1356) & [#1407](https://github.com/Azure/azure-cosmos-dotnet-v3/pull/1407) & [#1428](https://github.com/Azure/azure-cosmos-dotnet-v3/pull/1428) & [#1407](https://github.com/Azure/azure-cosmos-dotnet-v3/pull/1407) 自动缩放：添加支持
- [#1398](https://github.com/Azure/azure-cosmos-dotnet-v3/pull/1398) 诊断：为 .NET Core 添加 CPU 监视
- [#1441](https://github.com/Azure/azure-cosmos-dotnet-v3/pull/1441) 传输：在 CosmosClientOptions 上添加 HttpClientFactory 支持
- [#1457](https://github.com/Azure/azure-cosmos-dotnet-v3/pull/1457) 容器：向容器添加数据库引用
- [#1455](https://github.com/Azure/azure-cosmos-dotnet-v3/pull/1454) 序列化程序：向 Client.ClientOptions.Serializer 添加 SDK 序列化程序
- [#1397](https://github.com/Azure/azure-cosmos-dotnet-v3/pull/1397) CosmosClientBuilder：添加首选区域和 WithConnectionModeDirect()
- [#1439](https://github.com/Azure/azure-cosmos-dotnet-v3/pull/1439) 响应中无内容：增加了让操作不从 Cosmos DB 返回内容的功能。 
- [#1398](https://github.com/Azure/azure-cosmos-dotnet-v3/pull/1398) & [#1516](https://github.com/Azure/azure-cosmos-dotnet-v3/pull/1516) 读取源和更改源：增加了序列化优化，将内存和 CPU 使用率降低高达 90%。 对象现在作为数组传递给序列化程序。 
- [#1516](https://github.com/Azure/azure-cosmos-dotnet-v3/pull/1516) 查询：增加了序列化优化，将内存使用率降低高达 50%，将 CPU 使用率降低高达 25%。 对象现在作为数组传递给序列化程序。 

### <a name="fixed"></a>已修复

- [#1401](https://github.com/Azure/azure-cosmos-dotnet-v3/pull/1401) & [#1437](https://github.com/Azure/azure-cosmos-dotnet-v3/pull/1437)：响应类型：修复使用 Response.Container 时在包含 SynchronizationContext 的场景中出现的死锁
- [#1445](https://github.com/Azure/azure-cosmos-dotnet-v3/pull/1445) 传输：修复 WebAssembly 的 ServicePoint
- [#1462](https://github.com/Azure/azure-cosmos-dotnet-v3/pull/1462) UserAgent：修复功能使用情况跟踪
- [#1469](https://github.com/Azure/azure-cosmos-dotnet-v3/pull/1469) 诊断：修复 InvalidOperationException 并将运行时间转换为毫秒
- [#1512](https://github.com/Azure/azure-cosmos-dotnet-v3/pull/1512) PartitionRoutingHelper：修复由于容器缓存未命中导致的 ReadFeed ArgumentNullException
- [#1530](https://github.com/Azure/azure-cosmos-dotnet-v3/pull/1530) CosmosClientBuilder：修复 WithDefaultTimeToLive 参数拼写

<a name="3.9.0-preview3"></a>
## <a name="390-preview3---2020-05-11"></a>[3.9.0-preview3](https://www.nuget.org/packages/Microsoft.Azure.Cosmos/3.9.0-preview3) - 2020-05-11

### <a name="added"></a>已添加

- [#1356](https://github.com/Azure/azure-cosmos-dotnet-v3/pull/1356) & [#1407](https://github.com/Azure/azure-cosmos-dotnet-v3/pull/1407) & [#1428](https://github.com/Azure/azure-cosmos-dotnet-v3/pull/1428) 自动缩放预览版：添加到公开预览版
- [#1407](https://github.com/Azure/azure-cosmos-dotnet-v3/pull/1407) 自动缩放：添加 CreateDatabaseIfNotExistsAsync 和 CreateContainerIfNotExistsAsync
- [#1410](https://github.com/Azure/azure-cosmos-dotnet-v3/pull/1410) FeedRange：添加 Json 序列化支持
- [#1398](https://github.com/Azure/azure-cosmos-dotnet-v3/pull/1398) 诊断：为 .NET Core 添加 CPU 监视
- [#1441](https://github.com/Azure/azure-cosmos-dotnet-v3/pull/1441) 传输：在 CosmosClientOptions 上添加 HttpClientFactory 支持
- [#1457](https://github.com/Azure/azure-cosmos-dotnet-v3/pull/1457) 容器：向容器添加数据库引用
- [#1453](https://github.com/Azure/azure-cosmos-dotnet-v3/pull/1453) 响应工厂：将响应工厂添加到公共 API
- [#1455](https://github.com/Azure/azure-cosmos-dotnet-v3/pull/1454) 序列化程序：向 Client.ClientOptions.Serializer 添加 SDK 序列化程序
- [#1397](https://github.com/Azure/azure-cosmos-dotnet-v3/pull/1397) CosmosClientBuilder：添加首选区域和公共内部函数 WithConnectionModeDirect()
- [#1439](https://github.com/Azure/azure-cosmos-dotnet-v3/pull/1439) 响应中无内容：增加了让操作不从 Cosmos DB 返回内容的功能
- [#1469](https://github.com/Azure/azure-cosmos-dotnet-v3/pull/1469) 诊断：修复 InvalidOperationException 并将运行时间转换为毫秒

### <a name="fixed"></a>已修复

- [#1398](https://github.com/Azure/azure-cosmos-dotnet-v3/pull/1398) 降低了查询反序列化的内存分配。
- [#1401](https://github.com/Azure/azure-cosmos-dotnet-v3/pull/1401) & [#1437](https://github.com/Azure/azure-cosmos-dotnet-v3/pull/1437)：响应类型：修复使用 Response.Container 时在包含 SynchronizationContext 的场景中出现的死锁
- [#1445](https://github.com/Azure/azure-cosmos-dotnet-v3/pull/1445) 传输：修复 WebAssembly 的 ServicePoint
- [#1462](https://github.com/Azure/azure-cosmos-dotnet-v3/pull/1462) UserAgent：修复功能使用情况跟踪

<a name="3.9.0-preview"></a>
## <a name="390-preview---2020-04-17"></a>[3.9.0-preview](https://www.nuget.org/packages/Microsoft.Azure.Cosmos/3.9.0-preview) - 2020-04-17

### <a name="added"></a>已添加

- [#1356](https://github.com/Azure/azure-cosmos-dotnet-v3/pull/1356) 自动缩放预览版：添加到公开预览版

<a name="3.8.0-preview"></a>
## <a name="380-preview---2020-04-16"></a>[3.8.0-preview](https://www.nuget.org/packages/Microsoft.Azure.Cosmos/3.8.0-preview) - 2020-04-16

### <a name="added"></a>已添加

- [#1331](https://github.com/Azure/azure-cosmos-dotnet-v3/pull/1331) 为事务性批处理请求启用客户端加密/解密

### <a name="fixed"></a>已修复

- [#1369](https://github.com/Azure/azure-cosmos-dotnet-v3/pull/1369) 修复“order by”查询结果的解密

<a name="3.8.0"></a>
## <a name="380---2020-04-07"></a>[3.8.0](https://www.nuget.org/packages/Microsoft.Azure.Cosmos/3.8.0) - 2020-04-07

### <a name="added"></a>已添加

- [#1314](https://github.com/Azure/azure-cosmos-dotnet-v3/pull/1314) 添加了用于主动 TCP 连接端检测的配置
- [#1305](https://github.com/Azure/azure-cosmos-dotnet-v3/pull/1305) 添加了对首选区域自定义的支持

### <a name="fixed"></a>已修复

- [#1312](https://github.com/Azure/azure-cosmos-dotnet-v3/pull/1312) 修复了使用 default(PartitionKey) 时的空引用
- [#1296](https://github.com/Azure/azure-cosmos-dotnet-v3/pull/1296) 在返回查询结果之前解密已加密的属性
- [#1345](https://github.com/Azure/azure-cosmos-dotnet-v3/pull/1345) 修复了获取查询计划诊断

<a name="3.7.1-preview"></a>
## <a name="371-preview---2020-03-30"></a>[3.7.1-preview](https://www.nuget.org/packages/Microsoft.Azure.Cosmos/3.7.1-preview) - 2020-03-30

- [#1210](https://github.com/Azure/azure-cosmos-dotnet-v3/pull/1210) 更改源拉取模型
- [#1242](https://github.com/Azure/azure-cosmos-dotnet-v3/pull/1242) 客户端加密 - 修复不含已加密属性的读取路径中的 bug
- [#1314](https://github.com/Azure/azure-cosmos-dotnet-v3/pull/1314) 添加了用于主动 TCP 连接端检测的配置
- [#1312](https://github.com/Azure/azure-cosmos-dotnet-v3/pull/1312) 修复了使用 default(PartitionKey) 时的空引用
- [#1296](https://github.com/Azure/azure-cosmos-dotnet-v3/pull/1296) 在返回查询结果之前解密已加密的属性

<a name="3.7.0"></a>
## <a name="370---2020-03-26"></a>[3.7.0](https://www.nuget.org/packages/Microsoft.Azure.Cosmos/3.7.0) - 2020-03-26

### <a name="added"></a>已添加
- [#1268](https://github.com/Azure/azure-cosmos-dotnet-v3/pull/1268) 将 GetElapsedClientLatency 添加到 CosmosDiagnostics
- [#1239](https://github.com/Azure/azure-cosmos-dotnet-v3/pull/1239) 使 MultiPolygon 和 PolygonCoordinates 类成为公共类。
- [#1233](https://github.com/Azure/azure-cosmos-dotnet-v3/pull/1265) PartitionKey 现在支持使用运算符 == 和 != 进行等式比较。
- [#1285](https://github.com/Azure/azure-cosmos-dotnet-v3/pull/1285) 将查询计划检索添加到诊断
- [#1289](https://github.com/Azure/azure-cosmos-dotnet-v3/pull/1289) 查询 ORDER BY Resume 优化
- [#1074](https://github.com/Azure/azure-cosmos-dotnet-v3/pull/1074) 批量 API 拥塞控制

### <a name="fixed"></a>已修复

- [#1213](https://github.com/Azure/azure-cosmos-dotnet-v3/pull/1213) CosmosException 现在返回原始堆栈跟踪。
- [#1213](https://github.com/Azure/azure-cosmos-dotnet-v3/pull/1213) ResponseMessage.ErrorMessage 现在始终会正确填充。 过去存在一个 bug，在某些场景下，错误消息会留在内容流中。
- [#1298](https://github.com/Azure/azure-cosmos-dotnet-v3/pull/1298) CosmosException.Message 包含与 CosmosException.ToString() 相同的信息以确保跟踪所有信息
- [#1242](https://github.com/Azure/azure-cosmos-dotnet-v3/pull/1242) 客户端加密 - 修复不含已加密属性的读取路径中的 bug
- [#1189](https://github.com/Azure/azure-cosmos-dotnet-v3/pull/1189) 查询诊断显示正确的总体时间。
- [#1189](https://github.com/Azure/azure-cosmos-dotnet-v3/pull/1189) 修复了在诊断上下文中导致重复信息的一个 bug。
- [#1263](https://github.com/Azure/azure-cosmos-dotnet-v3/pull/1263) 修复以下 bug：未在查询流响应上设置间隔后重试
- [#1198](https://github.com/Azure/azure-cosmos-dotnet-v3/pull/1198) 修复当调用已处理的 CosmosClient 时出现的空引用异常
- [#1274](https://github.com/Azure/azure-cosmos-dotnet-v3/pull/1274) 调用引用已处理客户端的所有 SDK 对象（例如数据库和容器）时引发 ObjectDisposedException 
- [#1268](https://github.com/Azure/azure-cosmos-dotnet-v3/pull/1268) 修复以下 bug：Database.ReadStreamAsync 和 Database.DeleteStreamAsync 的请求选项丢失
- [#1304](https://github.com/Azure/azure-cosmos-dotnet-v3/pull/1304) 修复了 XML 文档，使其现在可在 Visual Studio 中显示

<a name="3.7.0-preview2"></a>
## <a name="370-preview2---2020-03-09"></a>[3.7.0-preview2](https://www.nuget.org/packages/Microsoft.Azure.Cosmos/3.7.0-preview2) - 2020-03-09

- [#1210](https://github.com/Azure/azure-cosmos-dotnet-v3/pull/1210) 更改源拉取模型
- [#1242](https://github.com/Azure/azure-cosmos-dotnet-v3/pull/1242) 客户端加密 - 修复不含已加密属性的读取路径中的 bug

<a name="3.7.0-preview"></a>
## <a name="370-preview---2020-02-25"></a>[3.7.0-preview](https://www.nuget.org/packages/Microsoft.Azure.Cosmos/3.7.0-preview) - 2020-02-25

- [#1074](https://github.com/Azure/azure-cosmos-dotnet-v3/pull/1074) 批量 API 拥塞控制
- [#1210](https://github.com/Azure/azure-cosmos-dotnet-v3/pull/1210) 更改源拉取模型

<a name="3.6.0"></a>
## <a name="360---2020-01-23"></a>[3.6.0](https://www.nuget.org/packages/Microsoft.Azure.Cosmos/3.6.0) - 2020-01-23

- [#1105](https://github.com/Azure/azure-cosmos-dotnet-v3/pull/1198) CosmosClient 不变性 + 可处理项修复

### <a name="added"></a>已添加

- [#1097](https://github.com/Azure/azure-cosmos-dotnet-v3/pull/1097) 将 GeospatialConfig 添加到 ContainerProperties，将 BoundingBoxProperties 添加到 SpatialPath
- [#1061](https://github.com/Azure/azure-cosmos-dotnet-v3/pull/1061) 将流有效负载添加到 ExecuteStoredProcedureStreamAsync
- [#1062](https://github.com/Azure/azure-cosmos-dotnet-v3/pull/1062) 添加更多诊断信息，包括通过不同的 SDK 层跟踪时间的功能
- [#1107](https://github.com/Azure/azure-cosmos-dotnet-v3/pull/1107) 添加源链接支持
- [#1121](https://github.com/Azure/azure-cosmos-dotnet-v3/pull/1121) StandByFeedIterator 广度优先读取策略

### <a name="fixed"></a>已修复

- [#1105](https://github.com/Azure/azure-cosmos-dotnet-v3/pull/1105) 自定义序列化程序不再调用 SDK 拥有的会导致序列化异常的类型
- [#1112](https://github.com/Azure/azure-cosmos-dotnet-v3/pull/1112) 修复了 SDK 属性（例如 DatabaseProperties）以使其具有相同的 JSON 特性
- [#1116](https://github.com/Azure/azure-cosmos-dotnet-v3/pull/1116) 修复了执行异步查询操作时在包含 SynchronizationContext 的场景中出现的死锁
- [#1143](https://github.com/Azure/azure-cosmos-dotnet-v3/pull/1143) 修复了通过资源令牌针对特定分区键执行查询时出现的权限资源链接和授权问题
- [#1150](https://github.com/Azure/azure-cosmos-dotnet-v3/pull/1150) 修复了使用不存在的租用容器时出现的 NullReferenceException。

<a name="3.5.1"></a>
## <a name="351---2019-12-11"></a>[3.5.1](https://www.nuget.org/packages/Microsoft.Azure.Cosmos/3.5.1) - 2019-12-11

### <a name="fixed"></a>已修复

- [#1060](https://github.com/Azure/azure-cosmos-dotnet-v3/pull/1060) 修复了 DISTINCT 查询中的 Unicode 编码 bug。
- [#1070](https://github.com/Azure/azure-cosmos-dotnet-v3/pull/1070) 在重新创建集合的情况下，CreateItem 将仅针对自动提取的分区键进行重试
- [#1075](https://github.com/Azure/azure-cosmos-dotnet-v3/pull/1075) 为具有大型标头的 BadRequest 包括标头大小详细信息
- [#1078](https://github.com/Azure/azure-cosmos-dotnet-v3/pull/1078) 修复了执行异步 SDK API 时在包含 SynchronizationContext 的场景中出现的死锁
- [#1081](https://github.com/Azure/azure-cosmos-dotnet-v3/pull/1081) 修复了序列化程序中导致空引用异常的争用条件。
- [#1086](https://github.com/Azure/azure-cosmos-dotnet-v3/pull/1086) 修复 TransactionalBatch 代码路径上的可能 NullReferenceException
- [#1091](https://github.com/Azure/azure-cosmos-dotnet-v3/pull/1091) 修复了进行分区拆分时查询中会引发 NotImplementedException 的一个 bug。
- [#1089](https://github.com/Azure/azure-cosmos-dotnet-v3/pull/1089) 修复了对没有主键的项使用批量处理时出现的 NullReferenceException

<a name="3.5.0"></a>
## <a name="350---2019-12-03"></a>[3.5.0](https://www.nuget.org/packages/Microsoft.Azure.Cosmos/3.5.0) - 2019-12-03

### <a name="added"></a>已添加

- [#979](https://github.com/Azure/azure-cosmos-dotnet-v3/pull/979) 在 QueryRequestOptions 中使 SessionToken 成为公共项。
- [#995](https://github.com/Azure/azure-cosmos-dotnet-v3/pull/995) 在诊断中包括了会话令牌。
- [#1000](https://github.com/Azure/azure-cosmos-dotnet-v3/pull/1000) 向 CosmosClientOptions 中添加了 PortReuseMode。
- [#1017](https://github.com/Azure/azure-cosmos-dotnet-v3/pull/1017) 向网关调用添加 ClientSideRequestStatistics 并使 endtime 可为空
- [#1038](https://github.com/Azure/azure-cosmos-dotnet-v3/pull/1038) 向资源属性中添加 Selflink

### <a name="fixed"></a>已修复

- [#921](https://github.com/Azure/azure-cosmos-dotnet-v3/pull/921) 修复了在某些情况下保留堆栈跟踪所需的错误处理
- [#944](https://github.com/Azure/azure-cosmos-dotnet-v3/pull/944) 对于内部操作，更改源处理器不使用用户序列化程序
- [#988](https://github.com/Azure/azure-cosmos-dotnet-v3/pull/988) 修复了由于重试用完/名称缓存过时而导致的查询变异。
- [#954](https://github.com/Azure/azure-cosmos-dotnet-v3/pull/954) 在多主帐户中，支持对更改源处理器使用“从头开始”功能
- [#999](https://github.com/Azure/azure-cosmos-dotnet-v3/pull/999) 修复了抓取额外页面的问题，更新了异常路径上的继续标记，并修复了 order by 继续标记中的非 ASCII 字符。
- [#1013](https://github.com/Azure/azure-cosmos-dotnet-v3/pull/1013) 网关 OperationCanceledException 现在以请求超时的形式返回
- [#1020](https://github.com/Azure/azure-cosmos-dotnet-v3/pull/1020) 直接包更新删除了调试语句
- [#1023](https://github.com/Azure/azure-cosmos-dotnet-v3/pull/1023) 修复了 ThroughputResponse.IsReplacePending 标头映射
- [#1036](https://github.com/Azure/azure-cosmos-dotnet-v3/pull/1036) 修复了查询响应，以在失败时返回空内容
- [#1045](https://github.com/Azure/azure-cosmos-dotnet-v3/pull/1045) 向 CosmosException 添加了堆栈跟踪和内部异常
- [#1050](https://github.com/Azure/azure-cosmos-dotnet-v3/pull/1050) 向 TransactionalBatchOperationResult 中添加模拟构造函数

<a name="3.4.1"></a>
## <a name="341---2019-11-06"></a>[3.4.1](https://www.nuget.org/packages/Microsoft.Azure.Cosmos/3.4.1) - 2019-11-06

### <a name="fixed"></a>已修复

- [#978](https://github.com/Azure/azure-cosmos-dotnet-v3/pull/978) 针对 FeedIterator 和 Response 类修复了模拟

<a name="3.4.0"></a>
## <a name="340---2019-11-04"></a>[3.4.0](https://www.nuget.org/packages/Microsoft.Azure.Cosmos/3.4.0) - 2019-11-04

### <a name="added"></a>已添加

- [#853](https://github.com/Azure/azure-cosmos-dotnet-v3/pull/853) ORDER BY 数组和对象支持。
- [#877](https://github.com/Azure/azure-cosmos-dotnet-v3/pull/877) 查询诊断现在包含客户端请求诊断信息
- [#923](https://github.com/Azure/azure-cosmos-dotnet-v3/pull/923) 批量操作支持现在为公共版
- [#922](https://github.com/Azure/azure-cosmos-dotnet-v3/pull/922) 在用户代理中包括了批量操作支持使用情况信息
- [#934](https://github.com/Azure/azure-cosmos-dotnet-v3/pull/934) 在 GROUP BY 查询中保留了投影的顺序。
- [#952](https://github.com/Azure/azure-cosmos-dotnet-v3/pull/952) ORDER BY 未定义和混合类型的 ORDER BY 支持
- [#965](https://github.com/Azure/azure-cosmos-dotnet-v3/pull/965) Batch API 现在为公共版

### <a name="fixed"></a>已修复
- [#901](https://github.com/Azure/azure-cosmos-dotnet-v3/pull/901) 修复了导致查询响应为每个内容调用创建新流的一个 bug
- [#918](https://github.com/Azure/azure-cosmos-dotnet-v3/pull/918) 修复了用于与脚本、权限和冲突相关的迭代器的序列化程序
- [#936](https://github.com/Azure/azure-cosmos-dotnet-v3/pull/936) 修复了包含大型资源的批量请求以使异常自然化

<a name="3.3.3"></a>
## <a name="333---2019-10-30"></a>[3.3.3](https://www.nuget.org/packages/Microsoft.Azure.Cosmos/3.3.3) - 2019-10-30

- [#837](https://github.com/Azure/azure-cosmos-dotnet-v3/pull/837) 修复了非 Windows 平台的分组依据 bug
- [#927](https://github.com/Azure/azure-cosmos-dotnet-v3/pull/927) 修复了返回部分结果而非错误的查询

<a name="3.3.2"></a>
## <a name="332---2019-10-16"></a>[3.3.2](https://www.nuget.org/packages/Microsoft.Azure.Cosmos/3.3.2) - 2019-10-16

### <a name="fixed"></a>已修复

- [#905](https://github.com/Azure/azure-cosmos-dotnet-v3/pull/909) 修复了 LINQ 驼峰式大小写 bug

<a name="3.3.1"></a>
## <a name="331---2019-10-11"></a>[3.3.1](https://www.nuget.org/packages/Microsoft.Azure.Cosmos/3.3.1) - 2019-10-11

### <a name="fixed"></a>已修复

- [#895](https://github.com/Azure/azure-cosmos-dotnet-v3/pull/895) 修复了在非 Windows 平台上导致格式异常的用户代理 bug

<a name="3.3.0"></a>
## <a name="330---2019-10-09"></a>[3.3.0](https://www.nuget.org/packages/Microsoft.Azure.Cosmos/3.3.0) - 2019-10-09

### <a name="added"></a>已添加

- [#801](https://github.com/Azure/azure-cosmos-dotnet-v3/pull/801) 启用了 OrderBy 后的 LINQ ThenBy 运算符
- [#814](https://github.com/Azure/azure-cosmos-dotnet-v3/pull/814) 仅限使用已配置终结点的功能
- [#822](https://github.com/Azure/azure-cosmos-dotnet-v3/pull/822) GROUP BY 查询支持。
- [#844](https://github.com/Azure/azure-cosmos-dotnet-v3/pull/844) 向容器生成器添加了 PartitionKeyDefinitionVersion

### <a name="fixed"></a>已修复

- [#835](https://github.com/Azure/azure-cosmos-dotnet-v3/pull/835) 修复了导致 sortedRanges 异常的一个 bug
- [#846](https://github.com/Azure/azure-cosmos-dotnet-v3/pull/846) 统计信息在 CosmosException 中未正确填充。
- [#857](https://github.com/Azure/azure-cosmos-dotnet-v3/pull/857) 修复了批量操作支持在容器实例之间的可重用性
- [#860](https://github.com/Azure/azure-cosmos-dotnet-v3/pull/860) 修复了基础用户代理字符串
- [#876](https://github.com/Azure/azure-cosmos-dotnet-v3/pull/876) 默认连接超时从 60 秒减少到了 10 秒

<a name="3.2.0"></a>
## <a name="320---2019-09-17"></a>[3.2.0](https://www.nuget.org/packages/Microsoft.Azure.Cosmos/3.2.0) - 2019-09-17

### <a name="added"></a>已添加

- [#100](https://github.com/Azure/azure-cosmos-dotnet-v3/pull/100) CosmosClientOptions 的可配置 Tcp 设置
- [#615](https://github.com/Azure/azure-cosmos-dotnet-v3/pull/615)、[#775](https://github.com/Azure/azure-cosmos-dotnet-v3/pull/775) 向响应中添加了请求诊断
- [#622](https://github.com/Azure/azure-cosmos-dotnet-v3/pull/622) 添加了针对用户和权限的 CRUD 和查询操作，这启用了 [ResourceToken](/cosmos-db/secure-access-to-data#resource-tokens) 支持
- [#716](https://github.com/Azure/azure-cosmos-dotnet-v3/pull/716) 添加了在 LINQ 查询生成时会启用的驼峰式大小写序列化
- [#729](https://github.com/Azure/azure-cosmos-dotnet-v3/pull/729)、[#776](https://github.com/Azure/azure-cosmos-dotnet-v3/pull/776) 针对 LINQ 查询添加了 aggregate(CountAsync/SumAsync 等) 扩展
- [#743](https://github.com/Azure/azure-cosmos-dotnet-v3/pull/743) 向 CosmosClientOptions 中添加了 WebProxy

### <a name="fixed"></a>已修复

- [#726](https://github.com/Azure/azure-cosmos-dotnet-v3/pull/726) 当遇到异常时，查询迭代器 HasMoreResults 现在返回 false
- [#705](https://github.com/Azure/azure-cosmos-dotnet-v3/pull/705) 用户代理后缀被截断
- [#753](https://github.com/Azure/azure-cosmos-dotnet-v3/pull/753) 冲突异常的原因未传播
- [#756](https://github.com/Azure/azure-cosmos-dotnet-v3/pull/756) 带 WithStartTime 的更改源处理器在第一次执行委托时将不使用任何项。
- [#761](https://github.com/Azure/azure-cosmos-dotnet-v3/pull/761) 由于 jkonecki，当使用自定义任务计划程序（例如 Orleans）时，CosmosClient 发生死锁
- [#769](https://github.com/Azure/azure-cosmos-dotnet-v3/pull/769) 会话一致性 + 网关模式会话令牌 bug 修复：在极少数不成功的情况下，会话令牌可能不正确
- [#772](https://github.com/Azure/azure-cosmos-dotnet-v3/pull/772) 修复了在使用了自定义序列化程序或产品/服务不存在的情况下引发的吞吐量异常
- [#785](https://github.com/Azure/azure-cosmos-dotnet-v3/pull/785) 密钥错误引发 CosmosExceptions 且带有 HttpStatusCode.Unauthorized 状态代码

<a name="3.2.0-preview2"></a>
## <a name="320-preview2---2019-09-10"></a>[3.2.0-preview2](https://www.nuget.org/packages/Microsoft.Azure.Cosmos/3.2.0-preview2) - 2019-09-10

- [#585](https://github.com/Azure/azure-cosmos-dotnet-v3/pull/585)、[#741](https://github.com/Azure/azure-cosmos-dotnet-v3/pull/741) 批量执行支持
- [#427](https://github.com/Azure/azure-cosmos-dotnet-v3/pull/427) 事务性批处理支持（项 CRUD）

<a name="3.2.0-preview"></a>
## <a name="320-preview---2019-08-09"></a>[3.2.0-preview](https://www.nuget.org/packages/Microsoft.Azure.Cosmos/3.2.0-preview) - 2019-08-09

- [#427](https://github.com/Azure/azure-cosmos-dotnet-v3/pull/427) 事务性批处理支持（项 CRUD）

<a name="3.1.1"></a>
## <a name="311---2019-08-12"></a>[3.1.1](https://www.nuget.org/packages/Microsoft.Azure.Cosmos/3.1.1) - 2019-08-12

### <a name="added"></a>已添加

- [#650](https://github.com/Azure/azure-cosmos-dotnet-v3/pull/650) 添加了 CosmosSerializerOptions，用于自定义序列化

### <a name="fixed"></a>已修复

- [#612](https://github.com/Azure/azure-cosmos-dotnet-v3/pull/612) 修复了具有分区键的 ReadFeed 的 bug
- [#614](https://github.com/Azure/azure-cosmos-dotnet-v3/pull/614) 修复了 SpatialPath 序列化以及与较旧索引版本的兼容性
- [#619](https://github.com/Azure/azure-cosmos-dotnet-v3/pull/619) 修复了 .NET Framework 的 PInvokeStackImbalance 异常
- [#626](https://github.com/Azure/azure-cosmos-dotnet-v3/pull/626) 对于成功的情况，FeedResponse<T> 状态代码现在返回 OK 而非无效的状态代码 0 或 Accepted
- [#629](https://github.com/Azure/azure-cosmos-dotnet-v3/pull/629) 已将 CreateContainerIfNotExistsAsync 验证修复为仅限 partitionKeyPath
- [#630](https://github.com/Azure/azure-cosmos-dotnet-v3/pull/630) 修复了用户代理以包含环境和包信息

<a name="3.1.0"></a>
## <a name="310---2019-07-29---unlisted"></a>3.1.0 - 2019-07-29 - 未列出

### <a name="added"></a>已添加

- [#541](https://github.com/Azure/azure-cosmos-dotnet-v3/pull/541) 向客户端和查询选项添加了一致性级别
- [#544](https://github.com/Azure/azure-cosmos-dotnet-v3/pull/544) 为 LINQ 添加了继续标记支持
- [#557](https://github.com/Azure/azure-cosmos-dotnet-v3/pull/557) 向项请求选项添加了触发器选项
- [#572](https://github.com/Azure/azure-cosmos-dotnet-v3/pull/572) 在 CreateContainerIfNotExistsAsync 上添加了分区键验证
- [#581](https://github.com/Azure/azure-cosmos-dotnet-v3/pull/581) 向 QueryDefinition API 添加了 LINQ
- [#592](https://github.com/Azure/azure-cosmos-dotnet-v3/pull/592) 向容器生成器添加了 CreateIfNotExistsAsync
- [#597](https://github.com/Azure/azure-cosmos-dotnet-v3/pull/597) 向 ResponseMessage 添加了继续标记属性
- [#604](https://github.com/Azure/azure-cosmos-dotnet-v3/pull/604) 添加了 LINQ ToStreamIterator 扩展方法

### <a name="fixed"></a>已修复

- [#548](https://github.com/Azure/azure-cosmos-dotnet-v3/pull/548) 修复了 CosmosException.ToString() 中错误键入的消息；
- [#558](https://github.com/Azure/azure-cosmos-dotnet-v3/pull/558) LocationCache ConcurrentDict 锁争用修复
- [#561](https://github.com/Azure/azure-cosmos-dotnet-v3/pull/561) GetItemLinqQueryable 现在可以处理空查询
- [#567](https://github.com/Azure/azure-cosmos-dotnet-v3/pull/567) 查询正确处理不同的语言区域性
- [#574](https://github.com/Azure/azure-cosmos-dotnet-v3/pull/574) 修复了查询分析因意外异常而失败时的空错误消息
- [#576](https://github.com/Azure/azure-cosmos-dotnet-v3/pull/576) 查询将输入正确地序列化为流

<a name="3.0.0"></a>
## <a name="300---2019-07-15"></a>[3.0.0](https://www.nuget.org/packages/Microsoft.Azure.Cosmos/3.0.0) - 2019-07-15

- .NET SDK [版本 3.0.0](https://www.nuget.org/packages/Microsoft.Azure.Cosmos/) 正式发布
- 适用于 .NET Standard 2.0，此版本支持 .NET Framework 4.6.1+ 和 .NET Core 2.0+
- 新对象模型，顶级的 CosmosClient 和方法在相关的数据库和容器类之间进行拆分
- 新的高性能流 API
- 对更改源处理器 API 提供了内置支持
- 适用于 CosmosClient、容器和更改源处理器的 Fluent 生成器 API
- 惯用的吞吐量管理 API
- 适用于数据库、容器、项、查询和吞吐量请求的粒度化 RequestOptions 和 ResponseTypes
- 缩放未分区容器的功能
- 可扩展和可自定义的序列化程序
- 支持自定义处理程序的可扩展请求管道

## <a name="release--retirement-dates"></a>发布和停用日期
Azure 会在停用 SDK 时至少提前 12 个月发出通知，以便用户顺利转换为更高版本/受支持版本。

新特性和功能以及优化仅添加到当前 SDK，因此建议始终尽早升级到最新的 SDK 版本。

使用已停用的 SDK 对 Azure Cosmos DB 发出的任何请求都会遭服务拒绝。

<br/>

| 版本 | 发布日期 | 停用日期 |
| --- | --- | --- |
| [3.6.0](#3.6.0) |2020 年 1 月 23 日 |--- |
| [3.5.1](#3.5.1) |2019 年 12 月 11 日 |--- |
| [3.5.0](#3.5.0) |2019 年 12 月 3 日 |--- |
| [3.4.1](#3.4.1) |2019 年 11 月 6 日 |--- |
| [3.4.0](#3.4.0) |2019 年 11 月 4 日 |--- |
| [3.3.3](#3.3.3) |2019 年 10 月 30 日 |--- |
| [3.3.2](#3.3.2) |2019 年 10 月 16 日 |--- |
| [3.3.1](#3.3.1) |2019 年 11 月 8 日 |--- |
| [3.3.0](#3.3.0) |2019 年 10 月 8 日 |--- |
| [3.2.0](#3.2.0) |2019 年 9 月 18 日 |--- |
| [3.1.1](#3.1.1) |2019 年 8 月 12 日 |--- |
| [3.1.0](#3.1.0) |2019 年 7 月 29 日 |--- |
| [3.0.0](#3.0.0) |2019 年 7 月 15 日 |--- |

## <a name="faq"></a>常见问题
[!INCLUDE [cosmos-db-sdk-faq](../../includes/cosmos-db-sdk-faq.md)]

## <a name="see-also"></a>另请参阅
若要了解有关 Cosmos DB 的详细信息，请参阅 [Azure Cosmos DB](https://www.azure.cn/home/features/cosmos-db/) 服务页。

<!-- Update_Description: update meta properties, wording update, update link -->
