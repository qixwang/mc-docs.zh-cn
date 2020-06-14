---
author: ggailey777
ms.service: azure-functions
ms.topic: include
ms.date: 06/03/2020
ms.author: v-junlch
ms.openlocfilehash: da6115c7385d3351caf4df00d7954e87019db5a3
ms.sourcegitcommit: f1a76ee3242698123a3d77f44c860db040b48f70
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/09/2020
ms.locfileid: "84563712"
---
现在可以使用新的 `msg` 参数，从函数代码写入到输出绑定。 将以下代码行添加到成功响应之前，以便将 `name` 的值添加到 `msg` 输出绑定。

```Java
// Write the name to the message queue.
```

使用输出绑定时，无需使用 Azure 存储 SDK 代码进行身份验证、获取队列引用或写入数据。 Functions 运行时和队列输出绑定将为你执行这些任务。

`run` 方法现在应如以下示例所示：

```Java
@FunctionName("HttpExample")
public HttpResponseMessage run(
        @HttpTrigger(name = "req", methods = {HttpMethod.GET, HttpMethod.POST}, authLevel = AuthorizationLevel.ANONYMOUS) 
        HttpRequestMessage<Optional<String>> request, 
        @QueueOutput(name = "msg", queueName = "outqueue", 
        connection = "AzureWebJobsStorage") OutputBinding<String> msg, 
        final ExecutionContext context) {
    context.getLogger().info("Java HTTP trigger processed a request.");

    // Parse query parameter
    String query = request.getQueryParameters().get("name");
    String name = request.getBody().orElse(query);

    if (name == null) {
        return request.createResponseBuilder(HttpStatus.BAD_REQUEST)
        .body("Please pass a name on the query string or in the request body").build();
    } else {
        // Write the name to the message queue. 
        msg.setValue(name);

        return request.createResponseBuilder(HttpStatus.OK).body("Hello, " + name).build();
    }
```

