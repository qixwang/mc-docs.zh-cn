---
title: 如何创建 Azure 自动化更新管理警报
description: 本文介绍如何配置 Azure 警报以通知有关更新评估或部署的状态。
services: automation
ms.subservice: update-management
origin.date: 07/28/2020
ms.date: 08/10/2020
ms.topic: conceptual
ms.openlocfilehash: 534b7ef9c900798a833a6f4490b67f99293221e5
ms.sourcegitcommit: e6b216b180734783219378410e13192e314a4497
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/05/2020
ms.locfileid: "87790552"
---
# <a name="how-to-create-alerts-for-update-management"></a>如何创建更新管理警报

Azure 中的警报会主动通知你 runbook 作业、服务运行状况问题或与自动化帐户相关的其他情况的结果。 Azure 自动化不包括预配置的警报规则，但你可以基于其生成的数据创建自己的警报规则。 本文介绍如何使用更新管理附带的指标创建警报规则。

## <a name="available-metrics"></a>可用度量值

Azure 自动化创建两个不同的平台指标，它们与收集并转发到 Azure Monitor 的更新管理相关。 使用[指标资源管理器](../../azure-monitor/platform/metrics-charts.md) 时，可以使用这些指标进行分析，并使用[指标警报规则](../../azure-monitor/platform/alerts-metric.md)发出警报。

发出的两个指标为：

* 更新部署计算机运行总数
* 更新部署运行总数

用于警报时，这两个指标都支持包含附加信息的维度，以帮助将警报的范围限定为特定的更新部署详细信息。 下表显示了有关配置警报时可用的指标和维度的详细信息。

|信号名称|维度|说明
|---|---|---|
|`Total Update Deployment Runs`|- 更新部署名称<br>- 状态 | 针对更新部署的总体状态的警报。|
|`Total Update Deployment Machine Runs`|- 更新部署名称</br>- 状态</br>- 目标计算机</br>- 更新部署运行 ID    |针对特定计算机的更新部署状态的警报。|

## <a name="create-alert"></a>创建警报

按照以下步骤设置警报，以获取更新部署的状态。 如果不熟悉 Azure 警报，请参阅 [Azure 警报概述](../../azure-monitor/platform/alerts-overview.md)。

1. 在自动化帐户中，选择“监视”下的“警报”，再选择“新建警报规则”  。

2. 在“创建警报规则”页上，系统已选择你的自动化帐户作为资源。 如果要对其进行更改，请选择“编辑资源”。

3. 在“选择资源”页上，从“按资源类型筛选”下拉列表中选择“自动化帐户” 。

4. 选择要使用的自动化帐户，再选择“完成”。

5. 选择“添加条件”以选择适合要求的内容。

6. 对于维度，请从列表中选择一个有效值。 如果所需的值不在列表中，请选择维度旁边的 \+，并键入自定义名称。 然后选择要查找的值。 若要为某个维度选择所有值，请选择“选择 \*”按钮。 如果没有为某个维度选择值，更新管理将忽略该维度。

    ![配置信号逻辑](./media/update-mgmt-manage-updates-for-vm/signal-logic.png)

7. 在“警报逻辑”中的“时间聚合”和“阈值”字段中输入值，然后选择“完成”   。

8. 在下一页上，输入警报的名称和描述。

9. 对于成功的运行，请将“严重性”字段设置为“信息(严重性 2)”；对于失败的运行，请将其设置为“信息(严重性 1)”  。

    ![配置信号逻辑](./media/update-mgmt-manage-updates-for-vm/define-alert-details.png)

10. 选择“是”以启用预警规则。

## <a name="configure-action-groups-for-your-alerts"></a>配置警报对应的操作组

配置警报后，可以设置操作组，即用于多个警报的一组操作。 这些操作可能包括电子邮件通知、Runbook、Webhook 等。 若要了解有关操作组的详细信息，请参阅[创建和管理操作组](../../azure-monitor/platform/action-groups.md)。

1. 选择警报，然后在“操作组”下选择“新建” 。

2. 输入操作组的全名和短名称。 使用指定的组发送通知时，更新管理将使用短名称。

3. 在“操作”下，输入指定操作的名称，例如“电子邮件通知” 。

4. 对于“操作类型”，选择适当的类型，例如“电子邮件/短信” 。

5. 选择“编辑详细信息”。

6. 填写操作类型的窗格。 例如，如果使用“电子邮件/短信”，请输入操作名称，选择“电子邮件”复选框，输入有效的电子邮件地址，然后单击“确定”  。

    ![配置电子邮件操作组](./media/update-mgmt-manage-updates-for-vm/configure-email-action-group.png)

7. 在“添加操作组”窗格中，选择“确定”。

8. 对于警报电子邮件，你可以自定义电子邮件主题。 在“创建规则”下选择“自定义操作”，然后选择“电子邮件主题”  。

9. 完成后，请选择“创建警报规则”。

