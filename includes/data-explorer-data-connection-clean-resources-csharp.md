---
author: orspod
ms.service: data-explorer
ms.topic: include
origin.date: 10/07/2019
ms.date: 11/18/2019
ms.author: v-tawe
ms.openlocfilehash: fea203de6f7dfc566d12afdf50427797c23f30f6
ms.sourcegitcommit: c1ba5a62f30ac0a3acb337fb77431de6493e6096
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2020
ms.locfileid: "74020945"
---
## <a name="clean-up-resources"></a>清理资源

若要删除数据连接，请使用以下命令：

```csharp
kustoManagementClient.DataConnections.Delete(resourceGroupName, clusterName, databaseName, dataConnectionName);
```