---
title: 从更改源处理器库迁移到 Azure Cosmos DB .NET V3 SDK
description: 了解如何使用更改源处理器库将应用程序迁移到 Azure Cosmos DB SDK V3
author: rockboyfor
ms.service: cosmos-db
ms.topic: how-to
origin.date: 09/17/2019
ms.date: 08/17/2020
ms.testscope: no
ms.testdate: ''
ms.author: v-yeche
ms.openlocfilehash: 89dc071585d2ffcd8c9a23b4fbf422d4b84581b5
ms.sourcegitcommit: 84606cd16dd026fd66c1ac4afbc89906de0709ad
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/14/2020
ms.locfileid: "88223442"
---
# <a name="migrate-from-the-change-feed-processor-library-to-the-azure-cosmos-db-net-v3-sdk"></a>从更改源处理器库迁移到 Azure Cosmos DB .NET V3 SDK

本文介绍了将使用[更改源处理器库](https://github.com/Azure/azure-documentdb-changefeedprocessor-dotnet)的现有应用程序代码迁移到 .NET SDK 最新版本（也称为 .NET V3 SDK）中的[更改源](change-feed.md)功能所需的步骤。

## <a name="required-code-changes"></a>所需的代码更改

.NET V3 SDK 有几项中断性变更，以下是迁移应用程序的关键步骤：

1. 将 `DocumentCollectionInfo` 实例转换为受监视容器和租约容器的 `Container` 引用。
1. 使用 `WithProcessorOptions` 的自定义应更新为对时间间隔使用 `WithLeaseConfiguration` 和 `WithPollInterval`、对`WithStartTime`开始时间[使用 ](how-to-configure-change-feed-start-time.md) 以及使用 `WithMaxItems` 来定义最大项计数。
1. 将 `processorName` 上的 `GetChangeFeedProcessorBuilder` 设置为与 `ChangeFeedProcessorOptions.LeasePrefix`上配置的值匹配，否则使用 `string.Empty`。
1. 更改将不再作为 `IReadOnlyList<Document>` 传递，而是作为 `IReadOnlyCollection<T>`，其中 `T` 是需要定义的类型，任何基项类都不再存在。
1. 若要处理这些更改，你不再需要一个实现，而是需要[定义一个委托](change-feed-processor.md#implementing-the-change-feed-processor)。 委托可以是静态函数，或者，如果需要跨执行维护状态，还可以创建自己的类并传递实例方法作为委托。

例如，如果用于生成更改源处理器的原始代码如下所示：

```csharp
ChangeFeedProcessorLibrary.DocumentCollectionInfo monitoredCollectionInfo = new ChangeFeedProcessorLibrary.DocumentCollectionInfo()
{
    DatabaseName = databaseId,
    CollectionName = Program.monitoredContainer,
    Uri = new Uri(configuration["EndPointUrl"]),
    MasterKey = configuration["AuthorizationKey"]
};

ChangeFeedProcessorLibrary.DocumentCollectionInfo leaseCollectionInfo = new ChangeFeedProcessorLibrary.DocumentCollectionInfo()
{
    DatabaseName = databaseId,
    CollectionName = Program.leasesContainer,
    Uri = new Uri(configuration["EndPointUrl"]),
    MasterKey = configuration["AuthorizationKey"]
};

ChangeFeedProcessorLibrary.ChangeFeedProcessorBuilder builder = new ChangeFeedProcessorLibrary.ChangeFeedProcessorBuilder();
var oldChangeFeedProcessor = await builder
    .WithHostName("consoleHost")
    .WithProcessorOptions(new ChangeFeedProcessorLibrary.ChangeFeedProcessorOptions {
        StartFromBeginning = true,
        LeasePrefix = "MyLeasePrefix" })
     .WithProcessorOptions(new ChangeFeedProcessorLibrary.ChangeFeedProcessorOptions()
     {
         MaxItemCount = 10,
         FeedPollDelay = TimeSpan.FromSeconds(1)
     })
    .WithFeedCollection(monitoredCollectionInfo)
    .WithLeaseCollection(leaseCollectionInfo)
    .WithObserver<ChangeFeedObserver>()
    .BuildAsync();

```

迁移的代码将如下所示：

```csharp
Container leaseContainer = client.GetContainer(databaseId, Program.leasesContainer);
Container monitoredContainer = client.GetContainer(databaseId, Program.monitoredContainer);
ChangeFeedProcessor changeFeedProcessor = monitoredContainer
    .GetChangeFeedProcessorBuilder<ToDoItem>("MyLeasePrefix", Program.HandleChangesAsync)
        .WithInstanceName("consoleHost")
        .WithLeaseContainer(leaseContainer)
        .WithMaxItems(10)
        .WithPollInterval(TimeSpan.FromSeconds(1))
        .WithStartTime(DateTime.MinValue.ToUniversalTime())
        .Build();

```

而委托可以是静态方法：

```csharp
static async Task HandleChangesAsync(IReadOnlyCollection<ToDoItem> changes, CancellationToken cancellationToken)
{
    foreach (ToDoItem item in changes)
    {
        Console.WriteLine($"\tDetected operation for item with id {item.id}, created at {item.creationTime}.");
        // Simulate work
        await Task.Delay(1);
    }
}

```

## <a name="state-and-lease-container"></a>状态和租约容器

与更改源处理器库类似，.NET V3 SDK 中的更改源功能使用[租约容器](change-feed-processor.md#components-of-the-change-feed-processor)来存储状态。 但是，架构是不同的。

SDK V3 更改源处理器将在首次执行迁移的应用程序代码时自动检测任何旧的库状态并将其迁移到新的架构。 

可以使用旧代码安全地停止应用程序，将代码迁移到新版本，启动已迁移的应用程序，而在应用程序停止时所发生的任何更改都将由新版本选取并处理。

> [!NOTE]
> 将使用该库的应用程序迁移到 .NET V3 SDK 是单向的，因为状态（租约）将迁移到新的架构。 此迁移不向后兼容。

## <a name="additional-resources"></a>其他资源

* [Azure Cosmos DB SDK](sql-api-sdk-dotnet.md)
* [GitHub 上的用法示例](https://github.com/Azure/azure-cosmos-dotnet-v3/tree/master/Microsoft.Azure.Cosmos.Samples/Usage/ChangeFeed)
* [GitHub 上的其他示例](https://github.com/Azure-Samples/cosmos-dotnet-change-feed-processor)

## <a name="next-steps"></a>后续步骤

现在，可以通过以下文章继续详细了解更改源处理器：

* [更改源处理器概述](change-feed-processor.md)
* [使用更改源估算器](how-to-use-change-feed-estimator.md)
* [更改源处理器开始时间](how-to-configure-change-feed-start-time.md)

<!-- Update_Description: update meta properties, wording update, update link -->