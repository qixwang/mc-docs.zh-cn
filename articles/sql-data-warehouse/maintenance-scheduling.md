---
title: Azure 维护计划
description: 维护计划使客户能够围绕 Azure SQL 数据仓库服务用于推出新功能、升级和修补程序的必要计划性维护事件进行规划。
services: sql-data-warehouse
author: WenJason
manager: digimobile
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.subservice: design
origin.date: 11/07/2019
ms.date: 12/09/2019
ms.author: v-jay
ms.reviewer: jrasnick
ms.openlocfilehash: f87b3ca69af73cb47106f2ea1c908ceda2dff85c
ms.sourcegitcommit: c1ba5a62f30ac0a3acb337fb77431de6493e6096
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2020
ms.locfileid: "74884944"
---
# <a name="use-maintenance-schedules-to-manage-service-updates-and-maintenance"></a>使用维护计划管理服务更新和维护

此维护计划功能可集成服务运行状况计划内维护通知、资源运行状况检查监视器和 Azure SQL 数据仓库维护计划服务。

应使用维护计划选择一个方便接收新功能、升级和修补程序的时间范围。 你将需要选择 7 天内的主要和辅助维护时段，每个时段必须在单独的日期范围内。

例如，可以这样计划：主要时段为星期六 22:00 到星期日 01:00，辅助时段为星期三 19:00 到 22:00。 如果 SQL 数据仓库在主要维护时段无法执行维护，则它会尝试在辅助维护时段再次执行维护。 在主要时段和辅助时段期间都可能会进行服务维护。 为了确保快速完成所有维护操作，DW400c 和更低的数据仓库层可以在指定的维护时段之外完成维护。

所有新创建的 Azure SQL 数据仓库实例将在部署期间应用系统定义的维护计划。 部署完成后即可编辑该计划。

虽然维护时段可以是 3 到 8 小时，但这并不意味着数据仓库会在这段时间内脱机。 我们可能会在该时段内的任何时间进行维护，你应该预料到在该时段内会出现一次断开连接的情况，持续时间大约为 5 到 6 分钟，因为服务需将新代码部署到数据仓库。 在维护时段，DW400c 及更低的数据仓库层可能会多次出现短暂的连接丢失情况。 维护开始时，将取消所有活动会话，并回退未提交的事务。 若要尽量减少实例停机时间，请确保数据仓库在选定的维护时限之前没有任何长时间运行的事务。

所有维护操作都会在指定的维护时段内完成，除非我们必须部署时间敏感型更新。 如果在计划维护期间暂停数据仓库，则会在恢复操作期间进行更新。 数据仓库维护完成后，你会立即收到通知。

## <a name="alerts-and-monitoring"></a>警报和监视

与服务运行状况通知和资源运行状况检查监视器的集成可让客户随时了解即将发生的维护活动。 此自动化利用 Azure Monitor。 你可以决定如何接收有关即将进行的维护事件的通知。 此外，可以选择自动化流来管理停机时间，尽量减少操作影响。
我们会在所有维护事件之前提前 24 小时通知，DWC400c 和更低层例外。

> [!NOTE]
> 在我们需要部署时间关键型更新的情况下，提前通知时间可能会显著减少。

如果提前收到了维护通知，但 SQL 数据仓库无法在此期间执行维护，则你会收到取消通知。 随即会在下一个计划的维护期间继续进行维护。

所有活动维护事件显示在“服务运行状况 - 计划内维护”部分中。  服务运行状况历史记录包括以往事件的完整记录。 可以在活动事件期间通过 Azure 服务运行状况检查门户仪表板监视维护。

### <a name="maintenance-schedule-availability"></a>维护计划可用性

即使维护计划在所选的区域中不可用，也随时可以查看和编辑维护计划。 维护计划在你的区域中可用后，指定的计划将立即针对你的数据仓库生效。

## <a name="view-a-maintenance-schedule"></a>查看维护计划 

### <a name="portal"></a>门户

默认情况下，所有新创建的 Azure SQL 数据仓库实例在部署期间会有八小时的主维护时段和辅助维护时段。 如上所述，部署完成后，你可以更改此时段。 在未事先通知的情况下，不会在指定的维护时段外进行维护。

若要查看已应用于你的数据仓库的维护计划，请完成以下步骤：

1. 登录 [Azure 门户](https://portal.azure.cn/)。
2. 选择要查看的数据仓库。 
3. 所选的数据仓库将在“概述”边栏选项卡上打开。 应用于该数据仓库的维护计划将显示在“维护计划”下方。 

![概述边栏选项卡](media/sql-data-warehouse-maintenance-scheduling/clear-overview-blade.PNG)

## <a name="change-a-maintenance-schedule"></a>更改维护计划 

### <a name="portal"></a>门户
可以在任何时间更新或更改维护计划。 如果所选的实例正在经历活动维护周期，将保存设置。 在下一个确定的维护周期，它们将处于活动状态。 [详细了解](https://docs.microsoft.com/azure/service-health/resource-health-overview)在活动维护事件期间监视数据仓库。 

### <a name="identifying-the-primary-and-secondary-windows"></a>确定主要和辅助时段

主要和辅助时段必须包含不同的日期范围。 例如，主要时段为星期二到星期四，辅助时段为星期六到星期日。

若要更改数据仓库的维护计划，请完成以下步骤：
1. 登录 [Azure 门户](https://portal.azure.cn/)。
2. 选择要更新的数据仓库。 页面将在概述边栏选项卡上打开。 
3. 在概述边栏选项卡上选择“维护计划(预览版)摘要”链接，打开维护计划设置的页面。  或者，选择左侧资源菜单中的“维护计划”选项。   

    ![概述边栏选项卡选项](media/sql-data-warehouse-maintenance-scheduling/maintenance-change-option.png)

4. 使用页面顶部的选项确定主要维护时段的首选日期范围。 此选择确定主要时段会出现在工作日还是周末。 所做的选择会更新下拉列表值。 在预览期，某些区域可能尚不支持完整的可用“日期”选项集。 

   ![维护设置边栏选项卡](media/sql-data-warehouse-maintenance-scheduling/maintenance-settings-page.png)

5. 使用下拉列表框选择首选的主要和辅助维护时段：
   - **日期**：在所选时段内执行维护的首选日期。
   - **开始时间**：维护时段的首选开始时间。
   - **时间范围**：时间范围的首选持续时间。

   边栏选项卡底部的“计划摘要”区域将根据所选的值更新。  
  
6. 选择“保存”。  此时会显示一条消息，确认新计划现已处于活动状态。 

   如果要在不支持维护计划的区域中保存计划，则会显示以下消息。 当此功能在所选区域中可用时，设置将会保存并且变为活动状态。    

   ![有关区域可用性的消息](media/sql-data-warehouse-maintenance-scheduling/maintenance-notactive-toast.png)

## <a name="next-steps"></a>后续步骤
- [详细了解](/azure-monitor/platform/alerts-metric)如何使用 Azure Monitor 创建、查看和管理警报。
- [详细了解](/azure-monitor/platform/alerts-log-webhook)用于日志警报规则的 Webhook 操作。
- [深入了解](/monitoring-and-diagnostics/monitoring-action-groups)创建和管理操作组。
- [详细了解](/service-health/service-health-overview) Azure 服务运行状况。
