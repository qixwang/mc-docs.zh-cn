---
title: 诊断和排查第 2 代环境的问题 - Azure 时序见解 | Microsoft Docs
description: 了解如何诊断和排查 Azure 时序见解第 2 代环境的问题。
author: deepakpalled
ms.author: v-junlch
manager: diviso
ms.workload: big-data
ms.service: time-series-insights
services: time-series-insights
ms.topic: conceptual
ms.date: 08/04/2020
ms.custom: seodec18
ms.openlocfilehash: 3039439c073367653557aa4b096e4147a280d4e7
ms.sourcegitcommit: 36e7f37481969f92138bfe70192b1f4a2414caf7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/05/2020
ms.locfileid: "87801839"
---
# <a name="diagnose-and-troubleshoot-an-azure-time-series-insights-gen2-environment"></a>诊断和排查 Azure 时序见解第 2 代环境的问题

本文汇总了在使用 Azure 时序见解第 2 代环境时可能会遇到的若干常见问题。 本文还介绍了每个问题的可能原因和解决方案。

## <a name="problem-some-data-shows-but-some-is-missing"></a>问题：显示了一些数据，但是还有一些数据缺失

可能在发送数据时没有提供时序 ID。

- 如果在发送事件时有效负载中没有时序 ID 字段，则可能会发生此问题。 有关详细信息，请阅读[支持的 JSON 形状](./how-to-shape-query-json.md)。
- 可能因环境受限而发生此问题。

    > [!NOTE]
    > 目前，时序见解支持的最大引入速率为 6 Mbps。

## <a name="problem-data-was-showing-but-now-ingestion-has-stopped"></a>问题：以前可以显示数据，但引入现已停止

- 可能已重新生成事件源密钥，并且第 2 代环境需要新的事件源密钥。

如果创建事件源时提供的密钥不再有效，则会出现此问题。 你会在中心看到遥测数据，但不会在时序见解中收到入口接收的消息。 如果不确定是否重新生成了密钥，可以在事件中心的活动日志中搜索“创建或更新命名空间授权规则”或“为 IoT 中心创建或更新 IotHub 资源”。 

若要用新密钥更新时序见解第 2 代环境，请在 Azure 门户中打开中心资源并复制新密钥。 导航到 TSI 资源，单击“事件源”。 

   [![更新密钥。](./media/preview-troubleshoot/update-hub-key-step-1.png)](./media/preview-troubleshoot/update-hub-key-step-1.png#lightbox)

选择已停止从其引入的事件源，粘贴新密钥，然后单击“保存”。

   [![更新密钥。](./media/preview-troubleshoot/update-hub-key-step-2.png)](./media/preview-troubleshoot/update-hub-key-step-2.png#lightbox)

## <a name="problem-power-bi-connector-shows-unable-to-connect"></a>问题：Power BI 连接器显示“无法连接”

如果未在 Power BI Desktop 中使用最新版本的 Power BI 连接器，可能会出现此问题。

[![无父级实例将显示警告。](./media/preview-troubleshoot/power-bi-unable-to-connect.png)](./media/preview-troubleshoot/power-bi-unable-to-connect.png#lightbox)

* 请检查 Power BI Desktop 的版本，并确保使用的是 2020 年 7 月版。 如果不是，请更新 Power BI Desktop 并再次运行该连接器。 

## <a name="next-steps"></a>后续步骤

- 阅读有关如何[使用时序模型](/time-series-insights/time-series-insights-overview)的信息。

- 了解[支持的 JSON 形状](./how-to-shape-query-json.md)。

- 查看 Azure 时序见解第 2 代中的[规划和限制](./time-series-insights-update-plan.md)。

