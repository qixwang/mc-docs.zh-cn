---
title: 使用 Azure 数据资源管理器 .NET Standard SDK（预览版）引入数据
description: 本文介绍如何使用 .NET Standard SDK 将数据引入（加载）到 Azure 数据资源管理器中。
author: orspod
ms.author: v-tawe
ms.reviewer: mblythe
ms.service: data-explorer
ms.topic: conceptual
origin.date: 05/19/2020
ms.date: 07/08/2020
ms.openlocfilehash: 4b0e044a4b2bb7d3961ff5b3b8b2105358e72643
ms.sourcegitcommit: 5fb9ae9adc04e79d6d0e78c9e69dbe8aa3ceb00a
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/08/2020
ms.locfileid: "86100249"
---
# <a name="ingest-data-using-the-azure-data-explorer-net-standard-sdk-preview"></a>使用 Azure 数据资源管理器 .NET Standard SDK（预览版）引入数据

Azure 数据资源管理器 (ADX) 是一项快速且高度可缩放的数据探索服务，适用于日志和遥测数据。 ADX 为 .NET Standard 提供了两个客户端库：[引入库](https://www.nuget.org/packages/Microsoft.Azure.Kusto.Ingest.NETStandard)和[数据库](https://www.nuget.org/packages/Microsoft.Azure.Kusto.Data.NETStandard)。 可以使用这些库在群集中引入（加载）数据并从代码中查询数据。 本文首先在测试群集中创建一个表和数据映射。 然后将引入排列到群集并验证结果。

## <a name="prerequisites"></a>必备条件

* 如果没有 Azure 订阅，可在开始前创建一个 [试用帐户](https://www.azure.cn/pricing/1rmb-trial) 。

* [测试群集和数据库](create-cluster-database-portal.md)

## <a name="install-the-ingest-library"></a>安装引入库

```
Install-Package Microsoft.Azure.Kusto.Ingest.NETStandard
```

## <a name="authentication"></a>Authentication

Azure 数据资源管理器使用 AAD 租户 ID，以对应用程序进行身份验证。 要查找租户 ID，请使用以下 URL，并将域替换为 YourDomain  。

```
https://login.chinacloudapi.cn/<YourDomain>/.well-known/openid-configuration/
```

例如，如果域名为 contoso.com，则该 URL 将是：[https://login.chinacloudapi.cn/contoso.com/.well-known/openid-configuration/](https://login.chinacloudapi.cn/contoso.com/.well-known/openid-configuration/)。 单击此 URL 以查看结果；第一行如下所示。 

```
"authorization_endpoint":"https://login.chinacloudapi.cn/6babcaad-604b-40ac-a9d7-9fd97c0b779f/oauth2/authorize"
```

在这种情况下，租户 ID 为 `6babcaad-604b-40ac-a9d7-9fd97c0b779f`。

此示例使用 AAD 用户和密码进行访问群集所需的身份验证。 也可使用 AAD 应用程序证书和 AAD 应用程序密钥。 为 `tenantId`、`user`、`password` 设置值，然后运行以下代码。

```csharp
var tenantId = "<TenantId>";
var user = "<User>";
var password = "<Password>";
```

## <a name="construct-the-connection-string"></a>构造连接字符串
现在构造连接字符串。 在后续步骤中创建目标表和映射。

```csharp
var kustoUri = "https://<ClusterName>.<Region>.kusto.chinacloudapi.cn:443/";
var database = "<DatabaseName>";

var kustoConnectionStringBuilder =
    new KustoConnectionStringBuilder(kustoUri)
    {
        FederatedSecurity = true,
        InitialCatalog = database,
        UserID = user,
        Password = password,
        Authority = tenantId
    };
```

## <a name="set-source-file-information"></a>设置源文件信息

设置源文件的路径。 此示例使用 Azure Blob 存储上托管的示例文件。 StormEvents  示例数据集包含[美国国家环境信息中心](https://www.ncdc.noaa.gov/stormevents/)中与天气相关的数据。

```csharp
var blobPath = "https://kustosamplefiles.blob.core.chinacloudapi.cn/samplefiles/StormEvents.csv?st=2018-08-31T22%3A02%3A25Z&se=2020-09-01T22%3A02%3A00Z&sp=r&sv=2018-03-28&sr=b&sig=LQIbomcKI8Ooz425hWtjeq6d61uEaq21UVX7YrM61N4%3D";
```

## <a name="create-a-table-on-your-test-cluster"></a>在测试群集上创建表
创建与 `StormEvents.csv` 文件中的数据架构匹配的名为 `StormEvents` 的表。

```csharp
var table = "StormEvents";
using (var kustoClient = KustoClientFactory.CreateCslAdminProvider(kustoConnectionStringBuilder))
{
    var command =
        CslCommandGenerator.GenerateTableCreateCommand(
            table,
            new[]
            {
                Tuple.Create("StartTime", "System.DateTime"),
                Tuple.Create("EndTime", "System.DateTime"),
                Tuple.Create("EpisodeId", "System.Int32"),
                Tuple.Create("EventId", "System.Int32"),
                Tuple.Create("State", "System.String"),
                Tuple.Create("EventType", "System.String"),
                Tuple.Create("InjuriesDirect", "System.Int32"),
                Tuple.Create("InjuriesIndirect", "System.Int32"),
                Tuple.Create("DeathsDirect", "System.Int32"),
                Tuple.Create("DeathsIndirect", "System.Int32"),
                Tuple.Create("DamageProperty", "System.Int32"),
                Tuple.Create("DamageCrops", "System.Int32"),
                Tuple.Create("Source", "System.String"),
                Tuple.Create("BeginLocation", "System.String"),
                Tuple.Create("EndLocation", "System.String"),
                Tuple.Create("BeginLat", "System.Double"),
                Tuple.Create("BeginLon", "System.Double"),
                Tuple.Create("EndLat", "System.Double"),
                Tuple.Create("EndLon", "System.Double"),
                Tuple.Create("EpisodeNarrative", "System.String"),
                Tuple.Create("EventNarrative", "System.String"),
                Tuple.Create("StormSummary", "System.Object"),
            });

    kustoClient.ExecuteControlCommand(command);
}
```

## <a name="define-ingestion-mapping"></a>定义引入映射

将传入的 CSV 数据映射到创建表时使用的列名称。
在该表上预配 [CSV 列映射对象](https://docs.microsoft.com/azure/data-explorer/kusto/management/create-ingestion-mapping-command)

```csharp
var tableMapping = "StormEvents_CSV_Mapping";
using (var kustoClient = KustoClientFactory.CreateCslAdminProvider(kustoConnectionStringBuilder))
{
    var command =
        CslCommandGenerator.GenerateTableMappingCreateCommand(
            Data.Ingestion.IngestionMappingKind.Csv,
            table,
            tableMapping,
            new[] {
                new ColumnMapping() { ColumnName = "StartTime", Properties = new Dictionary<string, string>() { { MappingConsts.Ordinal, "0" } } },
                new ColumnMapping() { ColumnName = "EndTime", Properties =  new Dictionary<string, string>() { { MappingConsts.Ordinal, "1" } } },
                new ColumnMapping() { ColumnName = "EpisodeId", Properties = new Dictionary<string, string>() { { MappingConsts.Ordinal, "2" } } },
                new ColumnMapping() { ColumnName = "EventId", Properties =  new Dictionary<string, string>() { { MappingConsts.Ordinal, "3" } } },
                new ColumnMapping() { ColumnName = "State", Properties =  new Dictionary<string, string>() { { MappingConsts.Ordinal, "4" } } },
                new ColumnMapping() { ColumnName = "EventType", Properties =  new Dictionary<string, string>() { { MappingConsts.Ordinal, "5" } } },
                new ColumnMapping() { ColumnName = "InjuriesDirect", Properties =  new Dictionary<string, string>() { { MappingConsts.Ordinal, "6" } } },
                new ColumnMapping() { ColumnName = "InjuriesIndirect", Properties =  new Dictionary<string, string>() { { MappingConsts.Ordinal, "7" } } },
                new ColumnMapping() { ColumnName = "DeathsDirect", Properties =  new Dictionary<string, string>() { { MappingConsts.Ordinal, "8" } } },
                new ColumnMapping() { ColumnName = "DeathsIndirect", Properties =  new Dictionary<string, string>() { { MappingConsts.Ordinal, "9" } } },
                new ColumnMapping() { ColumnName = "DamageProperty", Properties =  new Dictionary<string, string>() { { MappingConsts.Ordinal, "10" } } },
                new ColumnMapping() { ColumnName = "DamageCrops", Properties =  new Dictionary<string, string>() { { MappingConsts.Ordinal, "11" } } },
                new ColumnMapping() { ColumnName = "Source", Properties =  new Dictionary<string, string>() { { MappingConsts.Ordinal, "12" } } },
                new ColumnMapping() { ColumnName = "BeginLocation", Properties =  new Dictionary<string, string>() { { MappingConsts.Ordinal, "13" } } },
                new ColumnMapping() { ColumnName = "EndLocation", Properties =  new Dictionary<string, string>() { { MappingConsts.Ordinal, "14" } } },
                new ColumnMapping() { ColumnName = "BeginLat", Properties =  new Dictionary<string, string>() { { MappingConsts.Ordinal, "15" } } },
                new ColumnMapping() { ColumnName = "BeginLon", Properties =  new Dictionary<string, string>() { { MappingConsts.Ordinal, "16" } } },
                new ColumnMapping() { ColumnName = "EndLat", Properties =  new Dictionary<string, string>() { { MappingConsts.Ordinal, "17" } } },
                new ColumnMapping() { ColumnName = "EndLon", Properties =  new Dictionary<string, string>() { { MappingConsts.Ordinal, "18" } } },
                new ColumnMapping() { ColumnName = "EpisodeNarrative", Properties =  new Dictionary<string, string>() { { MappingConsts.Ordinal, "19" } } },
                new ColumnMapping() { ColumnName = "EventNarrative", Properties =  new Dictionary<string, string>() { { MappingConsts.Ordinal, "20" } } },
                new ColumnMapping() { ColumnName = "StormSummary", Properties =  new Dictionary<string, string>() { { MappingConsts.Ordinal, "21" } } }
        });

    kustoClient.ExecuteControlCommand(command);
}
```

## <a name="queue-a-message-for-ingestion"></a>列入一条引入消息

将一条消息排入队列，以便从 Blob 存储中提取数据并将该数据引入到 ADX。

```csharp
var ingestUri = "https://ingest-<ClusterName>.<Region>.kusto.chinacloudapi.cn:443/";
var ingestConnectionStringBuilder =
    new KustoConnectionStringBuilder(ingestUri)
    {
        FederatedSecurity = true,
        InitialCatalog = database,
        UserID = user,
        Password = password,
        Authority = tenantId
    };

using (var ingestClient = KustoIngestFactory.CreateQueuedIngestClient(ingestConnectionStringBuilder))
{
    var properties =
        new KustoQueuedIngestionProperties(database, table)
        {
            Format = DataSourceFormat.csv,
            IngestionMapping = new IngestionMapping()
            { 
                IngestionMappingReference = tableMapping
            },
            IgnoreFirstRecord = true
        };

    ingestClient.IngestFromStorageAsync(blobPath ingestionProperties: properties);
}
```

## <a name="validate-data-was-ingested-into-the-table"></a>验证数据已引入表中

等待五到十分钟，直到排入队列的引入已计划在 ADX 中引入和加载数据。 然后运行以下代码，以获取 `StormEvents` 表中记录的计数。

```csharp
using (var cslQueryProvider = KustoClientFactory.CreateCslQueryProvider(kustoConnectionStringBuilder))
{
    var query = $"{table} | count";

    var results = cslQueryProvider.ExecuteQuery<long>(query);
    Console.WriteLine(results.Single());
}
```

## <a name="run-troubleshooting-queries"></a>运行故障排除查询

登录到 [https://dataexplorer.azure.cn](https://dataexplorer.azure.cn) 并连接到群集。 在数据库中运行以下命令以查看过去四个小时内是否存在任何失败引入。 在运行之前替换数据库名称。

```Kusto
.show ingestion failures
| where FailedOn > ago(4h) and Database == "<DatabaseName>"
```

运行以下命令以查看过去四个小时内所有引入操作的状态。 在运行之前替换数据库名称。

```Kusto
.show operations
| where StartedOn > ago(4h) and Database == "<DatabaseName>" and Operation == "DataIngestPull"
| summarize arg_max(LastUpdatedOn, *) by OperationId
```

## <a name="clean-up-resources"></a>清理资源

如果计划学习我们的其他文章，请保留已创建的资源。 否则，在数据库中运行以下命令以清除 `StormEvents` 表。

```Kusto
.drop table StormEvents
```

## <a name="next-steps"></a>后续步骤

* [编写查询](write-queries.md)
