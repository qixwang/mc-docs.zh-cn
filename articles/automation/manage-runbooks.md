---
title: 在 Azure 自动化中管理 Runbook
description: 本文介绍如何在 Azure 自动化中管理 Runbook。 它涵盖了基本操作并添加了一些最佳做法。
services: automation
ms.subservice: process-automation
origin.date: 02/14/2019
ms.date: 05/25/2020
ms.topic: conceptual
ms.openlocfilehash: e1314f1c6ab6c212aff92df5cd8f36535afc8932
ms.sourcegitcommit: 981a75a78f8cf74ab5a76f9e6b0dc5978387be4b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/22/2020
ms.locfileid: "83801140"
---
# <a name="manage-runbooks-in-azure-automation"></a>在 Azure 自动化中管理 Runbook

可以通过以下方法将 Runbook 添加到 Azure 自动化：创建新的 Runbook 或从文件中导入现有 Runbook。 本文介绍如何通过文件创建和导入 Runbook。

>[!NOTE]
>本文进行了更新，以便使用新的 Azure PowerShell Az 模块。 你仍然可以使用 AzureRM 模块，至少在 2020 年 12 月之前，它将继续接收 bug 修补程序。 若要详细了解新的 Az 模块和 AzureRM 兼容性，请参阅[新 Azure Powershell Az 模块简介](https://docs.microsoft.com/powershell/azure/new-azureps-module-az?view=azps-3.5.0)。 有关适用于混合 Runbook 辅助角色的 Az 模块安装说明，请参阅安装 [Azure PowerShell 模块](https://docs.microsoft.com/powershell/azure/install-az-ps?view=azps-3.5.0)。 

## <a name="create-a-runbook"></a>创建 runbook

使用 Azure 门户或 Windows PowerShell 在 Azure 自动化中创建一个新的 Runbook。 创建 Runbook 后，可以使用以下信息对其进行编辑：

* [在 Azure 自动化中编辑文本 Runbook](automation-edit-textual-runbook.md) 
* [了解适用于自动化 Runbook 的关键 Windows PowerShell 工作流概念](automation-powershell-workflow.md)
* [Azure 自动化中的图形创作](automation-graphical-authoring-intro.md)
* [在 Azure 自动化中管理 Python 2 程序包](python-packages.md)

### <a name="create-a-runbook-in-the-azure-portal"></a>在 Azure 门户中创建 Runbook

1. 在 Azure 门户中，打开自动化帐户。
2. 从中心内，在“过程自动化”下选择“Runbook”，打开 Runbook 列表。 
3. 单击“创建 Runbook”。
4. 为 Runbook 键入一个名称并选择其[类型](automation-runbook-types.md)。 Runbook 名称必须以字母开头，可以包含字母、数字、下划线和短划线。
5. 单击“创建”以创建 Runbook 并打开编辑器。

### <a name="create-a-runbook-with-powershell"></a>通过 PowerShell 创建 Runbook

使用 [AzAutomationRunbook](https://docs.microsoft.com/powershell/module/az.automation/new-azautomationrunbook?view=azps-3.5.0) cmdlet 创建空的 Runbook。 使用 `Type` 参数指定为 `New-AzAutomationRunbook` 定义的 runbook 类型之一。

以下示例演示了如何创建新的空 runbook。

```azurepowershell
New-AzAutomationRunbook -AutomationAccountName MyAccount `
-Name NewRunbook -ResourceGroupName MyResourceGroup -Type PowerShell
```

## <a name="import-a-runbook"></a>导入 Runbook

你可以导入 PowerShell 或 PowerShell 工作流 (.ps1) 脚本、图形Runbook (.graphrunbook) 或 Python 2 脚本 (.py) 来创建自己的 Runbook  。  必须指定在导入期间创建的 [Runbook 类型](automation-runbook-types.md)，并考虑以下注意事项。

* 不包含工作流的 .ps1 文件可以导入到 [PowerShell Runbook](automation-runbook-types.md#powershell-runbooks) 中，也可以导入到 [PowerShell 工作流 Runbook](automation-runbook-types.md#powershell-workflow-runbooks) 中。 如果将其导入到 PowerShell 工作流 Runbook 中，则它将转换为工作流。 在这种情况下，Runbook 中将包含注释来描述所做的更改。

* 只能将包含 PowerShell 工作流的 .ps1 文件导入到 [PowerShell 工作流 Runbook](automation-runbook-types.md#powershell-workflow-runbooks) 中。 如果该文件包含多个 PowerShell 工作流，则导入将失败。 必须将每个工作流保存到各自的文件中，并分别导入每个工作流。

* 包含 PowerShell 工作流的 .ps1 文件不应导入到 [PowerShell Runbook](automation-runbook-types.md#powershell-runbooks) 中，因为 PowerShell 脚本引擎无法识别该文件。

* 只将 .graphrunbook 文件导入到新的[图形 Runbook](automation-runbook-types.md#graphical-runbooks) 中。 

### <a name="import-a-runbook-from-the-azure-portal"></a>从 Azure 门户导入 Runbook

可通过以下过程将脚本文件导入 Azure 自动化。

> [!NOTE]
> 只能通过门户将 **.ps1** 文件导入 PowerShell 工作流 Runbook。

1. 在 Azure 门户中，打开自动化帐户。
2. 在“过程自动化”下选择“Runbook”，打开 Runbook 列表。 
3. 单击“导入 Runbook”。
4. 单击“Runbook 文件”，并选择要导入的文件。
5. 如果“名称”字段已启用，则可以更改 runbook 名称。 名称必须以字母开头，可以包含字母、数字、下划线和短划线。
6. 将自动选择 [Runbook 类型](automation-runbook-types.md)，但可以在考虑适用的限制后更改该类型。
7. 单击**创建**。 新的 runbook 会出现在自动化帐户的 runbook 列表中。
8. 必须先 [发布 Runbook](#publish-a-runbook) ，才能运行它。

> [!NOTE]
> 导入图形 Runbook 后，可以将它转换为其他类型。 但是，无法将图形 Runbook 转换为文本 Runbook。

### <a name="import-a-runbook-with-windows-powershell"></a>使用 Windows PowerShell 导入 Runbook

可以使用 [Import-AzAutomationRunbook](https://docs.microsoft.com/powershell/module/az.automation/import-azautomationrunbook?view=azps-3.5.0) cmdlet 将脚本文件导入为草稿 Runbook。 如果 Runbook 已存在，除非对该 cmdlet 使用 `Force` 参数，否则导入将失败。

以下示例演示了如何将脚本文件导入到 Runbook 中。

```azurepowershell
$automationAccountName =  "AutomationAccount"
$runbookName = "Sample_TestRunbook"
$scriptPath = "C:\Runbooks\Sample_TestRunbook.ps1"
$RGName = "ResourceGroup"

Import-AzAutomationRunbook -Name $runbookName -Path $scriptPath `
-ResourceGroupName $RGName -AutomationAccountName $automationAccountName `
-Type PowerShellWorkflow
```

## <a name="handle-resources"></a>处理资源

如果 Runbook 要创建某个[资源](automation-runbook-execution.md#resources)，在尝试创建该资源之前，脚本应检查该资源是否已存在。 下面是一个基本示例。

```powershell
$vmName = "WindowsVM1"
$resourceGroupName = "myResourceGroup"
$myCred = Get-AutomationPSCredential "MyCredential"
$vmExists = Get-AzureRmResource -Name $vmName -ResourceGroupName $resourceGroupName

if(!$vmExists)
    {
    Write-Output "VM $vmName does not exist, creating"
    New-AzureRmVM -Name $vmName -ResourceGroupName $resourceGroupName -Credential $myCred
    }
else
    {
    Write-Output "VM $vmName already exists, skipping"
    }
```

## <a name="retrieve-details-from-activity-log"></a>从活动日志中检索详细信息

可以从自动化帐户的[活动日志](automation-runbook-execution.md#activity-logging)中检索 Runbook 详细信息，例如启动该 Runbook 的人员或帐户。 以下 PowerShell 示例提供运行指定 Runbook 的最后一个用户。

```powershell
$SubID = "00000000-0000-0000-0000-000000000000"
$AutomationResourceGroupName = "MyResourceGroup"
$AutomationAccountName = "MyAutomationAccount"
$RunbookName = "MyRunbook"
$StartTime = (Get-Date).AddDays(-1)
$JobActivityLogs = Get-AzureRmLog -ResourceGroupName $AutomationResourceGroupName -StartTime $StartTime `
                                | Where-Object {$_.Authorization.Action -eq "Microsoft.Automation/automationAccounts/jobs/write"}

$JobInfo = @{}
foreach ($log in $JobActivityLogs)
{
    # Get job resource
    $JobResource = Get-AzureRmResource -ResourceId $log.ResourceId

    if ($JobInfo[$log.SubmissionTimestamp] -eq $null -and $JobResource.Properties.runbook.name -eq $RunbookName)
    {
        # Get runbook
        $Runbook = Get-AzureRmAutomationJob -ResourceGroupName $AutomationResourceGroupName -AutomationAccountName $AutomationAccountName `
                                            -Id $JobResource.Properties.jobId | ? {$_.RunbookName -eq $RunbookName}

        # Add job information to hashtable
        $JobInfo.Add($log.SubmissionTimestamp, @($Runbook.RunbookName,$Log.Caller, $JobResource.Properties.jobId))
    }
}
$JobInfo.GetEnumerator() | sort key -Descending | Select-Object -First 1
```

## <a name="track-progress"></a>跟踪进度

合理的做法是创作模块化的 Runbook，并使用能够轻松重用和重启的逻辑。 跟踪 Runbook 的进度可确保在出现问题时正常执行 Runbook 逻辑。 

可以使用存储帐户、数据库或共享文件等外部源跟踪 Runbook 的进度。 在你的 Runbook 中创建逻辑，以便先检查所执行的最后一个操作的状态。 然后，该逻辑可以根据检查结果跳过或继续执行 Runbook 中的特定任务。

## <a name="prevent-concurrent-jobs"></a>预防并发作业

某些 Runbook 在同时针对多个作业运行时会出现怪异的行为。 在此情况下，必须让 Runbook 实现相应的逻辑来确定是否已存在运行中的作业。 下面是一个基本示例。

```powershell
# Authenticate to Azure
$connection = Get-AutomationConnection -Name AzureRunAsConnection
Connect-AzureRmAccount -ServicePrincipal -Tenant $connection.TenantID `
-ApplicationId $connection.ApplicationID -CertificateThumbprint $connection.CertificateThumbprint -EnvironmentName AzureChinaCloud

$AzContext = Select-AzureRmSubscription -SubscriptionId $connection.SubscriptionID

# Check for already running or new runbooks
$runbookName = "<RunbookName>"
$rgName = "<ResourceGroupName>"
$aaName = "<AutomationAccountName>"
$jobs = Get-AzureRmAutomationJob -ResourceGroupName $rgName -AutomationAccountName $aaName -RunbookName $runbookName -AzContext $AzureContext

# Check to see if it is already running
$runningCount = ($jobs | ? {$_.Status -eq "Running"}).count

If (($jobs.status -contains "Running" -And $runningCount -gt 1 ) -Or ($jobs.Status -eq "New")) {
    # Exit code
    Write-Output "Runbook is already running"
    Exit 1
} else {
    # Insert Your code here
}
```

