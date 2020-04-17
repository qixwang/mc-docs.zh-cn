---
author: ggailey777
ms.service: azure-functions
ms.topic: include
ms.date: 03/02/2020
ms.author: v-junlch
ms.openlocfilehash: 8382f317604ab3aeb4a31e73e0f3b3fd4aef5d6c
ms.sourcegitcommit: c1ba5a62f30ac0a3acb337fb77431de6493e6096
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2020
ms.locfileid: "78266057"
---
在 C# 类库项目中，绑定被定义为函数方法上的绑定属性。 然后，基于这些属性自动生成 Functions 所需的 function.json  文件。

打开 HttpExample.cs  项目文件，并将以下参数添加到 `Run` 方法定义中：

```cs
[Queue("outqueue"),StorageAccount("AzureWebJobsStorage")] ICollector<string> msg,
```
`msg` 参数为 `ICollector<T>` 类型，表示函数完成时写入输出绑定的消息集合。 在这种情况下，输出是名为的 `outqueue` 存储队列。 存储帐户的连接字符串由 `StorageAccountAttribute` 设置。 此属性指示包含存储帐户连接字符串的设置，可以在类、方法或参数级别应用。 在这种情况下，可以省略 `StorageAccountAttribute`，因为你已使用默认存储帐户。

Run 方法定义如下所示：  

```cs
[FunctionName("HttpExample")]
public static async Task<IActionResult> Run(
    [HttpTrigger(AuthorizationLevel.Function, "get", "post", Route = null)] HttpRequest req, 
    [Queue("outqueue"),StorageAccount("AzureWebJobsStorage")] ICollector<string> msg, 
    ILogger log)
```
