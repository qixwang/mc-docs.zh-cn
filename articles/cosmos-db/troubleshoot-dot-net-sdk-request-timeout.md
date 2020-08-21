---
title: 排查 .NET SDK 的 Azure Cosmos DB HTTP 408 或请求超时问题
description: 如何诊断和修复 .NET SDK 请求超时异常
author: rockboyfor
ms.service: cosmos-db
origin.date: 07/29/2020
ms.date: 08/17/2020
ms.testscope: no
ms.testdate: ''
ms.author: v-yeche
ms.topic: troubleshooting
ms.reviewer: sngun
ms.openlocfilehash: af0165d859c1f8a432016c4e96009d3ae91b58c4
ms.sourcegitcommit: 84606cd16dd026fd66c1ac4afbc89906de0709ad
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/14/2020
ms.locfileid: "88223481"
---
<!--Verified successfully-->
# <a name="diagnose-and-troubleshoot-azure-cosmos-db-net-sdk-request-timeout"></a>诊断和排查 Azure Cosmos DB .NET SDK 请求超时问题
如果 SDK 在超时限制发生之前无法完成请求，则会出现 HTTP 408 错误。

## <a name="customizing-the-timeout-on-the-azure-cosmos-net-sdk"></a>在 Azure Cosmos .NET SDK 中自定义超时

该 SDK 提供了两个不同选择来用于控制超时，每个都有不同的作用域。

### <a name="requesttimeout"></a>RequestTimeout

`CosmosClientOptions.RequestTimeout`（或 SDK V2 的 `ConnectionPolicy.RequestTimeout`）配置可用于设置影响每一单个网络请求的超时。  用户启动的操作可以跨多个网络请求（例如，可能会存在限制），此配置将适用于有关重试的每个网络请求。 这不是端到端操作请求超时。

### <a name="cancellationtoken"></a>CancellationToken

SDK 中的所有异步操作都有一个可选的 CancellationToken 参数。 此 [CancellationToken](https://docs.microsoft.com/dotnet/standard/threading/how-to-listen-for-cancellation-requests-by-polling) 在整个操作过程中（对所有网络请求）使用。 在网络请求之间，可能会检查 CancellationToken，如果相关令牌已过期，则会取消操作。 应使用 CancellationToken 来定义操作作用域上的近似预期超时。

<!--CORRECT ON (https://docs.microsoft.com/dotnet/standard/threading/how-to-listen-for-cancellation-requests-by-polling)-->

> [!NOTE]
> CancellationToken 是一种机制，其中库将在[它不会导致无效状态](https://devblogs.microsoft.com/premier-developer/recommended-patterns-for-cancellationtoken/)时检查取消。 操作可能不会在取消中定义的时间已到时正好取消，而是在该时间已到后，在取消该操作非常安全时取消。

## <a name="troubleshooting-steps"></a>疑难解答步骤
下面的列表包含请求超时异常的已知原因和解决方案。

### <a name="1-high-cpu-utilization-most-common-case"></a>1.CPU 使用率较高（最常见的情况）
为实现最佳延迟，建议 CPU 使用率大约为 40%。 建议使用 10 秒作为时间间隔来监视最大（而不是平均）CPU 使用率。 对于可能会为单个查询执行多个连接的跨分区查询，CPU 峰值更常见。

#### <a name="solution"></a>解决方案：
使用该 SDK 的客户端应用程序应纵向扩展/横向扩展。

### <a name="2-socket--port-availability-might-be-low"></a>2.套接字/端口可用性可能较低
使用 .NET SDK 的客户端在 Azure 中运行时，可能会遇到 Azure SNAT (PAT) 端口耗尽问题。

#### <a name="solution-1"></a>解决方案 1：
按照 [SNAT 端口耗尽指南](troubleshoot-dot-net-sdk.md#snat)操作。

#### <a name="solution-2"></a>解决方案 2：
如果使用 HTTP 代理，请确保它支持 SDK `ConnectionPolicy` 中配置的连接数。
否则，将遇到连接问题。

### <a name="3-creating-multiple-client-instances"></a>3.创建多个客户端实例
创建多个客户端实例可能会导致连接争用和超时问题。

#### <a name="solution"></a>解决方案：
遵循[性能提示](performance-tips-dotnet-sdk-v3-sql.md#sdk-usage)，并在整个过程中使用单个 DocumentClient 实例。

### <a name="4-hot-partition-key"></a>4.热分区键
Azure Cosmos DB 在物理分区之间均匀分配预配的总吞吐量。 存在热分区时，物理分区上的一个或多个逻辑分区键会消耗该物理分区的全部 RU/s，而其他物理分区上的 RU/s 则不会被使用。 作为一种症状，所消耗的 RU/s 总数将小于数据库或容器上的整体预配 RU/s，但针对该热逻辑分区键将会显示请求限制 (429s)。

<!--Not Available on [Normalized RU Consumption metric](monitor-normalized-request-units.md)-->

#### <a name="solution"></a>解决方案：
选择均匀分配请求量和存储的适当分区键。 了解如何[更改分区键](https://devblogs.microsoft.com/cosmosdb/how-to-change-your-partition-key/)。

### <a name="5-high-degree-of-concurrency"></a>5.并发度较高
应用程序正在执行高级别的并发，这可能会导致通道上出现争用

#### <a name="solution"></a>解决方案：
使用该 SDK 的客户端应用程序应纵向扩展/横向扩展。

### <a name="6-large-requests-andor-responses"></a>6.大型请求和/或响应
较大的请求或响应可能导致通道上出现队头阻塞并加剧资源争用（甚至在并发度相对较低的情况下）。

#### <a name="solution"></a>解决方案：
使用该 SDK 的客户端应用程序应纵向扩展/横向扩展。

### <a name="7-failure-rate-is-within-cosmos-db-sla"></a>7.失败率在 Cosmos DB SLA 内
应用程序应该能够处理暂时性故障，并在必要时重试。 408 异常不会被重试，因为在创建路径时不可能知道服务是否创建了该项。 再次发送相同的项进行创建将导致冲突异常。 用户应用程序业务逻辑可能包含用于处理冲突的自定义逻辑，这会消除现有项的不确定性与来自“创建”重试的冲突。

### <a name="8-failure-rate-is-violating-the-cosmos-db-sla"></a>8.失败率违反 Cosmos DB SLA
请联系 Azure 支持。

## <a name="next-steps"></a>后续步骤
* [诊断和排查](troubleshoot-dot-net-sdk.md)在使用 Azure Cosmos DB .NET SDK 时遇到的问题
* 了解 [.NET V3](performance-tips-dotnet-sdk-v3-sql.md) 和 [.NET V2](performance-tips.md) 的性能指南

<!-- Update_Description: new article about troubleshoot dot net sdk request timeout -->
<!--NEW.date: 08/10/2020-->