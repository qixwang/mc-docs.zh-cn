---
author: ggailey777
ms.service: azure-functions
ms.topic: include
ms.date: 06/05/2020
ms.author: v-junlch
ms.openlocfilehash: ba4297c3e6b057c5bfbc548c374c3ac317c5c2ce
ms.sourcegitcommit: f1a76ee3242698123a3d77f44c860db040b48f70
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/09/2020
ms.locfileid: "84574646"
---
### <a name="functions-2x-and-higher"></a>Functions 2.x 及更高版本

```json
{
    "version": "2.0",
    "extensions": {
        "eventHubs": {
            "batchCheckpointFrequency": 5,
            "eventProcessorOptions": {
                "maxBatchSize": 256,
                "prefetchCount": 512
            }
        }
    }
}  
```

|属性  |默认 | 说明 |
|---------|---------|---------|
|maxBatchSize|10 个|每个接收循环收到的最大事件计数。|
|prefetchCount|300|基础 `EventProcessorHost` 使用的默认预提取计数。|
|batchCheckpointFrequency|1|创建 EventHub 游标检查点之前要处理的事件批数。|

> [!NOTE]
> 有关 Azure Functions 2.x 及更高版本中的 host.json 参考，请参阅 [Azure Functions 的 host.json 参考](../articles/azure-functions/functions-host-json.md)。

### <a name="functions-1x"></a>Functions 1.x

```json
{
    "eventHub": {
      "maxBatchSize": 64,
      "prefetchCount": 256,
      "batchCheckpointFrequency": 1
    }
}
```

|属性  |默认 | 说明 |
|---------|---------|---------| 
|maxBatchSize|64|每个接收循环收到的最大事件计数。|
|prefetchCount|不适用|基础 `EventProcessorHost` 将使用的默认预提取。| 
|batchCheckpointFrequency|1|创建 EventHub 游标检查点之前要处理的事件批数。| 

> [!NOTE]
> 有关 Azure Functions 1.x 中 host.json 的参考，请参阅 [Azure Functions 1.x 的 host.json 参考](../articles/azure-functions/functions-host-json-v1.md)。


