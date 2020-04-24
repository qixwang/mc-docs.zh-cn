---
title: 在 Azure Cosmos DB 中优化多区域部署的成本
description: 本文说明如何在 Azure Cosmos DB 中管理多区域部署的成本。
author: rockboyfor
ms.author: v-yeche
ms.service: cosmos-db
ms.topic: conceptual
origin.date: 07/31/2019
ms.date: 10/28/2019
ms.openlocfilehash: 0624cd8bf6345a3f1ce2323228285c0b9736d161
ms.sourcegitcommit: c1ba5a62f30ac0a3acb337fb77431de6493e6096
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2020
ms.locfileid: "72914423"
---
# <a name="optimize-multi-region-cost-in-azure-cosmos-db"></a>在 Azure Cosmos DB 中优化多区域成本

可随时对 Azure Cosmos 帐户添加或删除区域。 在与帐户关联的每个区域中会保留为各种 Azure Cosmos 数据库和容器配置的吞吐量。 如果每小时预配的吞吐量（即，跨所有数据库和容器为 Azure Cosmos 帐户配置的 RU/秒总和）为 `T`，并且与数据库帐户关联的 Azure 区域数是 `N`，则对于给定小时，Cosmos 帐户的总预配吞吐量等于：

1. `T x N RU/s`，如果使用单个写入区域配置 Azure Cosmos 帐户。 

1. `T x (N+1) RU/s`，如果使用能够处理写入的所有区域配置 Azure Cosmos 帐户。 

<!--MOONCAKE: $0.008 against CNY0.082 on 100 RU/s for Azure China-->

具有单个写入区域的预配吞吐量每 100 RU/秒的成本为 0.082 元/小时，具有多个可写区域的预配吞吐量每 100 RU/秒的成本为 0.102 元/小时。 若要了解详细信息，请参阅 Azure Cosmos DB [定价页](https://www.azure.cn/pricing/details/cosmos-db/)。

<!--MOONCAKE: $0.008 against CNY0.082 on 100 RU/s for Azure China-->

## <a name="costs-for-multiple-write-regions"></a>多个写入区域的成本

在多主数据库系统中，可用于写入操作的净 RU 会增加 `N` 倍，其中 `N` 是写入区域数。 与单区域写入不同，每个区域都可写，应支持冲突解决。 编写器的工作负载量会增加。 从成本规划角度来看，若要在中国范围内执行 `M` RU/秒的写入，需要在容器或数据库级别预配 M `RUs`。 随后可以添加任意多个区域并将它们用于写入以在中国范围内执行 `M` RU 写入。 

### <a name="example"></a>示例

假设你在中国北部有一个容器，该容器预配的吞吐量为 10K RU/秒，这个月存储的数据为 1 TB。 假设你添加了三个区域 - 中国东部、中国北部 2 和中国东部 2，每个区域的存储和吞吐量相同，而你希望能够通过自己的多区域分布式应用将内容写入所有四个区域中的容器。 一个月的总月度帐单（假定为 31 天）如下所示：

<!--MOONCAKE: Master region(WEST US) is China North, and the other 3 regions are China East , China North 2, and China East 2-->

|**项**|**使用情况（每月）**|**费率**|**每月成本**|
|----|----|----|----|
|中国北部（多个写入区域）容器的吞吐量帐单 |10K RU/秒 * 24 * 31 |每小时每 100 RU/秒 0.102 元 |7,588.8 元 |
|3 个其他区域 - 中国东部、中国北部 2 和中国东部 2（多个写入区域）的吞吐量帐单 |(3 + 1) * 10K RU/秒 * 24 * 31 |每小时每 100 RU/秒 0.102 元 |30,355.2 元 |
|中国北部容器的存储帐单 |1 TB（或 1,024 GB）  |2\.576 元/GB | 2637.83 元 |
|3 个其他区域 - 中国东部、中国北部 2 和中国东部 2 的存储帐单 |3 * 1 TB（或 3,072 GB）|2\.576 元/GB  |7,913.47 元 |
|**总计**|||**48,192.3 元** |

<!--MOONCAKE: Master region(WEST US) is China North, and the other 3 regions are China East , China North 2, and China East 2-->

## <a name="improve-throughput-utilization-on-a-per-region-basis"></a>按每个区域提高吞吐量利用率

如果使用率效率低下（例如，一个或多个区域利用率不足或过度利用），则可以执行以下步骤以提高吞吐量利用率：  

1. 请确保首先优化写入区域中的预配吞吐量 (RU)，然后通过使用来自读取区域等的更改源充分使用读取区域中的 RU。 

2. 多写入区域读取和写入可以跨与 Azure Cosmos 帐户关联的所有区域进行缩放。 

3. 监视区域中的活动，你可以按需添加和删除区域以缩放读取和写入吞吐量。

## <a name="next-steps"></a>后续步骤

接下来，可通过以下文章详细了解 Azure Cosmos DB 中的成本优化：

* 详细了解[开发和测试优化](optimize-dev-test.md)
* 详细了解[了解 Azure Cosmos DB 帐单](understand-your-bill.md)
* 详细了解如何[优化吞吐量成本](optimize-cost-throughput.md)
* 详细了解如何[优化存储成本](optimize-cost-storage.md)
* 详细了解如何[优化读取和写入成本](optimize-cost-reads-writes.md)
* 详细了解如何[优化查询成本](optimize-cost-queries.md)

<!--Update_Description: wording update, update link -->