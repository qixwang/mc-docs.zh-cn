---
title: 管理 Azure 自动化中的计划
description: 了解如何在 Azure 自动化中创建和管理计划，以便在特定的时间或按重复计划自动启动 Runbook。
services: automation
ms.service: automation
ms.subservice: shared-capabilities
author: WenJason
ms.author: v-jay
origin.date: 04/04/2019
ms.date: 05/18/2020
ms.topic: conceptual
manager: digimobile
ms.openlocfilehash: bc0bf727fbc7d8228d3c16792272e5761920c65a
ms.sourcegitcommit: 981a75a78f8cf74ab5a76f9e6b0dc5978387be4b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/22/2020
ms.locfileid: "83801290"
---
# <a name="manage-schedules-in-azure-automation"></a>管理 Azure 自动化中的计划

要将 Azure 自动化中的 Runbook 计划为在指定的时间启动，可以将它链接到一个或多个计划。 可以在 Azure 门户中为 Runbook 配置计划，使其运行一次或者每小时或每日重复运行。 还可以将它们计划为每周、每月、每周的特定几天或每月的特定几天或每月的特定一天运行。 可将一个 Runbook 链接到多个计划，一个计划可以链接多个 Runbook。

> [!NOTE]
> 目前计划不支持 Azure Automation DSC 配置。

## <a name="powershell-cmdlets-used-to-access-schedules"></a>用于访问计划的 PowerShell cmdlet

