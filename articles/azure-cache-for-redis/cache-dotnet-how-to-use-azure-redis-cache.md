---
title: 快速入门：了解如何将 Azure Redis 缓存与 .NET 应用配合使用 | Microsoft Docs
description: 本快速入门介绍如何从 .NET 应用访问 Azure Redis 缓存
services: cache,app-service
documentationcenter: ''
author: yegu-ms
manager: jhubbard
editor: ''
ms.assetid: c502f74c-44de-4087-8303-1b1f43da12d5
ms.service: cache
ms.workload: tbd
ms.tgt_pltfrm: cache
ms.devlang: dotnet
ms.topic: quickstart
origin.date: 05/18/2018
ms.date: 09/03/2019
ms.author: v-junlch
ms.custom: mvc
ms.openlocfilehash: 9bd49f2b5387cde386caa4f43b9c06ef4eeb9673
ms.sourcegitcommit: 3c98f52b6ccca469e598d327cd537caab2fde83f
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/13/2020
ms.locfileid: "79291991"
---
# <a name="quickstart-use-azure-cache-for-redis-with-a-net-framework-application"></a>快速入门：将 Azure Redis 缓存与 .NET Framework 应用程序配合使用

在本快速入门中，会将 Azure Redis 缓存合并到 .NET Framework 应用中，以便能够访问 Azure 中的任何应用程序都可以访问的安全专用缓存。 你专门在 .NET 控制台应用中将 [StackExchange.Redis](https://github.com/StackExchange/StackExchange.Redis) 客户端与 C# 代码配合使用。

## <a name="prerequisites"></a>先决条件

- Azure 订阅 - [创建订阅](https://www.azure.cn/pricing/1rmb-trial/)
- [Visual Studio 2019](https://www.visualstudio.com/downloads/)
- [.NET Framework 4 或更高版本](https://www.microsoft.com/net/download/dotnet-framework-runtime)，这是 StackExchange.Redis 客户端所必需的。

## <a name="create-a-cache"></a>创建缓存
[!INCLUDE [redis-cache-create](../../includes/redis-cache-create.md)]

[!INCLUDE [redis-cache-access-keys](../../includes/redis-cache-access-keys.md)]

在计算机上创建名为 *CacheSecrets.config* 的文件，将其放到不会连同示例应用程序源代码一起签入的位置。 在本快速入门中，*CacheSecrets.config* 文件的路径为 *C:\AppSecrets\CacheSecrets.config*。

编辑 *CacheSecrets.config* 文件，添加以下内容：

```xml
<appSettings>
    <add key="CacheConnection" value="<cache-name>.redis.cache.chinacloudapi.cn,abortConnect=false,ssl=true,password=<access-key>"/>
</appSettings>
```

将 `<cache-name>` 替换为缓存主机名。

将 `<access-key>` 替换缓存的主密钥。


## <a name="create-a-console-app"></a>创建控制台应用

在 Visual Studio 中，单击“文件”   > “新建”   > “项目”  。

在“Visual C#”  下，单击“Windows 经典桌面”  ，然后依次单击“控制台应用”  和“确定”  来创建新的控制台应用程序。


<a name="configure-the-cache-clients"></a>

## <a name="configure-the-cache-client"></a>配置缓存客户端

在本部分中，你将配置控制台应用程序来将 [StackExchange.Redis](https://github.com/StackExchange/StackExchange.Redis) 客户端用于 .NET。

在 Visual Studio 中，单击“工具” > “NuGet 包管理器” > “包管理器控制台”，然后从“包管理器控制台”窗口运行以下命令。   

```powershell
Install-Package StackExchange.Redis
```

安装完成后，*StackExchange.Redis* 缓存客户端可供用于你的项目。


## <a name="connect-to-the-cache"></a>连接到缓存

在 Visual Studio 中，打开 App.config  文件并将其更新为包含引用 CacheSecrets.config  文件的 `appSettings` `file` 属性。

```xml
<?xml version="1.0" encoding="utf-8" ?>
<configuration>
    <startup> 
        <supportedRuntime version="v4.0" sku=".NETFramework,Version=v4.7.1" />
    </startup>

    <appSettings file="C:\AppSecrets\CacheSecrets.config"></appSettings>  

</configuration>
```

在解决方案资源管理器中，右键单击“引用”并单击“添加引用”。   添加对 **System.Configuration** 程序集的引用。

向 *Program.cs* 中添加以下 `using` 语句：

```csharp
using StackExchange.Redis;
using System.Configuration;
```

与 Azure Redis 缓存的连接由 `ConnectionMultiplexer` 类管理。 应当在整个客户端应用程序中共享并重复使用此类。 不要为每个操作都创建新连接。 

切勿将凭据存储在源代码中。 为了保持简单，此示例仅使用一个外部机密配置文件。 更好的方法是使用[包含证书的 Azure Key Vault](https://docs.microsoft.com/rest/api/keyvault/certificate-scenarios)。

在 *Program.cs* 中，将以下成员添加到控制台应用程序的 `Program` 类：

```csharp
private static Lazy<ConnectionMultiplexer> lazyConnection = new Lazy<ConnectionMultiplexer>(() =>
{
    string cacheConnection = ConfigurationManager.AppSettings["CacheConnection"].ToString();
    return ConnectionMultiplexer.Connect(cacheConnection);
});

public static ConnectionMultiplexer Connection
{
    get
    {
        return lazyConnection.Value;
    }
}
```


此方式在应用程序中共享 `ConnectionMultiplexer` 实例，它使用返回所连接的实例的静态属性。 该代码提供了一种线程安全方式，它仅初始化单个已连接的 `ConnectionMultiplexer` 实例。 `abortConnect` 设置为 false，这意味着即使未建立与 Azure Redis 缓存的连接，调用也会成功。 `ConnectionMultiplexer` 的一个关键功能是，一旦解决网络问题和其他原因，它便会自动还原缓存连接。

*CacheConnection* appSetting 的值用来将 Azure 门户中的缓存连接字符串引用为 password 参数。

## <a name="executing-cache-commands"></a>执行缓存命令

为控制台应用程序的 `Program` 类的 `Main` 过程添加以下代码：

```csharp
static void Main(string[] args)
{
    // Connection refers to a property that returns a ConnectionMultiplexer
    // as shown in the previous example.
    IDatabase cache = lazyConnection.Value.GetDatabase();

    // Perform cache operations using the cache object...

    // Simple PING command
    string cacheCommand = "PING";
    Console.WriteLine("\nCache command  : " + cacheCommand);
    Console.WriteLine("Cache response : " + cache.Execute(cacheCommand).ToString());

    // Simple get and put of integral data types into the cache
    cacheCommand = "GET Message";
    Console.WriteLine("\nCache command  : " + cacheCommand + " or StringGet()");
    Console.WriteLine("Cache response : " + cache.StringGet("Message").ToString());

    cacheCommand = "SET Message \"Hello! The cache is working from a .NET console app!\"";
    Console.WriteLine("\nCache command  : " + cacheCommand + " or StringSet()");
    Console.WriteLine("Cache response : " + cache.StringSet("Message", "Hello! The cache is working from a .NET console app!").ToString());

    // Demonstrate "SET Message" executed as expected...
    cacheCommand = "GET Message";
    Console.WriteLine("\nCache command  : " + cacheCommand + " or StringGet()");
    Console.WriteLine("Cache response : " + cache.StringGet("Message").ToString());

    // Get the client list, useful to see if connection list is growing...
    cacheCommand = "CLIENT LIST";
    Console.WriteLine("\nCache command  : " + cacheCommand);
    Console.WriteLine("Cache response : \n" + cache.Execute("CLIENT", "LIST").ToString().Replace("id=", "id="));

    lazyConnection.Value.Dispose();
}
```

Azure Redis 缓存具有可配置的数据库数量（默认为 16 个），因此可以通过逻辑方式隔离 Azure Redis 缓存中的数据。 该代码将连接到默认数据库 DB 0。 有关详细信息，请参阅[什么是 Redis 数据库？](cache-faq.md#what-are-redis-databases)和[默认 Redis 服务器配置](cache-configure.md#default-redis-server-configuration)。

可以使用 `StringSet` 和 `StringGet` 方法来存储和检索缓存项。

Redis 将大多数数据存储为 Redis 字符串，但这些字符串可能包含许多类型的数据，包括序列化的二进制数据，可在缓存中存储 .NET 对象时使用。

按 **Ctrl+F5** 生成并运行控制台应用。

在以下示例中可以看到，`Message` 键事先已包含一个缓存值，该值是使用 Azure 门户中的 Redis 控制台设置的。 应用更新了该缓存值。 应用还执行了 `PING` 和 `CLIENT LIST` 命令。

![控制台应用的一部分](./media/cache-dotnet-how-to-use-azure-redis-cache/cache-console-app-partial.png)


## <a name="work-with-net-objects-in-the-cache"></a>处理缓存中的 .NET 对象

Azure Redis 缓存可以缓存 .NET 对象以及基元数据类型，但在缓存 .NET 对象之前，必须将其序列化。 此 .NET 对象序列化是应用程序开发人员的责任，同时赋与开发人员选择序列化程序的弹性。

将对象序列化的一种简单方式是使用 [Newtonsoft.Json](https://www.nuget.org/packages/Newtonsoft.Json/) 中的 `JsonConvert` 序列化方法，并以 JSON 为源和目标进行序列化。 在本部分中，将向缓存添加一个 .NET 对象。

在 Visual Studio 中，单击“工具” > “NuGet 包管理器” > “包管理器控制台”，然后从“包管理器控制台”窗口运行以下命令。   

```powershell
Install-Package Newtonsoft.Json
```

将下面的 `using` 语句添加到 *Program.cs* 的开头：

```csharp
using Newtonsoft.Json;
```

将下面的 `Employee` 类定义添加到 *Program.cs* 中：

```csharp
class Employee
{
    public string Id { get; set; }
    public string Name { get; set; }
    public int Age { get; set; }

    public Employee(string EmployeeId, string Name, int Age)
    {
        this.Id = EmployeeId;
        this.Name = Name;
        this.Age = Age;
    }
}
```

在 *Program.cs* 中 `Main()` 过程的底部并且在 `Dispose()` 调用之前，添加以下代码行来缓存和检索序列化的 .NET 对象：

```csharp
    // Store .NET object to cache
    Employee e007 = new Employee("007", "Davide Columbo", 100);
    Console.WriteLine("Cache response from storing Employee .NET object : " + 
        cache.StringSet("e007", JsonConvert.SerializeObject(e007)));

    // Retrieve .NET object from cache
    Employee e007FromCache = JsonConvert.DeserializeObject<Employee>(cache.StringGet("e007"));
    Console.WriteLine("Deserialized Employee .NET object :\n");
    Console.WriteLine("\tEmployee.Name : " + e007FromCache.Name);
    Console.WriteLine("\tEmployee.Id   : " + e007FromCache.Id);
    Console.WriteLine("\tEmployee.Age  : " + e007FromCache.Age + "\n");
```

按 **Ctrl+F5** 来生成并运行控制台应用以测试 .NET 对象的序列化。 

![已完成的控制台应用](./media/cache-dotnet-how-to-use-azure-redis-cache/cache-console-app-complete.png)


## <a name="clean-up-resources"></a>清理资源

如果想要继续学习下一篇教程，可以保留本快速入门中创建的资源，以便重复使用。

如果已完成快速入门示例应用程序，可以删除本快速入门中创建的 Azure 资源，以免产生费用。 

> [!IMPORTANT]
> 删除资源组的操作不可逆，资源组以及其中的所有资源将被永久删除。 请确保不会意外删除错误的资源组或资源。 如果在现有资源组（其中包含要保留的资源）中为托管此示例而创建了相关资源，可从各自的边栏选项卡逐个删除这些资源，而不要删除资源组。
>

登录到 [Azure 门户](https://portal.azure.cn)，然后单击“资源组”。 

在“按名称筛选...”文本框中键入资源组的名称  。 本文的说明使用了名为 *TestResources* 的资源组。 在结果列表中的资源组上，单击“...”，然后单击“删除资源组”   。

![Delete](./media/cache-dotnet-how-to-use-azure-redis-cache/cache-delete-resource-group.png)

系统会要求确认是否删除资源组。 键入资源组的名称进行确认，然后单击“删除”  。

片刻之后，将会删除该资源组及其包含的所有资源。



<a name="next-steps"></a>

## <a name="next-steps"></a>后续步骤

本快速入门介绍了如何通过 .NET 应用程序使用 Azure Redis 缓存。 请继续学习下一个快速入门，将 Azure Redis 缓存与 ASP.NET Web 应用配合使用。

> [!div class="nextstepaction"]
> [创建使用 Azure Redis 缓存的 ASP.NET Web 应用。](./cache-web-app-howto.md)

<!-- Update_Description: wording update -->

