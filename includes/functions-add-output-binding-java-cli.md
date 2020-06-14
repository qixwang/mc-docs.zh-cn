---
author: ggailey777
ms.service: azure-functions
ms.topic: include
ms.date: 06/03/2020
ms.author: v-junlch
ms.openlocfilehash: 71af5368c84c01b13fd6839e9d6efb4c8ea9f0af
ms.sourcegitcommit: f1a76ee3242698123a3d77f44c860db040b48f70
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/09/2020
ms.locfileid: "84563695"
---
在 Java 项目中，绑定被定义为函数方法上的绑定注释。 然后根据这些注释自动生成 *function.json* 文件。

浏览到函数代码在 _src/main/java_ 下的位置，打开 *Function.java* 项目文件，然后将以下参数添加到 `run` 方法定义：

```java
@QueueOutput(name = "msg", queueName = "outqueue", connection = "AzureWebJobsStorage") OutputBinding<String> msg
```

`msg` 参数为 [`OutputBinding<T>`](https://docs.microsoft.com/java/api/com.microsoft.azure.functions.outputbinding) 类型，表示函数完成时作为消息写入到输出绑定的字符串集合。 在这种情况下，输出是名为的 `outqueue` 存储队列。 存储帐户的连接字符串由 `connection` 方法设置。 请传递包含存储帐户连接字符串的应用程序设置，而不是传递连接字符串本身。

`run` 方法定义现在应如以下示例所示：  

```java
@FunctionName("HttpTrigger-Java")
public HttpResponseMessage run(
        @HttpTrigger(name = "req", methods = {HttpMethod.GET, HttpMethod.POST}, authLevel = AuthorizationLevel.FUNCTION)  
        HttpRequestMessage<Optional<String>> request, 
        @QueueOutput(name = "msg", queueName = "outqueue", connection = "AzureWebJobsStorage") 
        OutputBinding<String> msg, final ExecutionContext context) {
    ...
}
```