下表中的 cmdlet 通过 PowerShell 创建和管理自动化计划。 它们作为 [Azure PowerShell 模块](https://docs.microsoft.com/powershell/azure/overview)的一部分提供。

| Cmdlet | 说明 |
|:--- |:--- |
| [Get-AzureRmAutomationSchedule](https://docs.microsoft.com/powershell/module/azurerm.automation/get-azurermautomationschedule) |检索计划。 |
| [New-AzureRmAutomationSchedule](https://docs.microsoft.com/powershell/module/azurerm.automation/new-azurermautomationschedule) |创建新计划。 |
| [Remove-AzureRmAutomationSchedule](https://docs.microsoft.com/powershell/module/azurerm.automation/remove-azurermautomationschedule) |删除计划。 |
| [Set-AzureRmAutomationSchedule](https://docs.microsoft.com/powershell/module/azurerm.automation/set-azurermautomationschedule) |设置现有计划的属性。 |
| [Get-AzureRmAutomationScheduledRunbook](https://docs.microsoft.com/powershell/module/azurerm.automation/get-azurermautomationscheduledrunbook) |检索计划 Runbook。 |
| [Register-AzureRmAutomationScheduledRunbook](https://docs.microsoft.com/powershell/module/azurerm.automation/register-azurermautomationscheduledrunbook) |将 Runbook 与计划相关联。 |
| [Unregister-AzureRmAutomationScheduledRunbook](https://docs.microsoft.com/powershell/module/azurerm.automation/unregister-azurermautomationscheduledrunbook) |将 Runbook 与计划取消关联。 |

## <a name="create-a-schedule"></a>创建计划

可以使用 Azure 门户或 PowerShell 为 Runbook 创建新计划。 若要避免影响 Runbook 以及其自动执行进程，应首先使用测试专用自动化帐户测试已链接计划的任何 Runbook。 测试将会验证计划的 Runbook 是否继续正常运行。 如果出现问题，可以进行排除故障并应用所需的任何更改，然后再将更新的 Runbook 版本迁移到生产环境。

### <a name="create-a-new-schedule-in-the-azure-portal"></a>在 Azure 门户中创建新计划

1. 在 Azure 门户中，从你的自动化帐户中，从左侧的“共享资源”部分下选择“计划”。 
1. 选择页面顶部的“添加计划”。
1. 在“新建计划”窗格中，输入新计划的名称和可选说明。
1. 通过选择“一次”或“重复”来选择该计划是运行一次，还是按计划重复运行 。 如果选择“一次”，请指定开始时间，然后选择“创建” 。 如果选择“重复”，请指定开始时间。 对于“重复间隔”，请选择想要 Runbook 重复运行的频率。 请选择按小时、天、周或月运行。
    1. 如果选择“周”，系统会显示星期供你选择。 根据需要选择天数。 计划的第一次运行将在开始时间之后选择的第一天进行。 例如，若要选择周末计划，请选择“星期六”和“星期日”。
    
       ![设置周末重复计划](../media/schedules/week-end-weekly-recurrence.png)

    2. 如果选择“月”，则会看到不同的选项。 对于“每月进行次数”选项，请选择“每月天数”或“每周天数”  。 如果选择“月份日期”，则会显示一个日历供你选择所需的天数。 如果选择当月不存在的日期（例如 31 日），则计划将不会运行。 如果希望计划在最后一天运行，请在“在月份的最后一天运行”下选择“是” 。 如果选择“星期日期”，则会显示“重复间隔” 。 选择“第一”、“第二”、“第三”、“第四”或“最后”    。 最后选择一天进行重复。

       ![在每月 1 号、15 号和最后一天运行的计划](../media/schedules/monthly-first-fifteenth-last.png)

1. 完成后，选择“创建”。

### <a name="create-a-new-schedule-with-powershell"></a>使用 PowerShell 创建新计划

可使用 [New-AzureRmAutomationSchedule](https://docs.microsoft.com/powershell/module/azurerm.automation/new-azurermautomationschedule) cmdlet 创建计划。 必须指定计划的开始时间以及运行频率。 以下示例展示了如何创建许多不同的计划方案。

#### <a name="create-a-one-time-schedule"></a>创建一次性计划

以下示例创建一次性计划。

```azurepowershell
$TimeZone = ([System.TimeZoneInfo]::Local).Id
New-AzureRmAutomationSchedule -AutomationAccountName "ContosoAutomation" -Name "Schedule01" -StartTime "23:00" -OneTime -ResourceGroupName "ResourceGroup01" -TimeZone $TimeZone
```

#### <a name="create-a-recurring-schedule"></a>创建重复计划

以下示例演示如何创建在一年中的每天下午 1:00 运行的重复计划。

```azurepowershell
$StartTime = Get-Date "13:00:00"
$EndTime = $StartTime.AddYears(1)
New-AzureRmAutomationSchedule -AutomationAccountName "ContosoAutomation" -Name "Schedule02" -StartTime $StartTime -ExpiryTime $EndTime -DayInterval 1 -ResourceGroupName "ResourceGroup01"
```

#### <a name="create-a-weekly-recurring-schedule"></a>创建每周重复计划

以下示例演示如何创建仅在工作日运行的每周计划。

```azurepowershell
$StartTime = (Get-Date "13:00:00").AddDays(1)
[System.DayOfWeek[]]$WeekDays = @([System.DayOfWeek]::Monday..[System.DayOfWeek]::Friday)
New-AzureRmAutomationSchedule -AutomationAccountName "ContosoAutomation" -Name "Schedule03" -StartTime $StartTime -WeekInterval 1 -DaysOfWeek $WeekDays -ResourceGroupName "ResourceGroup01"
```

#### <a name="create-a-weekly-recurring-schedule-for-weekends"></a>创建在工作日运行的每周重复计划

以下示例演示如何创建仅在周末运行的每周计划。

```azurepowershell
$StartTime = (Get-Date "18:00:00").AddDays(1)
[System.DayOfWeek[]]$WeekendDays = @([System.DayOfWeek]::Saturday,[System.DayOfWeek]::Sunday)
New-AzureRmAutomationSchedule -AutomationAccountName "ContosoAutomation" -Name "Weekends 6PM" -StartTime $StartTime -WeekInterval 1 -DaysOfWeek $WeekendDays -ResourceGroupName "ResourceGroup01"
```

#### <a name="create-a-recurring-schedule-for-the-first-fifteenth-and-last-days-of-the-month"></a>创建在当月 1 号、15 号和最后一天运行的重复计划

以下示例演示如何创建在某月的 1 号、15 号和最后一天运行的重复计划。

```azurepowershell
$StartTime = (Get-Date "18:00:00").AddDays(1)
New-AzureRmAutomationSchedule -AutomationAccountName "TestAzureAuto" -Name "1st, 15th and Last" -StartTime $StartTime -DaysOfMonth @("One", "Fifteenth", "Last") -ResourceGroupName "TestAzureAuto" -MonthInterval 1
```

## <a name="link-a-schedule-to-a-runbook"></a>将计划链接到 Runbook

可将一个 Runbook 链接到多个计划，一个计划可以链接多个 Runbook。 如果 Runbook 包含参数，你可以提供这些参数的值。 必须为所有必需参数提供值，并可以为任何可选参数提供值。 此计划每次启动 Runbook 时，都将使用这些值。 可以将同一个 Runbook 附加到另一个计划，并指定不同的参数值。

### <a name="link-a-schedule-to-a-runbook-with-the-azure-portal"></a>使用 Azure 门户将计划链接到 Runbook

1. 在 Azure 门户中，从你的自动化帐户中，选择“流程自动化”下的“Runbook” 。
1. 选择要计划的 Runbook 的名称。
1. 如果 Runbook 当前未链接到计划，则系统会提供“创建新计划”或“链接到现有计划”选项。
1. 如果 Runbook 包含参数，可以选择选项“修改运行设置(默认值: Azure)”，此时会显示“参数”窗格 。 可在此窗格中输入参数信息。

### <a name="link-a-schedule-to-a-runbook-with-powershell"></a>使用 PowerShell 将计划链接到 Runbook

使用 [Register-AzureRmAutomationScheduledRunbook](https://docs.microsoft.com/powershell/module/azurerm.automation/register-azurermautomationscheduledrunbook) cmdlet 链接计划。 可以使用 Parameters 参数为 Runbook 的参数指定值。 有关如何指定参数值的详细信息，请参阅[在 Azure 自动化中启动 Runbook](../automation-starting-a-runbook.md)。
以下示例演示如何使用带参数的 Azure 资源管理器 cmdlet 将计划链接到 Runbook。

```azurepowershell
$automationAccountName = "MyAutomationAccount"
$runbookName = "Test-Runbook"
$scheduleName = "Sample-DailySchedule"
$params = @{"FirstName"="Joe";"LastName"="Smith";"RepeatCount"=2;"Show"=$true}
Register-AzureRmAutomationScheduledRunbook –AutomationAccountName $automationAccountName `
–Name $runbookName –ScheduleName $scheduleName –Parameters $params `
-ResourceGroupName "ResourceGroup01"
```

## <a name="schedule-runbooks-to-run-more-frequently"></a>将 Runbook 计划为更频率地运行

可为 Azure 自动化中的计划配置的最频繁间隔为一小时。 如果需要按更频繁的计划运行，可采用两种做法：

* 为 Runbook 创建 [Webhook](../automation-webhooks.md)，并使用 [Azure 逻辑应用](../../logic-apps/logic-apps-overview.md)调用 Webhook。 Azure 逻辑应用提供更精细的粒度来定义计划。

* 创建 4 个计划，它们全都相隔 15 分钟启动，并每隔一小时运行一次。 此方案使用不同的计划，可让 Runbook 每隔 15 分钟运行一次。

## <a name="disable-a-schedule"></a>禁用计划

禁用计划后，链接到该计划的所有 Runbook 不再按该计划运行。 可以手动禁用计划，也可以在创建带频率的计划时设置其过期时间。 到达过期时间时，会禁用该计划。

### <a name="disable-a-schedule-from-the-azure-portal"></a>从 Azure 门户禁用计划

1. 在你的自动化帐户中，选择“共享资源”下的“计划” 。
1. 选择某个计划的名称以打开详细信息窗格。
1. 将“已启用”更改为“否” 。

> [!NOTE]
> 若要禁用开始时间已过去的计划，必须将开始日期更改为将来的某个时间，然后保存计划。

### <a name="disable-a-schedule-with-powershell"></a>使用 PowerShell 禁用计划

使用 [Set-AzureRmAutomationSchedule](https://docs.microsoft.com/powershell/module/azurerm.automation/set-azurermautomationschedule) cmdlet 更改现有计划的属性。 若要禁用计划，请为 `IsEnabled` 参数指定 False。

以下示例演示如何使用 Azure 资源管理器 cmdlet 禁用 Runbook 的计划。

```powershell
$automationAccountName = "MyAutomationAccount"
$scheduleName = "Sample-MonthlyDaysOfMonthSchedule"
Set-AzureRmAutomationSchedule –AutomationAccountName $automationAccountName `
–Name $scheduleName –IsEnabled $false -ResourceGroupName "ResourceGroup01"
```

## <a name="remove-a-schedule"></a>删除计划

准备好删除计划时，可以使用 Azure 门户或 PowerShell 将其删除。 请记住，只能删除已按前一部分中所述禁用的计划。

### <a name="remove-a-schedule-using-the-azure-portal"></a>使用 Azure 门户删除计划

1. 在你的自动化帐户中，选择“共享资源”下的“计划” 。
2. 单击某个计划的名称以打开详细信息窗格。
3. 单击“删除” 。

### <a name="remove-a-schedule-with-powershell"></a>使用 PowerShell 删除计划

可按如下所示使用 `Remove-AzAutomationSchedule` cmdlet 删除现有计划。 

```azurepowershell
$automationAccountName = "MyAutomationAccount"
$scheduleName = "Sample-MonthlyDaysOfMonthSchedule"
Remove-AzAutomationSchedule -AutomationAccountName $automationAccountName `
-Name $scheduleName -ResourceGroupName "ResourceGroup01"
```

## <a name="next-steps"></a>后续步骤

* 若要了解关于 Runbook 的常规信息，请参阅 [Azure 自动化中的 Runbook 执行](../automation-runbook-execution.md)。
