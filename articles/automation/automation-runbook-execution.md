---
title: 在 Azure 自动化中执行 Runbook
description: 详细介绍如何处理 Azure 自动化中的 Runbook。
services: automation
ms.subservice: process-automation
origin.date: 04/04/2019
ms.date: 03/30/2020
ms.topic: conceptual
ms.openlocfilehash: ec410c7e4958f874e60abdb29d3ebd5a29154e61
ms.sourcegitcommit: c1ba5a62f30ac0a3acb337fb77431de6493e6096
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2020
ms.locfileid: "80290353"
---
# <a name="runbook-execution-in-azure-automation"></a>在 Azure 自动化中执行 Runbook

Runbook 基于其内部定义的逻辑执行。 如果 Runbook 中断，则 Runbook 将在开始时重启。 这种行为要求编写的 Runbook 在发生暂时性问题时支持重启。

在 Azure 自动化中启动 Runbook 会创建一个作业。 作业是 Runbook 的单一执行实例。 每个作业可以通过与 Azure 订阅建立连接来访问 Azure 资源。 仅当数据中心内的资源可从云访问时，作业才能访问这些资源。

在执行 Runbook 期间，Azure 自动化将分配一个辅助角色来运行每个作业。 尽管辅助角色由多个 Azure 帐户共享，但不同自动化帐户中的作业是相互独立的。 无法控制要由哪个辅助角色为作业请求提供服务。

在 Azure 门户中查看 Runbook 列表时，会显示针对每个 Runbook 启动的作业的状态。 Azure 自动化将作业日志最多存储 30 天。 

