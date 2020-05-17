---
author: orspod
ms.service: data-explorer
ms.topic: include
origin.date: 10/07/2019
ms.date: 05/15/2020
ms.author: v-tawe
ms.openlocfilehash: 2f24a2adabb865f918bac45e28c99ead60d5ce65
ms.sourcegitcommit: bfbd6694da33f703481386f2a3f16850c4e94bfa
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/15/2020
ms.locfileid: "83417769"
---
## <a name="clean-up-resources"></a>清理资源

若要删除数据连接，请使用以下命令：

```python
kusto_management_client.data_connections.delete(resource_group_name=resource_group_name, cluster_name=kusto_cluster_name, database_name=kusto_database_name, data_connection_name=kusto_data_connection_name)
```