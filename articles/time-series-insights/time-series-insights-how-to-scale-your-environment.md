---
title: 如何缩放环境 - Azure 时序见解 | Microsoft Docs
description: 了解如何使用 Azure 门户缩放 Azure 时序见解环境。
ms.service: time-series-insights
services: time-series-insights
author: deepakpalled
ms.author: v-yiso
manager: cshankar
ms.reviewer: v-mamcge, jasonh, kfile
ms.devlang: csharp
ms.workload: big-data
ms.topic: conceptual
origin.date: 11/22/2019
ms.date: 12/23/2019
ms.custom: seodec18
ms.openlocfilehash: a41f20b7f87a0c76a86fa7bb7669aef587fb18e7
ms.sourcegitcommit: 4a09701b1cbc1d9ccee46d282e592aec26998bff
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/25/2019
ms.locfileid: "75335692"
---
# <a name="how-to-scale-your-time-series-insights-environment"></a>如何缩放时序见解环境

本文介绍如何使用 Azure 门户更改时序见解环境的容量。 容量是应用于入口速率、存储容量以及与所选 SKU 关联的成本的乘数。 

可以使用 Azure 门户来增加或减少给定定价 SKU 中的容量。 

但是，不允许更改定价层 SKU。 例如，不能将定价 SKU 为 S1 的环境转换为 S2，反之亦然。 

## <a name="ga-limits"></a>GA 限制

[!INCLUDE [Azure Time Series Insights GA limits](../../includes/time-series-insights-ga-limits.md)]

## <a name="change-the-capacity-of-your-environment"></a>更改环境容量

1. 在 Azure 门户中，找到并选择自己的时序见解环境。

1. 在时序见解环境的菜单中，选择“存储配置”  。

   [![配置时序见解容量](media/scale-your-environment/scale-your-environment-configure.png)](media/scale-your-environment/scale-your-environment-configure.png#lightbox)

1. 调整“容量”  滑块来选择满足入口速率和存储容量需求的容量。 请注意，**入口速率**、**存储容量**和**估计成本**会动态更新，以显示更改产生的影响。

   [![使用容量滑块配置环境](media/scale-your-environment/scale-your-environment-slider.png)](media/scale-your-environment/scale-your-environment-slider.png#lightbox)

   或者，可以在滑块右侧的文本框中键入容量乘数的数字。

1. 选择“保存”以缩放环境。  在提交更改之前，进度指示器会短暂显示。

1. 检查新容量是否[足以防止出现限制情况](time-series-insights-diagnose-and-solve-problems.md)。

## <a name="next-steps"></a>后续步骤

- 有关详细信息，请参阅[了解时序见解中的保留期](time-series-insights-concepts-retention.md)。

- 了解如何[在 Azure 时序见解中配置数据保留](time-series-insights-how-to-configure-retention.md)。

- 了解如何[规划环境](time-series-insights-environment-planning.md)。