下图显示 [PowerShell Runbook](automation-runbook-types.md#powershell-runbooks)、[图形 Runbook](automation-runbook-types.md#graphical-runbooks) 和 [PowerShell 工作流 Runbook](automation-runbook-types.md#powershell-workflow-runbooks) 的 Runbook 作业生命周期。

![作业状态 - PowerShell 工作流](./media/automation-runbook-execution/job-statuses.png)

[!INCLUDE [GDPR-related guidance](../../includes/gdpr-dsr-and-stp-note.md)]

## <a name="where-to-run-your-runbooks"></a>运行 runbook 的位置

Azure 自动化中的 Runbook 可以在 Azure 沙盒上运行，也可以在[混合 Runbook 辅助角色](automation-hybrid-runbook-worker.md)上运行。 大多数 Runbook 可以轻松在 Azure 沙盒中运行。沙盒是多个作业可以使用的共享环境。 使用同一沙盒的作业受沙盒的资源限制约束。

使用混合 Runbook 辅助角色既可以直接在托管角色的计算机上运行 Runbook，也可以对环境中的本地资源运行 Runbook。 Azure 自动化将存储并管理 Runbook，然后将其传送到一台或多台指定的计算机。

下表列出了一些 Runbook 执行任务，以及每个任务的建议执行环境。

|任务|最佳选择|说明|
|---|---|---|
|与 Azure 资源集成|Azure 沙盒|托管在 Azure 中，身份验证更为简单。 如果使用的是 Azure VM 上的混合 Runbook 辅助角色，则可使用 [Azure 资源的托管标识](automation-hrw-run-runbooks.md#managed-identities-for-azure-resources)。|
|管理 Azure 资源时可以获得最佳性能|Azure 沙盒|脚本在同一环境中运行，因此延迟更低。|
|最大程度减少运营成本|Azure 沙盒|没有计算开销，不需要 VM。|
|执行长时间运行的脚本|混合 Runbook 辅助角色|Azure 沙盒[对资源施加限制](../azure-resource-manager/management/azure-subscription-service-limits.md#automation-limits)。|
|与本地服务交互|混合 Runbook 辅助角色|可以直接访问主机。|
|需要第三方软件和可执行文件|混合 Runbook 辅助角色|由你管理操作系统，可以安装软件。|
|运行资源密集型脚本|混合 Runbook 辅助角色| Azure 沙盒[对资源施加限制](../azure-resource-manager/management/azure-subscription-service-limits.md#automation-limits)。|
|使用存在特定要求的模块| 混合 Runbook 辅助角色|一些示例如下：</br> WinSCP - winscp.exe 上的依赖项 </br> IISAdministration - 依赖于启用 IIS。|
|使用安装程序安装模块|混合 Runbook 辅助角色|沙盒的模块必须支持复制。|
|使用需要不同于 4.7.2 版本的 .NET Framework 的 Runbook 或模块|混合 Runbook 辅助角色|自动化沙盒使用 .NET Framework 4.7.2，无法升级 .NET Framework。|
|运行需要提升权限的脚本|混合 Runbook 辅助角色|沙盒不允许提升权限。 使用混合 Runbook 辅助角色可以关闭 UAC，并在运行需要提升权限的命令时使用 **Invoke-Command**。|
|运行需要访问 WMI 的脚本|混合 Runbook 辅助角色|在云中的沙盒中运行的作业无权访问 WMI。 |

## <a name="runbook-behavior"></a>Runbook 行为

### <a name="creating-resources"></a>创建资源

如果 Runbook 要创建某个资源，在尝试创建该资源之前，脚本应检查该资源是否已存在。 下面是一个基本示例。

```powershell
$vmName = "WindowsVM1"
$resourceGroupName = "myResourceGroup"
$myCred = Get-AutomationPSCredential "MyCredential"
$vmExists = Get-AzureRmResource -Name $vmName -ResourceGroupName $resourceGroupName

if(!$vmExists)
    {
    Write-Output "VM $vmName does not exist, creating"
    New-AzureRMVM -Name $vmName -ResourceGroupName $resourceGroupName -Credential $myCred
    }
else
    {
    Write-Output "VM $vmName already exists, skipping"
    }
```

### <a name="supporting-time-dependent-scripts"></a>支持时间依赖脚本

Runbook 必须可靠，且能够处理可能导致它们重启或失败的暂时性错误。 如果 Runbook 失败，Azure 自动化会重试该 Runbook。

如果 Runbook 在时间约束范围内正常运行，请在脚本中实现检查执行时间的逻辑。 此项检查可确保只在特定时间运行启动、关闭或横向扩展等操作。

> [!NOTE]
> Azure 沙盒进程上的本地时间设置为 UTC 时间。 Runbook 中的日期和时间计算需要考虑到这一点。

### <a name="tracking-progress"></a>跟踪进程

合理的做法是创作模块性的 Runbook，并构建 Runbook 逻辑，以便能够轻松重用和重启 Runbook。 跟踪 Runbook 的进度能够很好地确保在出现问题时正常执行 Runbook 逻辑。 可以使用存储帐户、数据库或共享文件等外部源跟踪 Runbook 的进度。 可以在 Runbook 中创建逻辑，以便先检查所执行的最后一个操作的状态。 然后，该逻辑可以根据检查结果跳过或继续执行 Runbook 中的特定任务。

### <a name="preventing-concurrent-jobs"></a>防止并发作业

某些 Runbook 在同时针对多个作业运行时会出现怪异的行为。 在此情况下，必须让 Runbook 实现相应的逻辑来确定是否已存在运行中的作业。 下面是一个基本示例。

```powershell
# Authenticate to Azure
$connection = Get-AutomationConnection -Name AzureRunAsConnection
Connect-AzureRmAccount -EnvironmentName AzureChinaCloud -ServicePrincipal -Tenant $connection.TenantID `
-ApplicationId $connection.ApplicationID -CertificateThumbprint $connection.CertificateThumbprint

$AzureContext = Select-AzureRmSubscription -SubscriptionId $connection.SubscriptionID

# Check for already running or new runbooks
$runbookName = "<RunbookName>"
$rgName = "<ResourceGroupName>"
$aaName = "<AutomationAccountName>"
$jobs = Get-AzureRmAutomationJob -ResourceGroupName $rgName -AutomationAccountName $aaName -RunbookName $runbookName -AzureRmContext $AzureContext

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

### <a name="working-with-multiple-subscriptions"></a>使用多个订阅

若要处理多个订阅，Runbook 必须使用 [Disable-AzureRmContextAutosave](https://docs.microsoft.com/powershell/module/azurerm.profile/disable-azurermcontextautosave) cmdlet 来确保不会从同一个沙盒中运行的另一个 Runbook 检索到身份验证上下文。 Runbook 还会在 AzureRM cmdlet 中使用 `-AzureRmContext` 参数，并将其传递给适当的上下文。

```powershell
# Ensures that you do not inherit an AzureRMContext in your runbook
Disable-AzureRmContextAutosave -Scope Process

$Conn = Get-AutomationConnection -Name AzureRunAsConnection
Connect-AzureRmAccount -ServicePrincipal `
-EnvironmentName AzureChinaCloud `
-Tenant $Conn.TenantID `
-ApplicationId $Conn.ApplicationID `
-CertificateThumbprint $Conn.CertificateThumbprint

$context = Get-AzureRmContext

$ChildRunbookName = 'ChildRunbookDemo'
$AutomationAccountName = 'myAutomationAccount'
$ResourceGroupName = 'myResourceGroup'

Start-AzureRmAutomationRunbook `
    -ResourceGroupName $ResourceGroupName `
    -AutomationAccountName $AutomationAccountName `
    -Name $ChildRunbookName `
    -DefaultProfile $context
```

### <a name="handling-exceptions"></a>处理异常

本部分介绍如何在 Runbook 中处理异常或间歇性问题。

#### <a name="erroractionpreference"></a>ErrorActionPreference

[ErrorActionPreference](https://docs.microsoft.com/powershell/module/microsoft.powershell.core/about/about_preference_variables#erroractionpreference) 变量确定 PowerShell 如何对非终止性错误做出响应。 终止性错误始终会终止，不受 *ErrorActionPreference* 影响。

当 Runbook 使用 `ErrorActionPreference` 时，正常的非终止性错误（例如 **cmdlet 返回的**PathNotFound`Get-ChildItem`）会阻止 Runbook 完成。 以下示例演示了如何使用 `ErrorActionPreference`。 由于脚本将会停止，因此最终的 `Write-Output` 命令永远不会执行。

```powershell
$ErrorActionPreference = 'Stop'
Get-Childitem -path nofile.txt
Write-Output "This message will not show"
```

#### <a name="try-catch-finally"></a>Try Catch Finally

在 PowerShell 脚本中使用 [Try Catch Finally](https://docs.microsoft.com/powershell/module/microsoft.powershell.core/about/about_try_catch_finally) 可以处理终止性错误。 脚本可以使用此机制捕获特定的异常或一般的异常。 应使用 `catch` 语句来跟踪错误或尝试处理错误。 以下示例尝试下载一个不存在的文件。 它捕获 `System.Net.WebException` 异常，对于任何其他异常，它将返回最后一个值。

```powershell
try
{
   $wc = new-object System.Net.WebClient
   $wc.DownloadFile("http://www.contoso.com/MyDoc.doc")
}
catch [System.Net.WebException]
{
    "Unable to download MyDoc.doc from http://www.contoso.com."
}
catch
{
    "An error occurred that could not be resolved."
}
```

#### <a name="throw"></a>Throw

可以使用 [Throw](https://docs.microsoft.com/powershell/module/microsoft.powershell.core/about/about_throw) 来生成终止性错误。 在 Runbook 中定义自己的逻辑时，此机制可能非常有用。 如果脚本满足特定的停止条件，则它可以使用 `throw` 语句进行停止。 以下示例使用此语句显示所需的函数参数。

```powershell
function Get-ContosoFiles
{
  param ($path = $(throw "The Path parameter is required."))
  Get-ChildItem -Path $path\*.txt -recurse
}
```

### <a name="using-executables-or-calling-processes"></a>使用可执行文件或调用进程

在 Azure 沙盒中运行的 Runbook 不支持调用进程，例如可执行文件（ **.exe** 文件）或子进程。  这是因为，Azure 沙盒是在容器中运行的共享进程，它可能无权访问所有底层 API。 对于需要第三方软件或调用子进程的方案，建议在[混合 Runbook 辅助角色](automation-hybrid-runbook-worker.md)上执行 Runbook。

### <a name="accessing-device-and-application-characteristics"></a>访问设备和应用程序特征

在 Azure 沙盒中运行的 Runbook 作业无权访问任何设备或应用程序特征。 最常用于在 Windows 上查询性能指标的 API 是 WMI，其中的一些常见指标是内存和 CPU 使用率。 但是，使用哪个 API 并不重要，因为在云中运行的作业无权访问基于 Web 的企业管理 (WBEM) 的 Azure 实现。 此平台构建在通用信息模型 (CIM) 的基础之上。CIM 提供有关定义设备和应用程序特征的行业标准。

## <a name="handling-errors"></a>处理错误

Runbook 必须能够处理错误。 PowerShell 具有两种类型的错误：终止性和非终止性。 发生终止性错误时，Runbook 会停止执行。 Runbook 将会停止，作业状态为“失败”。

即使发生非终止性错误，脚本也能继续运行。 例如，当 Runbook 对某个不存在的路径使用 `Get-ChildItem` cmdlet 时，就会发生非终止错误。 PowerShell 发现路径不存在，然后引发错误，并继续转到下一文件夹。 在此情况下，该错误不会将 Runbook 作业状态设置为“失败”，甚至可能会完成作业。 若要强制 runbook 在发生非终止性错误时停止，可以使用 `-ErrorAction Stop` cmdlet。

## <a name="handling-jobs"></a>处理作业

可以重用同一自动化帐户中的作业的执行环境。 一个 runbook 可以同时运行多个作业。 同时运行的作业越多，就越可能将其分派到同一个沙盒中。

在同一沙盒进程中运行的作业可能会相互影响。 一个示例是运行 `Disconnect-AzureRmAccount` cmdlet。 执行此 cmdlet 会断开共享沙盒进程中的每个 Runbook 作业。

从 Azure 沙盒中运行的 Runbook 启动的 PowerShell 作业在完整语言模式下可能无法运行。 若要了解有关 PowerShell 语言模式的详细信息，请参阅 [PowerShell 语言模式](https://docs.microsoft.com/powershell/module/microsoft.powershell.core/about/about_language_modes)。 有关如何与 Azure 自动化中的作业进行交互的更多详细信息，请参阅[使用 PowerShell 检索作业状态](#retrieving-job-status-using-powershell)。

### <a name="job-statuses"></a>作业状态

下表描述了作业的可能状态。

| 状态 | 说明 |
|:--- |:--- |
| 已完成 |作业已成功完成。 |
| 失败 |图形 Runbook 或 PowerShell 工作流 Runbook 无法编译。 PowerShell 脚本 Runbook 无法启动，或作业出现异常。 请参阅 [Azure 自动化 Runbook 类型](automation-runbook-types.md)。|
| 失败，正在等待资源 |作业失败，因为它已达到[公平份额](#fair-share)限制三次，并且每次都从同一个检查点或 Runbook 开始处启动。 |
| 已排队 |作业正在等待提供自动化工作线程的资源，以便能够启动。 |
| 正在启动 |作业已分配给辅助角色，并且系统正在将它启动。 |
| 正在恢复 |系统正在恢复已暂停的作业。 |
| 正在运行 |作业正在运行。 |
| 正在运行，正在等待资源 |作业已卸载，因为它已达到公平份额限制。 片刻之后，它将从其上一个检查点恢复。 |
| 已停止 |作业在完成之前已被用户停止。 |
| 正在停止 |系统正在停止作业。 |
| Suspended |仅适用于[图形 Runbook 和 PowerShell 工作流 Runbook](automation-runbook-types.md)。 作业已被用户、系统或 Runbook 中的命令暂停。 如果 Runbook 没有检查点，它将从头开始启动。 如果它有检查点，它将重新启动并从其上一个检查点继续。 仅当发生异常时，系统才会挂起 Runbook。 默认情况下，`ErrorActionPreference` 变量设置为 Continue，表示出错时作业会保持运行。 如果该首选项变量设置为 Stop，则出错时作业会挂起。  |
| 正在暂停 |仅适用于[图形 Runbook 和 PowerShell 工作流 Runbook](automation-runbook-types.md)。 系统正在尝试按用户请求暂停作业。 Runbook 只有在达到其下一个检查点后才能挂起。 如果 Runbook 越过了最后一个检查点，则只有在完成后才能挂起。 |

### <a name="viewing-job-status-from-the-azure-portal"></a>从 Azure 门户查看作业状态

可以查看所有 runbook 作业的概述状态或在 Azure 门户中深入了解特定 runbook 作业的详细信息。 此外，还可配置与 Log Analytics 工作区的集成，以转发 runbook 作业状态和作业流。 有关与 Azure Monitor 日志集成的详细信息，请参阅[将作业状态和作业流从自动化转发到 Azure Monitor 日志](automation-manage-send-joblogs-log-analytics.md)。

在所选自动化帐户的右侧，可在“作业统计信息”磁贴下看到所有 Runbook 作业的摘要。 

![作业统计信息磁贴](./media/automation-runbook-execution/automation-account-job-status-summary.png)

此磁贴显示执行的每个作业的计数及其状态的图形表示形式。

单击磁贴可显示“作业”页，此页包括所有已执行作业的摘要列表。 此页显示每个作业的状态、Runbook 名称、开始时间和完成时间。

![自动化帐户作业页](./media/automation-runbook-execution/automation-account-jobs-status-blade.png)

可以选择“筛选作业”来筛选作业列表。  可按特定的 Runbook 和作业状态进行筛选，或者从下拉列表中进行选择，并提供搜索时间范围。

![筛选作业状态](./media/automation-runbook-execution/automation-account-jobs-filter.png)

或者，可通过从自动化帐户中的“Runbook”页上选择特定的 Runbook，并选择“作业”磁贴，来查看该 Runbook 的作业摘要详细信息。  此操作将打开“作业”页。 在此页中可以单击作业记录以查看其详细信息和输出。

![自动化帐户作业页](./media/automation-runbook-execution/automation-runbook-job-summary-blade.png)

### <a name="viewing-the-job-summary"></a>查看作业摘要

在上述作业摘要中可以查看为特定 Runbook 创建的所有作业的列表及其最新状态。 若要查看某个作业的详细信息和输出，请在列表中单击其名称。 作业的详细视图包括提供给该作业的 Runbook 参数值。

可以使用以下步骤查看 Runbook 的作业。

1. 在 Azure 门户中，选择“自动化”  ，然后选择自动化帐户的名称。
2. 在中心内的“过程自动化”下，选择“Runbook”。  
3. 在“Runbook”页上的列表中选择一个 Runbook。
3. 在所选 runbook 的页上，单击“作业”磁贴  。
4. 单击列表中的某个作业即可在 Runbook 作业详细信息页上查看其详细信息和输出。

### <a name="retrieving-job-status-using-powershell"></a>使用 PowerShell 检索作业状态

可以使用 `Get-AzureRmAutomationJob` cmdlet 检索为 Runbook 创建的作业和特定作业的详细信息。 如果在 PowerShell 中使用 `Start-AzureRmAutomationRunbook` 启动 Runbook，则会返回生成的作业。 可以使用 [Get-AzureRmAutomationJobOutput](https://docs.microsoft.com/powershell/module/azurerm.automation/get-azurermautomationjoboutput?view=azurermps-6.13.0) 检索作业输出。

以下示例获取某个示例 Runbook 的最后一个作业，并显示其状态、为 Runbook 参数提供的值以及该作业的输出。

```azurepowershell
$job = (Get-AzureRmAutomationJob –AutomationAccountName "MyAutomationAccount" `
–RunbookName "Test-Runbook" -ResourceGroupName "ResourceGroup01" | sort LastModifiedDate –desc)[0]
$job.Status
$job.JobParameters
Get-AzureRmAutomationJobOutput -ResourceGroupName "ResourceGroup01" `
–AutomationAccountName "MyAutomationAcct" -Id $job.JobId –Stream Output
```

以下示例检索特定作业的输出并返回每个记录。 如果某个记录存在异常，脚本将会写入该异常而不是值。 此行为非常有用，因为异常可以提供在输出过程中正常情况下可能无法记录的其他信息。

```azurepowershell
$output = Get-AzureRmAutomationJobOutput -AutomationAccountName <AutomationAccountName> -Id <jobID> -ResourceGroupName <ResourceGroupName> -Stream "Any"
foreach($item in $output)
{
    $fullRecord = Get-AzureRmAutomationJobOutputRecord -AutomationAccountName <AutomationAccountName> -ResourceGroupName <ResourceGroupName> -JobId <jobID> -Id $item.StreamRecordId
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

## <a name="getting-details-from-the-activity-log"></a>从活动日志中获取详细信息

可以从自动化帐户的活动日志中检索 Runbook 详细信息，例如启动该 Runbook 的人员或帐户。 以下 PowerShell 示例提供运行指定 Runbook 的最后一个用户。

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

## <a name="sharing-resources-among-runbooks"></a><a name="fair-share"></a>在 Runbook 之间共享资源

为了在云中的所有 Runbook 之间共享资源，Azure 自动化会暂时卸载或停止已运行三小时以上的所有作业。 [PowerShell Runbook](automation-runbook-types.md#powershell-runbooks) 和 [Python Runbook](automation-runbook-types.md#python-runbooks) 的作业将会停止且不会重启，作业状态变为“已停止”。

对于长时间运行的任务，建议使用混合 Runbook 辅助角色。 混合 Runbook 辅助角色不受公平份额限制，并且不会限制 runbook 的执行时间。 其他作业[限制](../azure-resource-manager/management/azure-subscription-service-limits.md#automation-limits)适用于 Azure 沙盒和混合 Runbook 辅助角色。 虽然混合 Runbook 辅助角色不受 3 小时公平份额限制的约束，但你应该开发在辅助角色上运行的 Runbook，以便在出现意外的本地基础结构问题时支持重启。

另一种做法是使用子 Runbook 来优化某个 Runbook。 例如，你的 Runbook 可能会循环访问多个资源上的同一个函数，例如针对多个数据库执行某个数据库操作。 可将此函数移到某个[子 Runbook](automation-child-runbooks.md)，并让 Runbook 使用 `Start-AzAutomationRunbook` 调用此函数。 子 Runbook 在单独的进程中并行执行。

使用子 Runbook 可以减少完成父 Runbook 所需的总时间。 如果 Runbook 仍然包含完成子级后要执行的操作，该 Runbook 可以使用 `Get-AzRmAutomationJob` cmdlet 来检查子 Runbook 的作业状态。

## <a name="next-steps"></a>后续步骤

* 若要详细了解可用于在 Azure 自动化中启动 Runbook 的方法，请参阅[在 Azure 自动化中启动 Runbook](automation-starting-a-runbook.md)。
* 有关 PowerShell 的详细信息（包括语言参考和学习模块），请参阅 [PowerShell 文档](https://docs.microsoft.com/powershell/scripting/overview)。
