---
title: 检查 Azure 数据资源管理器群集的运行状况
description: 本文介绍监视 Azure 数据资源管理器群集运行状况的步骤。
author: orspod
ms.author: v-tawe
ms.reviewer: mblythe
ms.service: data-explorer
ms.topic: conceptual
origin.date: 09/24/2018
ms.date: 05/09/2020
ms.openlocfilehash: 9745d1a0698e8b950bb9807396d4e824a9c91bfd
ms.sourcegitcommit: bfbd6694da33f703481386f2a3f16850c4e94bfa
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/15/2020
ms.locfileid: "83417687"
---
# <a name="check-the-health-of-an-azure-data-explorer-cluster"></a>检查 Azure 数据资源管理器群集的运行状况

有几个因素会影响 Azure 数据资源管理器群集的运行状况，包括 CPU、内存和磁盘子系统。 本文介绍了可用于衡量群集运行状况的一些基本步骤。

1. 登录到 [https://dataexplorer.azure.cn](https://dataexplorer.azure.cn)。

1. 在左窗格中，选择群集，然后运行以下命令。

    ```Kusto
    .show diagnostics
    | project IsHealthy
    ```
    输出为 1 表示运行正常；输出为 0 表示运行不正常。

1. 登录 [Azure 门户](https://portal.azure.cn)，并导航到群集。

1. 在“监视”  下，选择“指标”  ，然后选择“保持活动状态”  ，如下图所示。 输出接近 1 表示群集正常运行。

    ![群集保持活动状态指标](media/check-cluster-health/portal-metrics.png)

1. 可以向图表添加其他指标。 选择图表，然后选择“添加指标”  。 选择另一个指标 - 此示例显示 **CPU**。

    ![添加指标](media/check-cluster-health/add-metric.png)

1. 如果需要有关诊断群集运行状况问题的帮助，请在 [Azure 门户](https://portal.azure.cn/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/overview)中打开支持请求。