---
author: mgblythe
ms.service: data-explorer
ms.topic: include
origin.date: 11/14/2018
ms.date: 05/15/2020
ms.author: v-tawe
ms.openlocfilehash: 7d3f2317bff8c2cf4e5bfd8b715824e6e9aa8071
ms.sourcegitcommit: bfbd6694da33f703481386f2a3f16850c4e94bfa
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/15/2020
ms.locfileid: "83417606"
---
Power BI Desktop 中有了数据以后，即可创建基于该数据的报表。 将创建一个简单的包含柱状图的报表，以便按州显示作物损坏情况。

1. 在 Power BI 主窗口左侧，选择报表视图。

    ![报表视图](media/data-explorer-power-bi-visualize-basic/report-view.png)

1. 在“可视化”  窗格中，选择“簇状柱形图”。

    ![添加柱形图](media/data-explorer-power-bi-visualize-basic/add-column-chart.png)

    会向画布添加一个空白图。

    ![空白图](media/data-explorer-power-bi-visualize-basic/blank-chart.png)

1. 在“字段”  列表中，选择“DamageCrops”  和“州”  。

    ![选择字段](media/data-explorer-power-bi-visualize-basic/select-fields.png)

    现在已有一张图表，显示表中最前面 1000 行对应的作物损坏情况。

    ![作物损坏情况（按州）](media/data-explorer-power-bi-visualize-basic/damage-column-chart.png)

1. 保存报表。