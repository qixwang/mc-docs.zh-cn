---
author: orspod
ms.service: data-explorer
ms.topic: include
origin.date: 10/07/2019
ms.date: 11/18/2019
ms.author: v-tawe
ms.openlocfilehash: ad744046825c053315bdbbcd6c7f67ccae28ff0f
ms.sourcegitcommit: c1ba5a62f30ac0a3acb337fb77431de6493e6096
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2020
ms.locfileid: "74020947"
---
## <a name="clean-up-resources"></a>清理资源

若要删除数据连接，请使用以下命令：

```python
kusto_management_client.data_connections.delete(resource_group_name=resource_group_name, cluster_name=kusto_cluster_name, database_name=kusto_database_name, data_connection_name=kusto_data_connection_name)
```