---
title: 优化 Gen2 缓存
description: 了解如何通过 Azure 门户监视 Gen2 缓存。
services: sql-data-warehouse
author: WenJason
manager: digimobile
ms.service: sql-data-warehouse
ms.subservice: performance
ms.topic: conceptual
origin.date: 09/06/2018
ms.date: 03/02/2020
ms.author: v-jay
ms.reviewer: igorstan
ms.openlocfilehash: f402254966523ffab6a1c50ea31a200d4b8d6e11
ms.sourcegitcommit: c1ba5a62f30ac0a3acb337fb77431de6493e6096
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2020
ms.locfileid: "78154405"
---
# <a name="how-to-monitor-the-gen2-cache"></a>如何监视 Gen2 缓存
Gen2 存储体系结构自动将最常查询的列存储段归类到特定的缓存中，该缓存位于基于 NVMe 且专为 Gen2 数据仓库设计的 SSD 中。 如果查询检索驻留在缓存中的段，则可提高性能。 本文介绍如何监视和排查查询性能下降的问题，只需确定工作负荷是否在充分利用 Gen2 缓存即可。  
## <a name="troubleshoot-using-the-azure-portal"></a>使用 Azure 门户进行故障排除
可以使用 Azure Monitor 来查看 Gen2 缓存指标，以便排查查询性能问题。 首先转到 Azure 门户，然后单击“监视”：

![Azure Monitor](./media/sql-data-warehouse-cache-portal/cache_0.png)

选择指标按钮并填写数据仓库的**订阅**、**资源** **组**、**资源类型**和**资源名称**。

排查 Gen2 缓存问题时，关键指标是“缓存命中百分比”和“缓存使用百分比”。   配置 Azure 指标图表，以便显示这两个指标。

![缓存指标](./media/sql-data-warehouse-cache-portal/cache_1.png)


## <a name="cache-hit-and-used-percentage"></a>缓存命中和使用百分比
下表根据缓存指标的值对场景进行了说明：

|                                | **缓存命中百分比高** | **缓存命中百分比低** |
| :----------------------------: | :---------------------------: | :--------------------------: |
| **缓存使用百分比高** |          方案 1           |          方案 2          |
| **缓存使用百分比低**  |          方案 3           |          方案 4          |

**场景 1：** 你的缓存使用已优化。 [排查](sql-data-warehouse-manage-monitor.md)可能导致查询速度变慢的其他方面的情况。

**场景 2：** 当前工作数据集不适合放置在缓存中，引发物理读取困难，导致缓存命中百分比低。 考虑提升性能级别并重新运行工作负荷，以便填充缓存。

**场景 3：** 查询运行速度慢的原因可能与缓存无关。 [排查](sql-data-warehouse-manage-monitor.md)可能导致查询速度变慢的其他方面的情况。 也可以考虑[缩减实例](sql-data-warehouse-manage-monitor.md)，通过缩减缓存大小来节省成本。 

**场景 4：** 你使用了冷缓存，这可能是查询速度慢的原因。 考虑重新运行查询，因此工作数据集现在应该位于缓存中。 

> [!IMPORTANT]
> 如果缓存命中百分比或缓存使用百分比在重新运行工作负荷后未更新，则表明工作集可能已驻留在内存中。 仅缓存聚集列存储表。

## <a name="next-steps"></a>后续步骤
有关常规查询性能优化的详细信息，请参阅[监视查询执行](../sql-data-warehouse/sql-data-warehouse-manage-monitor.md#monitor-query-execution)。
