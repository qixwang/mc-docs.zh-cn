---
title: Azure Cosmos DB 多区域分发
description: 了解如何在 Azure 门户中使用 Azure Cosmos DB 全局复制数据
services: cosmos-db
author: rockboyfor
ms.service: cosmos-db
ms.topic: include
origin.date: 03/26/2018
ms.date: 04/23/2018
ms.author: v-yeche
ms.custom: include file
ms.openlocfilehash: c9d6bb010fcf890a537f82571463c34c304753e7
ms.sourcegitcommit: c1ba5a62f30ac0a3acb337fb77431de6493e6096
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2020
ms.locfileid: "74203668"
---
<!-- Not Available on [!VIDEO https://www.youtube.com/embed/1D06yjTVxt8] -->

有关 Azure Cosmos DB 中多区域数据库复制工作原理的详细信息，请参阅[使用 Cosmos DB 全局分发数据](../articles/cosmos-db/distribute-data-globally.md)。

<a name="addregion"></a>
## <a name="add-global-database-regions-using-the-azure-portal"></a>使用 Azure 门户添加全局数据库区域
在中国的所有 [Azure 区域][azureregions]中都可使用 Azure Cosmos DB。 为数据库帐户选择默认的一致性级别后，可以关联一个或多个区域（具体取决于所选的默认一致性级别和多区域分发需求）。

1. 在 [Azure 门户](https://portal.azure.cn/)的左侧栏中，单击“Azure Cosmos DB”  。
2. 在“Azure Cosmos DB”  页中，选择要修改的数据库帐户。
3. 在“帐户”页上的菜单中单击“全局复制数据”  。
4. 在“全局复制数据”  页中，通过单击地图中的区域选择要添加或删除的区域，并单击“保存”  。 添加区域会产生费用，有关详细信息，请参阅[定价页](https://www.azure.cn/pricing/details/cosmos-db/)或[使用 Azure Cosmos DB 全局分发数据](../articles/cosmos-db/distribute-data-globally.md)。

    ![单击图中的区域可以添加或删除区域][1]

添加第二个区域后，门户中的“全局复制数据”页上会启用“手动故障转移”选项。 可以使用此选项测试故障转移过程或更改主写入区域。 添加第三个区域后，会在同一页上启用“故障转移优先级”  选项，以便更改读取的故障转移顺序。  

### <a name="selecting-global-database-regions"></a>选择全局数据库区域
配置两个或更多区域有两个常见方案：

1. 向最终用户提供对数据的低延迟访问，而无论用户位于全球范围内的何位置
2. 添加区域复原以实现业务连续性和灾难恢复 (BCDR)

若要向最终用户提供低延迟，建议在与应用程序用户所在位置对应的区域中同时部署应用程序和 Azure Cosmos DB。

<!-- Not Available on [Business continuity and disaster recovery (BCDR): Azure Paired Regions][bcdr] -->

<!--

<a name="selectwriteregion"></a>
## Select the write region

While all regions associated with your Cosmos DB database account can serve reads (both, single item as well as multi-item paginated reads) and queries, only one region can actively receive the write (insert, upsert, replace, delete) requests. To set the active write region, do the following  

1. In the **Azure Cosmos DB** blade, select the database account to modify.
2. In the account blade, click **Replicate data globally** from the menu.
3. In the **Replicate data globally** blade, click **Manual Failover** from the top bar.
    ![Change the write region under Azure Cosmos DB Account > Replicate data globally > Manual Failover][2]
4. Select a read region to become the new write region, click the checkbox to confirm triggering a failover, and click OK
    ![Change the write region by selecting a new region in list under Azure Cosmos DB Account > Replicate data globally > Manual Failover][3]

--->
<!-- Not Available on ## Select the write region -->

<!--Image references-->

[1]: ./media/cosmos-db-tutorial-global-distribution-portal/azure-cosmos-db-add-region.png
[2]: ./media/cosmos-db-tutorial-global-distribution-portal/azure-cosmos-db-manual-failover-1.png
[3]: ./media/cosmos-db-tutorial-global-distribution-portal/azure-cosmos-db-manual-failover-2.png

<!--Reference style links - using these makes the source content way more readable than using inline links-->
<!-- Not Available on [bcdr]: /best-practices-availability-paired-regions/ -->

[consistency]: ../articles/cosmos-db/consistency-levels.md
[azureregions]: https://status.azure.com/status/
[offers]: https://www.azure.cn/pricing/details/cosmos-db/

<!--Update_Description: wording update-->