## <a name="handle-transient-errors-in-a-time-dependent-script"></a>在依赖时间的脚本中处理暂时性错误

Runbook 必须可靠，且能够处理[错误](automation-runbook-execution.md#errors)，包括可能导致它们重启或失败的暂时性错误。 如果 Runbook 失败，Azure 自动化会重试该 Runbook。

如果 Runbook 在时间约束范围内正常运行，请在脚本中实现检查执行时间的逻辑。 此项检查可确保只在特定时间运行启动、关闭或横向扩展等操作。

> [!NOTE]
> Azure 沙盒进程上的本地时间设置为 UTC 时间。 Runbook 中的日期和时间计算需要考虑到这一点。
使用自定义脚本：

1. 创建自动化帐户并获取[参与者角色](automation-role-based-access-control.md)。
2. [将帐户链接到 Azure 工作区](../security-center/security-center-enable-data-collection.md)。
3. 启用[混合 Runbook 辅助角色](automation-hybrid-runbook-worker.md)、[更新管理](automation-update-management.md)或其他自动化功能。 
4. 如果使用的是 Linux 计算机，则需要较高的权限。 登录以[关闭签名检查](automation-linux-hrw-install.md#turn-off-signature-validation)。

## <a name="test-a-runbook"></a>测试 Runbook

测试 Runbook 时，将执行[草稿版](#publish-a-runbook)，并会完成其执行的任何操作。 不会创建作业历史记录，但会在“测试输出”窗格中显示[“输出”](automation-runbook-output-and-messages.md#output-stream)与[“警告和错误”](automation-runbook-output-and-messages.md#message-streams)流。 仅当 [VerbosePreference](automation-runbook-output-and-messages.md#preference-variables) 变量设置为 `Continue` 时，发送到[详细流](automation-runbook-output-and-messages.md#message-streams)的消息才会显示在“输出”窗格中。

即使草稿版正在运行，该 Runbook 也仍会正常执行，并针对环境中的资源执行任何操作。 因此，只能在非生产资源中测试 Runbook。

测试每种 [Runbook 类型](automation-runbook-types.md)的过程是相同的。 并且，在 Azure 门户中使用文本编辑器进行测试和使用图形编辑器进行测试没有区别。

1. 在[文本编辑器](automation-edit-textual-runbook.md)或[图形编辑器](automation-graphical-authoring-intro.md)中打开 Runbook 的草稿版本。
1. 单击“测试”打开“测试”页。
1. 如果 Runbook 有参数，参数会在左窗格中列出，可以在其中提供用于测试的值。
1. 如果要在[混合 Runbook 辅助角色](automation-hybrid-runbook-worker.md)上运行测试，则将“运行设置”更改为“混合辅助角色”并选择目标组的名称。  **Azure** ，以在云中运行测试。
1. 单击“启动”开始测试。
1. 在测试期间，可以使用“输出”窗格下方的按钮来停止或暂停 [PowerShell 工作流](automation-runbook-types.md#powershell-workflow-runbooks)或[图形](automation-runbook-types.md#graphical-runbooks) runbook。 暂停 Runbook 时，该 Runbook 会完成它在被暂停之前正在进行的活动。 暂停 Runbook 后，可以将它停止或重启。
1. 在输出窗格中检查 Runbook 的输出。

## <a name="publish-a-runbook"></a>发布 Runbook

创建或导入新的 Runbook 时，必须先将其发布，才能导入。 Azure 自动化中的每个 Runbook 都有草稿版和已发布版。 只有已发布版才能用来运行，只有草稿版才能用来编辑。 已发布版不受对草稿版所做的任何更改的影响。 当草稿版可以使用时，可以发布草稿版，这样草稿版就会覆盖当前已发布版。

### <a name="publish-a-runbook-in-the-azure-portal"></a>在 Azure 门户中发布 Runbook

1. 在 Azure 门户中打开 Runbook。
2. 单击“编辑”。
3. 单击“发布”，然后单击“是”来响应验证消息。

### <a name="publish-a-runbook-using-powershell"></a>使用 PowerShell 发布 Runbook

使用 [AzAutomationRunbook](https://docs.microsoft.com/powershell/module/Az.Automation/Publish-AzAutomationRunbook?view=azps-3.5.0) cmdlet 发布 Runbook。 

```azurepowershell
$automationAccountName =  "AutomationAccount"
$runbookName = "Sample_TestRunbook"
$RGName = "ResourceGroup"

Publish-AzAutomationRunbook -AutomationAccountName $automationAccountName `
-Name $runbookName -ResourceGroupName $RGName
```

## <a name="schedule-a-runbook-in-the-azure-portal"></a>在 Azure 门户中计划 Runbook

发布 runbook 后，你可以计划其操作：

1. 在 Azure 门户中打开 Runbook。
2. 在“资源”下选择“计划” 。
3. 选择“添加计划”。
4. 在“计划 Runbook”窗格中，选择“将计划链接到 Runbook”。
5. 在“计划”窗格中选择“新建计划”。
6. 在“新建计划”窗格中输入名称、说明和其他参数。 
7. 创建计划后，突出显示该计划，然后单击“确定”。 现在它应该会链接到你的 Runbook。
8. 在邮箱中，查找通知你 Runbook 状态的电子邮件。

## <a name="obtain-job-statuses"></a>获取作业状态

### <a name="view-statuses-in-the-azure-portal"></a>在 Azure 门户中查看状态

[作业](automation-runbook-execution.md#jobs)中提供了 Azure 自动化中作业处理的详细信息。 如果已准备好查看 Runbook 作业，请使用 Azure 门户并访问你的自动化帐户。 在右侧的“作业统计信息”中，可以看到所有 Runbook 作业的摘要。 

![作业统计信息磁贴](./media/manage-runbooks/automation-account-job-status-summary.png)

摘要显示执行的每个作业的计数及其状态的图形表示形式。

单击磁贴可显示“作业”页，此页包括所有已执行作业的摘要列表。 此页显示每个作业的状态、Runbook 名称、开始时间和完成时间。

![自动化帐户作业页](./media/manage-runbooks/automation-account-jobs-status-blade.png)

可以选择“筛选作业”来筛选作业列表。 可按特定的 Runbook 和作业状态进行筛选，或者从下拉列表中进行选择，并提供搜索时间范围。

![筛选作业状态](./media/manage-runbooks/automation-account-jobs-filter.png)

或者，可通过从自动化帐户中的“Runbook”页上选择特定的 Runbook，并选择“作业”，来查看该 Runbook 的作业摘要详细信息。 此操作将打开“作业”页。 在此页中，可以单击作业记录以查看其详细信息和输出。

![自动化帐户作业页](./media/manage-runbooks/automation-runbook-job-summary-blade.png)

### <a name="retrieve-job-statuses-using-powershell"></a>使用 PowerShell 检索作业状态

可以使用 [Get-AzAutomationJob](https://docs.microsoft.com/powershell/module/Az.Automation/Get-AzAutomationJob?view=azps-3.7.0) cmdlet 检索为 Runbook 创建的作业和特定作业的详细信息。 如果使用 `Start-AzAutomationRunbook` 启动 Runbook，则会返回生成的作业。 可以使用 [Get-AzAutomationJobOutput](https://docs.microsoft.com/powershell/module/Az.Automation/Get-AzAutomationJobOutput?view=azps-3.5.0) 检索作业输出。

以下示例获取某个示例 Runbook 的最后一个作业，并显示其状态、为 Runbook 参数提供的值以及该作业的输出。

```azurepowershell
$job = (Get-AzAutomationJob -AutomationAccountName "MyAutomationAccount" `
-RunbookName "Test-Runbook" -ResourceGroupName "ResourceGroup01" | sort LastModifiedDate -desc)[0]
$job.Status
$job.JobParameters
Get-AzAutomationJobOutput -ResourceGroupName "ResourceGroup01" `
-AutomationAccountName "MyAutomationAcct" -Id $job.JobId -Stream Output
```

以下示例检索特定作业的输出并返回每个记录。 如果某个记录存在[异常](automation-runbook-execution.md#exceptions)，脚本会写入该异常而不是值。 此行为非常有用，因为异常可以提供在输出过程中正常情况下可能无法记录的其他信息。

```azurepowershell
$output = Get-AzAutomationJobOutput -AutomationAccountName <AutomationAccountName> -Id <jobID> -ResourceGroupName <ResourceGroupName> -Stream "Any"
foreach($item in $output)
{
    $fullRecord = Get-AzAutomationJobOutputRecord -AutomationAccountName <AutomationAccountName> -ResourceGroupName <ResourceGroupName> -JobId <jobID> -Id $item.StreamRecordId
    if ($fullRecord.Type -eq "Error")
    {
        $fullRecord.Value.Exception
    }
    else
    {
    $fullRecord.Value
    }
}
```

## <a name="next-steps"></a>后续步骤

* 若要了解 runbook 处理，请参阅 [Azure 自动化中的 Runbook 执行](automation-runbook-execution.md)。
* 若要深入了解如何使用文本编辑器编辑 PowerShell 和 PowerShell 工作流 Runbook，请参阅[在 Azure 自动化中编辑文本 Runbook](automation-edit-textual-runbook.md)。
* 若要详细了解图形 Runbook 创作，请参阅 [Azure 自动化中的图形创作](automation-graphical-authoring-intro.md)。
