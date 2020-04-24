---
title: 适用于 Azure Cosmos DB MongoDB API 的 Azure CLI 示例
description: 适用于 Azure Cosmos DB MongoDB API 的 Azure CLI 示例
author: rockboyfor
ms.service: cosmos-db
ms.subservice: cosmosdb-mongo
ms.topic: sample
origin.date: 09/25/2019
ms.date: 10/28/2019
ms.author: v-yeche
ms.openlocfilehash: 4bd17b958eb7860d528fccc5bfbe5f02ba243cc5
ms.sourcegitcommit: c1ba5a62f30ac0a3acb337fb77431de6493e6096
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2020
ms.locfileid: "72970285"
---
# <a name="azure-cli-samples-for-azure-cosmos-db-mongodb-api"></a>适用于 Azure Cosmos DB MongoDB API 的 Azure CLI 示例

下表包括适用于 Azure Cosmos DB MongoDB API 的示例 Azure CLI 脚本的链接。 [Azure CLI 参考](https://docs.azure.cn/cli/cosmosdb?view=azure-cli-latest)中收录了所有 Azure Cosmos DB CLI 命令的参考页。 可以在 [Azure Cosmos DB CLI GitHub 存储库](https://github.com/Azure-Samples/azure-cli-samples/tree/master/cosmosdb)中找到所有 Azure Cosmos DB CLI 脚本示例。

| |  |
|---|---|
| [创建 Azure Cosmos 帐户、数据库和集合](scripts/cli/mongodb/create.md?toc=%2fcli%2ftoc.json)| 为 MongoDB API 创建 Azure Cosmos DB 帐户、数据库和集合。 |
| [更改吞吐量](scripts/cli/mongodb/throughput.md?toc=%2fcli%2ftoc.json) | 更新数据库和集合的 RU/秒。|
| [添加或故障转移区域](scripts/cli/common/regions.md?toc=%2fcli%2ftoc.json) | 添加区域、更改故障转移优先级、触发手动故障转移。|
| [帐户密钥和连接字符串](scripts/cli/common/keys.md?toc=%2fcli%2ftoc.json) | 列出帐户密钥、只读密钥，重新生成密钥并列出连接字符串。|
| [使用 IP 防火墙保护](scripts/cli/common/ipfirewall.md?toc=%2fcli%2ftoc.json)| 创建配置了 IP 防火墙的 Cosmos 帐户。|
| [使用服务终结点保护新帐户](scripts/cli/common/service-endpoints.md?toc=%2fcli%2ftoc.json)| 使用服务终结点创建 Cosmos 帐户并确保其安全。|
| [使用服务终结点保护现有帐户](scripts/cli/common/service-endpoints-ignore-missing-vnet.md?toc=%2fcli%2ftoc.json)| 最终配置子网后，更新 Cosmos 帐户以使用服务终结点进行保护。|
|||

<!--Update_Description: new articles on cli samples mongodb  -->
<!--New.date: 10/28/2019-->