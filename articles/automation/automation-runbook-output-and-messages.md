---
title: Azure 自动化中的 Runbook 输出和消息
description: 介绍如何在 Azure 自动化中创建和检索 Runbook 的输出和错误消息。
services: automation
ms.subservice: process-automation
origin.date: 12/04/2018
ms.date: 05/11/2020
ms.topic: conceptual
ms.openlocfilehash: 08a01a2e400d5e32a2f47b02265f72b3783a2371
ms.sourcegitcommit: 7443ff038ea8afe511f7419d9c550d27fb642246
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/09/2020
ms.locfileid: "83001608"
---
# <a name="runbook-output-and-messages-in-azure-automation"></a>Azure 自动化中的 Runbook 输出和消息

大多数 Azure 自动化 runbook 都有某种形式的输出。 此输出可能是发给用户的错误消息，也可能是要用于另一个 Runbook 的复杂对象。 Windows PowerShell 提供 [多个流](https://docs.microsoft.com/powershell/module/microsoft.powershell.core/about/about_redirection) ，以便从脚本或工作流发送输出。 Azure 自动化以不同方式处理每个流。 在创建 Runbook 时，应遵循有关使用流的最佳做法。

下表简要描述了已发布 Runbook 以及在[测试 Runbook](automation-testing-runbook.md) 期间的每个流及其在 Azure 门户中的行为。 输出流是用于 Runbook 之间的通信的主要流。 其他流分类为消息流，旨在将信息传达给用户。 

| Stream | 说明 | 已发布 | 测试 |
|:--- |:--- |:--- |:--- |
| 错误 |面向用户的错误消息。 与发生异常时不同，默认情况下，在出现错误消息后，Runbook 会继续执行。 |写入作业历史记录 |显示在“测试”输出窗格中 |
| 调试 |面向交互式用户的消息。 不应在 Runbook 中使用。 |不会写入作业历史记录 |不会显示在“测试”输出窗格中 |
| 输出 |对象旨在由其他 Runbook 使用。 |写入作业历史记录 |显示在“测试”输出窗格中 |
| 进度 |完成 Runbook 中每个活动之前和之后自动生成的记录。 由于 Runbook 面向交互式用户，因此不应尝试创建其自身的进度记录。 |仅当为 Runbook 启用了进度日志记录时，才写入作业历史记录 |不会显示在“测试”输出窗格中 |
| 详细 |提供一般信息或调试信息的消息。 |仅当为 Runbook 启用了详细日志记录时，才写入作业历史记录 |仅当 Runbook 中的 `VerbosePreference` 变量设置为 Continue 时，才显示在“测试”输出窗格中 |
| 警告 |面向用户的警告消息。 |写入作业历史记录 |显示在“测试”输出窗格中 |

>[!NOTE]
>本文进行了更新，以便使用新的 Azure PowerShell Az 模块。 你仍然可以使用 AzureRM 模块，至少在 2020 年 12 月之前，它将继续接收 bug 修补程序。 若要详细了解新的 Az 模块和 AzureRM 兼容性，请参阅[新 Azure Powershell Az 模块简介](https://docs.microsoft.com/powershell/azure/new-azureps-module-az?view=azps-3.5.0)。 有关适用于混合 Runbook 辅助角色的 Az 模块安装说明，请参阅安装 [Azure PowerShell 模块](https://docs.microsoft.com/powershell/azure/install-az-ps?view=azps-3.5.0)。 

## <a name="output-stream"></a>输出流

输出流用于输出脚本或工作流创建的对象（如果该脚本或工作流正常运行）。 Azure 自动化主要将此流用于调用[当前 Runbook](automation-child-runbooks.md) 的父 Runbook 使用的对象。 当父 Runbook [调用某个内联 Runbook](automation-child-runbooks.md#invoking-a-child-runbook-using-inline-execution) 时，子级会将输出流中的数据返回给父级。 

仅在知道 Runbook 永不会被其他 Runbook 调用时，该 Runbook 才使用输出流将一般信息传达给客户端。 但是，作为最佳做法，Runbook 通常应该使用[详细流](#verbose-stream)向用户传达一般信息。

让 Runbook 使用 [Write-Output](https://technet.microsoft.com/library/hh849921.aspx) 向输出流写入数据。 或者，可以在脚本中独行放置对象。

```powershell
#The following lines both write an object to the output stream.
Write-Output -InputObject $object
$object
```

### <a name="handling-output-from-a-function"></a>处理函数的输出

当 Runbook 函数写入输出流时，输出将传回到 Runbook。 如果 Runbook 将该输出分配给某个变量，则不会将输出写入输出流。 向函数内部的任何其他流写入数据会向 Runbook 的相应流写入数据。 请考虑以下示例 PowerShell 工作流 Runbook。

```powershell
Workflow Test-Runbook
{
  Write-Verbose "Verbose outside of function" -Verbose
  Write-Output "Output outside of function"
  $functionOutput = Test-Function
  $functionOutput

  Function Test-Function
  {
    Write-Verbose "Verbose inside of function" -Verbose
    Write-Output "Output inside of function"
  }
}
```

Runbook 作业的输出流是：

```output
Output inside of function
Output outside of function
```

Runbook 作业的详细流是：

```output
Verbose outside of function
Verbose inside of function
```

发布 Runbook 之后、启动它之前，还必须在 Runbook 设置中启用详细日志记录，以获取详细流输出。

### <a name="declaring-output-data-type"></a>声明输出数据类型

下面是输出数据类型的示例：

* `System.String`
* `System.Int32`
* `System.Collections.Hashtable`
* `Microsoft.Azure.Commands.Compute.Models.PSVirtualMachine`

#### <a name="declare-output-data-type-in-a-workflow"></a>在工作流中声明输出数据类型

工作流使用 [OutputType 属性](https://technet.microsoft.com/library/hh847785.aspx)指定其输出的数据类型。 此属性在运行时不起作用，但在设计时，它会指示 Runbook 的预期输出。 随着 Runbook 工具集的持续发展，在设计时声明输出数据类型的重要性也在不断提升。 因此，最佳做法是在创建的所有 Runbook 中包含此声明。

以下示例 Runbook 输出一个字符串对象，并包含其输出类型的声明。 如果 Runbook 输出了特定类型的数组，则你仍应该指定相对于该类型数组的类型。

```powershell
Workflow Test-Runbook
{
  [OutputType([string])]

  $output = "This is some string output."
  Write-Output $output
}
 ```

#### <a name="declare-output-data-type-in-a-graphical-runbook"></a>在图形 Runbook 中声明输出数据类型

若要在图形 Runbook 或图形 PowerShell 工作流 Runbook 中声明输出类型，可以选择“输入和输出”菜单选项，并输入输出类型。  建议使用完整的 .NET 类名，以便在父 Runbook 引用该类型时可以轻松识别它。 使用完整名称会向 Runbook 中的数据总线公开该类的所有属性，并在将这些属性用于条件逻辑、日志记录和作为其他 Runbook 活动的值引用时提高灵活性。<br> ![Runbook 输入和输出选项](media/automation-runbook-output-and-messages/runbook-menu-input-and-output-option.png)

>[!NOTE]
>在“输入和输出属性”窗格中的“输出类型”字段内输入值后，请务必在控件外部单击，使控件识别输入内容。 

以下示例中的两个图形 Runbook 演示了“输入和输出”功能。 如果应用模块式 Runbook 设计模型，则会获得一个“身份验证 Runbook”模板形式的 Runbook，用于管理使用运行方式帐户在 Azure 中进行的身份验证。 在本例中，通常执行核心逻辑来自动化给定方案的第二个 Runbook 将执行“身份验证 Runbook”模板。 它在“测试”输出窗格中显示结果。 在正常情况下，会使用此 Runbook 针对利用子 Runbook 输出的资源执行某些操作。

下面是 **AuthenticateTo-Azure** Runbook 的基本逻辑。<br> ![身份验证 Runbook 模板示例](media/automation-runbook-output-and-messages/runbook-authentication-template.png)。

该 Runbook 包括输出类型 `Microsoft.Azure.Commands.Profile.Models.PSAzureContext`，该类型返回身份验证配置文件属性。<br> ![Runbook 输出类型示例](media/automation-runbook-output-and-messages/runbook-input-and-output-add-blade.png)

尽管此 Runbook 操作非常简单，此处仍需调出一个配置项。 最后一个活动执行 `Write-Output` cmdlet，以使用 `Inputobject` 参数的 PowerShell 表达式将配置文件数据写入某个变量。 此参数对于 `Write-Output` 是必需的。

本示例中名为 Test-ChildOutputType 的第二个 Runbook 只是定义了两个活动。 <br> ![示例子输出类型 Runbook](media/automation-runbook-output-and-messages/runbook-display-authentication-results-example.png)

第一个活动调用 AuthenticateTo-Azure Runbook。  第二个活动在“数据源”设置为“活动输出”的情况下运行 `Write-Verbose` cmdlet。   此外，“字段路径”设置为 Context.Subscription.SubscriptionName，即 AuthenticateTo-Azure Runbook 的上下文输出。   <br> ![Write-Verbose Cmdlet 参数数据源](media/automation-runbook-output-and-messages/runbook-write-verbose-parameters-config.png)

生成的输出是订阅名称。<br> ![Test-ChildOutputType Runbook 结果](media/automation-runbook-output-and-messages/runbook-test-childoutputtype-results.png)

## <a name="message-streams"></a>消息流

与输出流不同，消息流向用户传达信息。 有多个消息流用于传递不同类型的信息，Azure 自动化以不同的方式处理每个流。

### <a name="warning-and-error-streams"></a>警告和错误流

警告和错误流记录 Runbook 中出现的问题。 执行 Runbook 时，Azure 自动化会将这些流写入作业历史记录。 测试 Runbook 时，自动化会将这些流包含在 Azure 门户的“测试”输出窗格中。 

默认情况下，在出现警告或错误后，Runbook 将继续执行。 在创建消息之前，可以通过让 Runbook 设置 [preference 变量](#preference-variables)，指定 Runbook 应在出现警告或错误时暂停。 例如，若要使 Runbook 在出现错误时暂停（就像发生异常时那样），请将 `ErrorActionPreference` 变量设置为 Stop。

使用 [Write-Warning](https://technet.microsoft.com/library/hh849931.aspx) 或 [Write-Error](https://technet.microsoft.com/library/hh849962.aspx) cmdlet 创建警告或错误消息。 活动也可以写入到警告和错误流。

```powershell
#The following lines create a warning message and then an error message that will suspend the runbook.

$ErrorActionPreference = "Stop"
Write-Warning -Message "This is a warning message."
Write-Error -Message "This is an error message that will stop the runbook because of the preference variable."
```

### <a name="debug-stream"></a>调试流

Azure 自动化对交互式用户使用调试消息流。 不应在 Runbook 中使用此流。

### <a name="verbose-stream"></a>详细流

详细消息流支持有关 Runbook 操作的一般信息。 由于调试流对 Runbook 不可用，因此，Runbook 应使用详细消息来传递调试信息。 

默认情况下，出于性能方面的原因，作业历史记录不会存储来自已发布 Runbook 的详细消息。 若要存储详细消息，请使用 Azure 门户上的“配置”选项卡中的“记录详细记录”设置将已发布的 Runbook 配置为记录详细消息。   启用此选项的目的只是为了排查 Runbook 的问题或对它进行调试。 在大多数情况下，应保留默认设置，即，不记录详细记录。

[测试 runbook](automation-testing-runbook.md) 时，即使已将该 runbook 配置为记录详细记录，详细消息也不会显示。 若要在[测试 Runbook](automation-testing-runbook.md) 时显示详细消息，必须将 `VerbosePreference` 变量设置为 Continue。 设置该变量后，Azure 门户的“测试”输出窗格中会显示详细消息。

以下代码使用 [Write-Verbose](https://technet.microsoft.com/library/hh849951.aspx) cmdlet 创建详细消息。

```powershell
#The following line creates a verbose message.

Write-Verbose -Message "This is a verbose message."
```

## <a name="progress-records"></a>进度记录

可以使用 Azure 门户的“配置”选项卡将 Runbook 配置为记录进度记录。  默认设置是不记录记录，以最大程度地提高性能。 在大多数情况下，应保留默认设置。 启用此选项的目的只是为了排查 Runbook 的问题或对它进行调试。 

如果启用进度记录的记录，则每次运行活动之前和之后，Runbook 会在作业历史记录中写入一条记录。 测试 Runbook 不会显示进度消息，即使已将该 Runbook 配置为记录进度记录。

>[!NOTE]
>[Write-Progress](https://technet.microsoft.com/library/hh849902.aspx) cmdlet 在 runbook 中无效，因为此 cmdlet 旨在供交互式用户使用。

## <a name="preference-variables"></a>Preference 变量

可以在 Runbook 中设置某些 Windows PowerShell [preference 变量](https://technet.microsoft.com/library/hh847796.aspx)，以控制对发送到不同输出流的数据做出的响应。 下表列出了可在 Runbook 中使用的 preference 变量及其默认值和有效值。 在 Azure 自动化外部的 Windows PowerShell 中使用 preference 变量时，可对这些变量使用其他值。

| 变量 | 默认值 | 有效值 |
|:--- |:--- |:--- |
| `WarningPreference` |继续 |停止<br>继续<br>SilentlyContinue |
| `ErrorActionPreference` |继续 |停止<br>继续<br>SilentlyContinue |
| `VerbosePreference` |SilentlyContinue |Stop<br>继续<br>SilentlyContinue |

下表列出了在 Runbook 中有效的 preference 变量值的行为。

| Value | 行为 |
|:--- |:--- |
| 继续 |记录消息并继续执行 Runbook。 |
| SilentlyContinue |继续执行 Runbook 但不记录消息。 该值会导致忽略消息。 |
| 停止 |记录消息并挂起 Runbook。 |

## <a name="retrieving-runbook-output-and-messages"></a><a name="runbook-output"></a>检索 runbook 输出和消息

### <a name="retrieve-runbook-output-and-messages-in-azure-portal"></a>在 Azure 门户中检索 Runbook 输出和消息

可以在 Azure 门户中使用 Runbook 的“作业”选项卡查看 Runbook 作业的详细信息。  作业摘要显示输入参数和[输出流](#output-stream)，此外，还显示有关作业的一般信息以及发生的任何异常。 作业历史记录包含来自输出流以及[警告和错误流](#warning-and-error-streams)的消息。 此外，如果 Runbook 配置为记录详细记录和进度记录，则作业历史记录还包含来自[详细流](#verbose-stream)和[进度记录](#progress-records)的消息。

### <a name="retrieve-runbook-output-and-messages-in-windows-powershell"></a>在 Windows PowerShell 中检索 Runbook 输出和消息

在 Windows PowerShell 中，可以使用 [Get-AzureAutomationJobOutput](https://docs.microsoft.com/powershell/module/servicemanagement/azure/get-azureautomationjoboutput) cmdlet 检索 Runbook 的输出和消息。 此 cmdlet 需要作业的 ID，并具有一个名为 `Stream` 的参数，在其中可以指定要检索的流。 可为此参数指定 Any 值，以检索作业的所有流。

以下示例将启动一个示例 Runbook，并等待该 Runbook 完成。 Runbook 执行完成后，脚本将从作业收集 Runbook 输出流。

```powershell
$job = Start-AzAutomationRunbook -ResourceGroupName "ResourceGroup01" `
  -AutomationAccountName "MyAutomationAccount" -Name "Test-Runbook"

$doLoop = $true
While ($doLoop) {
  $job = Get-AzAutomationJob -ResourceGroupName "ResourceGroup01" `
    -AutomationAccountName "MyAutomationAccount" -Id $job.JobId
  $status = $job.Status
  $doLoop = (($status -ne "Completed") -and ($status -ne "Failed") -and ($status -ne "Suspended") -and ($status -ne "Stopped"))
}

Get-AzAutomationJobOutput -ResourceGroupName "ResourceGroup01" `
  -AutomationAccountName "MyAutomationAccount" -Id $job.JobId -Stream Output

# For more detailed job output, pipe the output of Get-AzAutomationJobOutput to Get-AzAutomationJobOutputRecord
Get-AzAutomationJobOutput -ResourceGroupName "ResourceGroup01" `
  -AutomationAccountName "MyAutomationAccount" -Id $job.JobId -Stream Any | Get-AzAutomationJobOutputRecord
```

### <a name="retrieve-runbook-output-and-messages-in-graphical-runbooks"></a>检索图形 Runbook 中的 Runbook 输出和消息

对于图形 Runbook，可通过活动级跟踪的形式额外记录输出和消息。 有两个级别的跟踪：基本和详细。 基本跟踪显示 Runbook 中每个活动的开始和结束时间，以及与任何活动重试相关的信息。 例如，活动的尝试次数和开始时间。 详细跟踪包含基本跟踪功能，此外会记录每个活动的输入和输出数据。 

目前，活动级跟踪将使用详细流写入记录。 因此，在启用跟踪时，必须启用详细日志记录。 对于启用了跟踪的图形 runbook，无需记录进度记录。 基本跟踪起着相同的目的，并且提供更多信息。

![“图形创作作业流”视图](media/automation-runbook-output-and-messages/job-streams-view-blade.png)

从插图中可以看出，为图形 Runbook 启用详细日志记录和跟踪会在生产“作业流”视图中产生多得多的信息。  这些额外的信息对于排查 Runbook 的生产问题至关重要。 

但是，除非需要此信息来跟踪 Runbook 进度以进行故障排除，否则一般情况下，建议将跟踪保持禁用状态。 跟踪记录可能特别庞大。 使用图形 Runbook 跟踪时，可以获取每个活动的二到四条记录，具体取决于是配置了基本跟踪还是详细跟踪。

**若要启用活动级跟踪：**

1. 在 Azure 门户中，打开自动化帐户。
2. 在“过程自动化”下选择“Runbook”，打开 Runbook 列表。  
3. 在“Runbook”页上，从 Runbook 列表中选择一个图形 Runbook。
4. 在“设置”  下，单击“日志记录和跟踪”  。
5. 在“日志记录和跟踪”页上的“记录详细记录”下，单击“打开”以启用详细日志记录。  
6. 在“活动级跟踪”下，根据所需的跟踪级别，将跟踪级别更改为“基本”或“详细”。   <br>

   ![“图形创作日志记录和跟踪”页面](media/automation-runbook-output-and-messages/logging-and-tracing-settings-blade.png)

### <a name="retrieve-runbook-output-and-messages-in-azure-monitor-logs"></a>在 Azure Monitor 日志中检索 Runbook 输出和消息

Azure 自动化可将 Runbook 作业状态和作业流发送到 Log Analytics 工作区。 Azure Monitor 支持使用日志来实现以下目的：

* 获取有关自动化作业的见解。
* 基于 Runbook 作业状态（例如“失败”或“暂停”）触发电子邮件或警报。
* 针对不同的作业流编写高级查询。
* 跨自动化帐户关联作业。
* 可视化作业历史记录。

有关如何配置与 Azure Monitor 日志的集成以收集、关联和处理作业数据的详细信息，请参阅[将作业状态和作业流从自动化转发到 Azure Monitor 日志](automation-manage-send-joblogs-log-analytics.md)。

## <a name="next-steps"></a>后续步骤

* 若要详细了解 Runbook 的执行方式、Runbook 作业的监视和其他技术详细信息，请参阅[跟踪 Runbook 作业](automation-runbook-execution.md)。
* 若要了解如何设计和使用子 Runbook，请参阅 [Azure 自动化中的子 Runbook](automation-child-runbooks.md)。
* 有关 PowerShell 的详细信息（包括语言参考和学习模块），请参阅 [PowerShell 文档](https://docs.microsoft.com/powershell/scripting/overview)。
