---
title: Azure Cosmos DB 中的开发和测试优化
description: 本文介绍 Azure Cosmos DB 如何为免费开发和测试服务提供多种选择。
author: rockboyfor
ms.author: v-yeche
ms.service: cosmos-db
ms.topic: conceptual
origin.date: 05/21/2019
ms.date: 10/28/2019
ms.openlocfilehash: aa471692f41a189032e8574d15622c50718bf1c9
ms.sourcegitcommit: c1ba5a62f30ac0a3acb337fb77431de6493e6096
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2020
ms.locfileid: "72914419"
---
# <a name="optimize-development-and-testing-cost-in-azure-cosmos-db"></a>在 Azure Cosmos DB 中优化开发和测试成本

本文介绍免费使用 Azure Cosmos DB 进行开发和测试的不同选项。

## <a name="azure-cosmos-db-emulator-locally-downloadable-version"></a>Azure Cosmos DB 模拟器（可以本地下载的版本）

[Azure Cosmos DB 模拟器](local-emulator.md)是一个本地的可下载版本，模拟 Azure Cosmos DB 云服务。 即使没有网络连接，也可以编写并测试使用 Azure Cosmos DB API 的代码，不需支付任何费用。 Azure Cosmos DB 模拟器提供了一个用于开发的具有云服务高保真特性的本地环境。 可以在本地开发和测试应用程序，无需创建 Azure 订阅。 做好将应用程序部署到云的准备以后，即可更新连接到云中的 Azure Cosmos DB 终结点所需的连接字符串，不需进行其他的修改。 也可在 Azure DevOps 中[通过 Azure Cosmos DB 模拟器生成任务设置 CI/CD 管道](tutorial-setup-ci-cd.md)，以便运行测试。 若要开始操作，可以参阅 [Azure Cosmos DB 模拟器](local-emulator.md)一文。

<!--Not Available on ## Try Azure Cosmos DB for free-->
<!--Not Available on [Try Azure Cosmos DB for free](https://www.azure.cn/try/cosmosdb/)-->

## <a name="azure-free-account"></a>Azure 免费帐户

Azure Cosmos DB 包含在 [Azure 试用帐户](https://www.azure.cn/pricing/1rmb-trial)中，该帐户提供 Azure 信用额度和资源，可以免费使用特定的一段时间。 特别对于 Azure Cosmos DB，该试用帐户提供 5 GB 的存储和 400 RU 的全年预配吞吐量。 任何开发人员都可以通过此体验轻松地测试 Azure Cosmos DB 的功能，或者将其与其他 Azure 服务集成，没有任何费用。 Azure 试用帐户提供 $200 的信用额度，可以在头 30 天使用。 在选择升级之前，即使开始使用服务也不会收费。 若要开始操作，请访问 [Azure 试用帐户](https://www.azure.cn/pricing/1rmb-trial)页。

## <a name="next-steps"></a>后续步骤

可以按照以下文章的说明，开始使用模拟器或免费的 Azure Cosmos DB 帐户：

* 详细了解[开发和测试优化](optimize-dev-test.md)
* 详细了解[了解 Azure Cosmos DB 帐单](understand-your-bill.md)
* 详细了解如何[优化吞吐量成本](optimize-cost-throughput.md)
* 详细了解如何[优化存储成本](optimize-cost-storage.md)
* 详细了解如何[优化读取和写入成本](optimize-cost-reads-writes.md)
* 详细了解如何[优化查询成本](optimize-cost-queries.md)
* 详细了解[优化多区域 Azure Cosmos 帐户的成本](optimize-cost-regions.md)

<!-- Update_Description: update meta properties, wording update -->