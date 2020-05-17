---
author: orspod
ms.service: data-explorer
ms.topic: include
origin.date: 10/07/2019
ms.date: 05/15/2020
ms.author: v-tawe
ms.openlocfilehash: f2169e962b24f736a18051513dc9df387697fa5b
ms.sourcegitcommit: bfbd6694da33f703481386f2a3f16850c4e94bfa
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/15/2020
ms.locfileid: "83417664"
---
## <a name="clean-up-resources"></a>清理资源

若要删除数据连接，请使用以下命令：

```csharp
kustoManagementClient.DataConnections.Delete(resourceGroupName, clusterName, databaseName, dataConnectionName);
```