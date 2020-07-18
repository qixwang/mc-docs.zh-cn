---
title: Kusto 客户端库 - Azure 数据资源管理器 | Microsoft Docs
description: 本文介绍 Azure 数据资源管理器中的 Kusto 客户端库。
services: data-explorer
author: orspod
ms.author: v-tawe
ms.reviewer: rkarlin
ms.service: data-explorer
ms.topic: reference
origin.date: 03/15/2020
ms.date: 07/01/2020
ms.openlocfilehash: 203a272b33572f328829136b621987e9d86fe716
ms.sourcegitcommit: 9bc3e55f01e0999f05e7b4ebaea95f3ac91d32eb
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/10/2020
ms.locfileid: "86226273"
---
# <a name="kusto-client-library"></a>Kusto 客户端库
    
Kusto 客户端 SDK (Kusto.Data) 公开了类似于 ADO.NET 的编程 API，因此，对于那些有 .NET 经验的人来说，使用它应该感觉很自然。 可以从指向 Kusto 引擎服务、数据库、身份验证方法等的连接字符串对象创建查询客户端 (`ICslQueryProvider`) 或控制命令提供程序 (`ICslAdminProvider`)。然后，可以通过指定适当的 Kusto 查询语言字符串来发出数据查询或控制命令，并通过返回的 `IDataReader` 对象获取一个或多个数据表。

更具体地说，若要创建允许对 Kusto 进行查询的类似于 ADO.NET 的客户端，请使用 `Kusto.Data.Net.Client.KustoClientFactory` 类上的静态方法。 这些方法接受连接字符串并创建线程安全的、可释放的客户端对象。 （强烈建议不要让客户端代码创建此对象的“过多”实例， 而应让客户端代码为每个连接字符串创建一个对象，并在必要情况下长时间保留该对象。）这样，客户端对象就可以有效缓存资源。

通常，客户端上的所有方法都是线程安全的，但有两个例外：`Dispose` 和 setter 属性。 为了获得一致的结果，请勿以并发方式调用任一方法。

下面是几个示例。 可在[此处](https://github.com/Azure/azure-kusto-samples-dotnet/tree/master/client)找到其他示例。

**示例：计算行数**
 
以下代码演示如何计算名为 `Samples` 的数据库中名为 `StormEvents` 的表的行数：

```csharp
var client = Kusto.Data.Net.Client.KustoClientFactory.CreateCslQueryProvider("https://help.kusto.chinacloudapi.cn/Samples;Fed=true");
var reader = client.ExecuteQuery("StormEvents | count");
// Read the first row from reader -- it's 0'th column is the count of records in MyTable
// Don't forget to dispose of reader when done.
```

**示例：从 Kusto 群集获取诊断信息**

```csharp
var kcsb = new KustoConnectionStringBuilder(cluster URI here). WithAadUserPromptAuthentication();
using (var client = KustoClientFactory.CreateCslAdminProvider(kcsb))
{
    var diagnosticsCommand = CslCommandGenerator.GenerateShowDiagnosticsCommand();
    var objectReader = new ObjectReader<DiagnosticsShowCommandResult>(client.ExecuteControlCommand(diagnosticsCommand));
    DiagnosticsShowCommandResult diagResult = objectReader.ToList().FirstOrDefault();
    // DO something with the diagResult    
}
```



## <a name="the-kustoclientfactory-client-factory"></a>KustoClientFactory 客户端工厂

静态类 `Kusto.Data.Net.Client.KustoClientFactory` 为使用 Kusto 的客户端代码的作者提供主入口点。 它提供了以下重要的静态方法：

|方法                                      |返回                                |用途                                                      |
|--------------------------------------------|---------------------------------------|--------------------------------------------------------------|
|`CreateCslQueryProvider`                    |`ICslQueryProvider`                    |向 Kusto 引擎群集发送查询。                    |
|`CreateCslAdminProvider`                    |`ICslAdminProvider`                    |向 Kusto 群集（任何类型）发送控制命令。    |
|`CreateRedirectProvider`                    |`IRedirectProvider`                    |为 Kusto 请求创建重定向 HTTP 响应消息。|

