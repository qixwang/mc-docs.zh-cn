---
title: 在 Azure 自动化中执行 Runbook
description: 详细介绍如何处理 Azure 自动化中的 Runbook。
services: automation
ms.subservice: process-automation
origin.date: 04/04/2019
ms.date: 05/25/2020
ms.topic: conceptual
ms.openlocfilehash: b84ca34214cc0f19e22c5b353f0d92e1c43232dd
ms.sourcegitcommit: 981a75a78f8cf74ab5a76f9e6b0dc5978387be4b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/22/2020
ms.locfileid: "83801106"
---
# <a name="runbook-execution-in-azure-automation"></a>在 Azure 自动化中执行 Runbook

使用 Azure 自动化中的流程自动化，你可以创建和管理 PowerShell、PowerShell 工作流和图形 Runbook。 有关详细信息，请参阅 [Azure 自动化 Runbook](automation-runbook-types.md)。 

自动化基于 Runbook 中定义的逻辑来执行 Runbook。 Runbook 在中断的情况下会从头开始重启。 这种行为要求编写的 Runbook 在发生暂时性问题时支持重启。

在 Azure 自动化中启动 Runbook 会创建一个作业，该作业是 Runbook 的单个执行实例。 每个作业通过与 Azure 订阅建立连接来访问 Azure 资源。 仅当数据中心的资源可从公有云访问时，作业才能访问这些资源。

在执行 Runbook 期间，Azure 自动化将分配一个辅助角色来运行每个作业。 尽管辅助角色由多个 Azure 帐户共享，但不同自动化帐户中的作业是相互独立的。 无法控制由哪个辅助角色来处理作业请求。

在 Azure 门户中查看 Runbook 列表时，会显示针对每个 Runbook 启动的作业的状态。 Azure 自动化将作业日志最多存储 30 天。

下图显示了 [PowerShell Runbook](automation-runbook-types.md#powershell-runbooks)、[PowerShell 工作流 Runbook](automation-runbook-types.md#powershell-workflow-runbooks) 和[图形 Runbook](automation-runbook-types.md#graphical-runbooks) 的 Runbook 作业生命周期。

![作业状态 - PowerShell 工作流](./media/automation-runbook-execution/job-statuses.png)

[!INCLUDE [GDPR-related guidance](../../includes/gdpr-dsr-and-stp-note.md)]

## <a name="runbook-execution-environment"></a>Runbook 执行环境

Azure 自动化中的 Runbook 可以在 Azure 沙盒上运行，也可以在[混合 Runbook 辅助角色](automation-hybrid-runbook-worker.md)上运行。 

当 Runbook 设计为针对 Azure 中的资源进行身份验证并运行时，它们将在 Azure 沙盒中运行，而沙盒是可以由多个作业使用的共享环境。 使用同一沙盒的作业受沙盒的资源限制约束。 Azure 沙盒环境不支持交互式操作。 它会阻止对所有进程外 COM 服务器的访问。 对于进行 Win32 调用的 Runbook，它还需要使用本地 MOF 文件。

你还可使用[混合 Runbook 辅助角色](automation-hybrid-runbook-worker.md)直接在托管角色的计算机上运行 Runbook，或对环境中的本地资源运行 Runbook。 Azure 自动化将存储并管理 Runbook，然后将其传送到一台或多台指定的计算机。

>[!NOTE]
>若要在 Linux 混合 Runbook 辅助角色上运行，必须对脚本进行签名，并相应地配置辅助角色。 或者，[必须禁用签名验证](automation-linux-hrw-install.md#turn-off-signature-validation)。 

下表列出了一些 Runbook 执行任务，以及每个任务的建议执行环境。

|任务|建议|注释|
|---|---|---|
|与 Azure 资源集成|Azure 沙盒|托管在 Azure 中，身份验证更为简单。 如果使用 Azure VM 上的混合 Runbook 辅助角色，则可[使用具有托管标识的 Runbook 身份验证](automation-hrw-run-runbooks.md#runbook-auth-managed-identities)。|
|管理 Azure 资源时可以获得最佳性能|Azure 沙盒|脚本在同一环境中运行，因此延迟更低。|
|最大程度减少运营成本|Azure 沙盒|没有计算开销，不需要 VM。|
|执行长时间运行的脚本|混合 Runbook 辅助角色|Azure 沙盒具有[资源限制](../azure-resource-manager/management/azure-subscription-service-limits.md#automation-limits)。|
|与本地服务进行交互|混合 Runbook 辅助角色|直接访问主机、其他云环境中的资源或本地环境中的资源。 |
|需要第三方软件和可执行文件|混合 Runbook 辅助角色|由你管理操作系统，可以安装软件。|
|运行资源密集型脚本|混合 Runbook 辅助角色| Azure 沙盒具有[资源限制](../azure-resource-manager/management/azure-subscription-service-limits.md#automation-limits)。|
|使用存在特定要求的模块| 混合 Runbook 辅助角色|下面是一些示例：</br> WinSCP - winscp.exe 上的依赖项 </br> IIS 管理 - 取决于 IIS 的启用或托管|
|使用安装程序安装模块|混合 Runbook 辅助角色|沙盒的模块必须支持复制。|
|使用需要不同于 4.7.2 版本的 .NET Framework 的 Runbook 或模块|混合 Runbook 辅助角色|Azure 沙盒支持 .NET Framework 4.7.2，不支持升级到其他版本。|
|运行需要提升权限的脚本|混合 Runbook 辅助角色|沙盒不允许提升权限。 使用混合 Runbook 辅助角色可以关闭 UAC，并在运行需要提升权限的命令时使用 [Invoke-Command](https://docs.microsoft.com/powershell/module/microsoft.powershell.core/invoke-command?view=powershell-7)。|
|运行那些需要访问 Windows Management Instrumentation (WMI) 的脚本|混合 Runbook 辅助角色|在云中的沙盒中运行的作业无法访问 WMI 提供程序。 |

## <a name="resources"></a>资源

Runbook 必须包含用于处理[资源](https://docs.microsoft.com/rest/api/resources/resources)（例如 VM、网络和网络上的资源）的逻辑。 资源与 Azure 订阅相关联，并且 Runbook 需要适当的凭据才能访问任何资源。 有关在 Runbook 中处理资源的示例，请参阅[处理资源](manage-runbooks.md#handle-resources)。 

## <a name="security"></a>安全性

Azure 自动化使用 Azure 安全中心 (ASC) 为资源提供安全保障，并检测 Linux 系统中是否存在数据泄露现象。 不管资源是否在 Azure 中，均跨工作负载提供安全性。 请参阅 [Azure 自动化中的身份验证简介](automation-security-overview.md)。

ASC 对可在 VM 上运行任何脚本（有符号或无符号）的用户施加限制。 如果你是具有 VM 的根访问权限的用户，则必须使用数字签名显式配置计算机或将其关闭。 否则，只有在创建自动化帐户并启用适当的功能之后，才能通过运行脚本来应用操作系统更新。

## <a name="credentials"></a>凭据

Runbook 需要适当的[凭据](shared-resources/credentials.md)才能访问 Azure 或第三方系统的任何资源。 这些凭据存储在 Azure 自动化、密钥保管库等中。  

## <a name="azure-monitor"></a>Azure Monitor

Azure 自动化利用 [Azure Monitor](/azure-monitor/overview) 来监视其计算机操作。 操作需要 Log Analytics 工作区和 [Log Analytics 代理](/azure-monitor/platform/log-analytics-agent)。

### <a name="log-analytics-agent-for-windows"></a>适用于 Windows 的 Log Analytics 代理

[适用于 Windows 的 Log Analytics 代理](/azure-monitor/platform/agent-windows)使用 Azure Monitor 来管理 Windows VM 和物理计算机。 计算机可以在 Azure 中运行，也可以在非 Azure 环境（例如本地数据中心）中运行。 必须将代理配置为向一个或多个 Log Analytics 工作区报告。 

>[!NOTE]
>适用于 Windows 的 Log Analytics 代理之前称为 Microsoft Monitoring Agent (MMA)。

### <a name="log-analytics-agent-for-linux"></a>适用于 Linux 的 Log Analytics 代理

[适用于 Linux 的 Log Analytics 代理](/azure-monitor/platform/agent-linux)的工作方式类似于适用于 Windows 的 Log Analytics 代理，但会将 Linux 计算机连接到 Azure Monitor。 安装此代理时需具有 nxautomation 用户帐户，该用户帐户允许执行需要根权限的命令，例如，在混合 Runbook 辅助角色上执行命令。 nxautomation 帐户是无需密码的系统帐户。 

在[安装 Linux 混合 Runbook 辅助角色](automation-linux-hrw-install.md)期间，必须存在具有相应 sudo 权限的 nxautomation 帐户。 如果尝试安装辅助角色时该帐户不存在或帐户不具有相应权限，则安装将失败。

Log Analytics 代理和 nxautomation 帐户的可用日志如下：

* /var/opt/microsoft/omsagent/log/omsagent.log - Log Analytics 代理日志 
* /var/opt/microsoft/omsagent/run/automationworker/worker.log - 自动化辅助角色日志

>[!NOTE]
>作为更新管理一部分加入的 nxautomation 用户仅执行已签名的 Runbook。

## <a name="runbook-permissions"></a>Runbook 权限

Runbook 需要通过凭据进行 Azure 身份验证的权限。 请参阅 [管理 Azure 自动化运行方式帐户](manage-runas-account.md)。 
## <a name="certificates"></a>证书

Azure 自动化使用[证书](shared-resources/certificates.md)进行 Azure 身份验证，或者将证书添加到 Azure 或第三方资源。 这些证书被存储在安全位置，以供 Runbook 和 DSC 配置使用。 

Runbook 可以使用不是由证书颁发机构 (CA) 签名的自签名证书。 请参阅[创建新证书](shared-resources/certificates.md#create-a-new-certificate)。

## <a name="jobs"></a>作业

Azure 自动化支持从同一自动化帐户运行作业的环境。 一个 runbook 可以同时运行多个作业。 同时运行的作业越多，就越可能将其分派到同一个沙盒中。 

在同一沙盒进程中运行的作业可能会相互影响。 一个示例是运行 [Disconnect-AzAccount](https://docs.microsoft.com/powershell/module/az.accounts/disconnect-azaccount?view=azps-3.7.0) cmdlet。 执行此 cmdlet 会断开共享沙盒进程中的每个 Runbook 作业。 有关使用此方案的示例，请参阅[预防并发作业](manage-runbooks.md#prevent-concurrent-jobs)。

>[!NOTE]
>从 Azure 沙盒中运行的 Runbook 启动的 PowerShell 作业可能无法在完整 [PowerShell 语言模式](https://docs.microsoft.com/powershell/module/microsoft.powershell.core/about/about_language_modes)下运行。 

### <a name="job-statuses"></a>作业状态

下表描述了作业的可能状态。 可以查看所有 runbook 作业的摘要状态，或深入了解 Azure 门户中特定 runbook 作业的详细信息。 此外，还可配置与 Log Analytics 工作区的集成，以转发 runbook 作业状态和作业流。 有关与 Azure Monitor 日志集成的详细信息，请参阅[将作业状态和作业流从自动化转发到 Azure Monitor 日志](automation-manage-send-joblogs-log-analytics.md)。 有关在 Runbook 中使用状态的示例，另请参阅[获取作业状态](manage-runbooks.md#obtain-job-statuses)。

| 状态 | 说明 |
|:--- |:--- |
| 已完成 |作业已成功完成。 |
| 已失败 |图形 Runbook 或 PowerShell 工作流 Runbook 无法编译。 PowerShell Runbook 无法启动，或作业出现异常。 请参阅 [Azure 自动化 Runbook 类型](automation-runbook-types.md)。|
| 失败，正在等待资源 |作业失败，因为它已达到 [公平份额](#fair-share) 限制三次，并且每次都已从同一个检查点或 Runbook 开始处启动。 |
| 已排队 |作业正在等待提供自动化工作线程的资源，以便能够启动。 |
| 正在恢复 |系统正在恢复已暂停的作业。 |
| 正在运行 |作业正在运行。 |
| 正在运行，正在等待资源 |作业已卸载，因为它已达到公平份额限制。 片刻之后，它会从其上一个检查点恢复。 |
| 正在启动 |作业已分配给辅助角色，并且系统正在将它启动。 |
| 已停止 |作业在完成之前已被用户停止。 |
| 正在停止 |系统正在停止作业。 |
| 已挂起 |仅适用于[图形 Runbook 和 PowerShell 工作流 Runbook](automation-runbook-types.md)。 作业已被用户、系统或 Runbook 中的命令暂停。 如果 Runbook 没有检查点，它将从头开始启动。 如果它有检查点，它将重新启动并从其上一个检查点继续。 仅当发生异常时，系统才会挂起 Runbook。 默认情况下，`ErrorActionPreference` 变量设置为 Continue，表示出错时作业会保持运行。 如果该首选项变量设置为 Stop，则出错时作业会挂起。  |
| 正在暂停 |仅适用于[图形 Runbook 和 PowerShell 工作流 Runbook](automation-runbook-types.md)。 系统正在尝试按用户请求暂停作业。 Runbook 只有在达到其下一个检查点后才能挂起。 如果 Runbook 越过了最后一个检查点，则只有在完成后才能挂起。 |

## <a name="activity-logging"></a>活动日志记录

在 Azure 自动化中执行 Runbook 会在自动化帐户的活动日志中写入详细信息。 有关使用日志的详细信息，请参阅[从活动日志中检索详细信息](manage-runbooks.md#retrieve-details-from-activity-log)。 

## <a name="exceptions"></a>异常

本部分介绍如何在 Runbook 中处理异常或间歇性问题。 例如，WebSocket 异常。 正确处理异常可防止暂时性的网络故障导致 runbook 失败。 

### <a name="erroractionpreference"></a>ErrorActionPreference

[ErrorActionPreference](https://docs.microsoft.com/powershell/module/microsoft.powershell.core/about/about_preference_variables#erroractionpreference) 变量确定 PowerShell 如何对非终止性错误做出响应。 终止性错误始终会终止，不受 `ErrorActionPreference` 影响。

当 Runbook 使用 `ErrorActionPreference` 时，正常的非终止性错误（例如 [Get-ChildItem](https://docs.microsoft.com/powershell/module/microsoft.powershell.management/get-childitem?view=powershell-7) cmdlet 返回的 `PathNotFound`）会阻止 Runbook 完成。 以下示例演示了如何使用 `ErrorActionPreference`。 由于脚本将会停止，最终的 [Write-Output](https://docs.microsoft.com/powershell/module/microsoft.powershell.utility/write-output?view=powershell-7) 命令永远不会执行。

```powershell
$ErrorActionPreference = 'Stop'
Get-ChildItem -path nofile.txt
Write-Output "This message will not show"
```

### <a name="try-catch-finally"></a>Try Catch Finally

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

### <a name="throw"></a>Throw

可以使用 [Throw](https://docs.microsoft.com/powershell/module/microsoft.powershell.core/about/about_throw) 来生成终止性错误。 在 Runbook 中定义自己的逻辑时，此机制可能非常有用。 如果脚本满足特定的停止条件，则它可以使用 `throw` 语句进行停止。 以下示例使用此语句显示所需的函数参数。

```powershell
function Get-ContosoFiles
{
  param ($path = $(throw "The Path parameter is required."))
  Get-ChildItem -Path $path\*.txt -recurse
}
```

## <a name="errors"></a>错误

Runbook 必须处理错误。 Azure 自动化支持两种类型的 PowerShell 错误：终止性和非终止性。 

发生终止性错误时，Runbook 会停止执行。 Runbook 将会停止，作业状态为“失败”。

即使发生非终止性错误，脚本也能继续运行。 例如，当 Runbook 对某个不存在的路径使用 `Get-ChildItem` cmdlet 时，就会发生非终止错误。 PowerShell 发现路径不存在，然后引发错误，并继续转到下一文件夹。 在此情况下，该错误不会将 Runbook 作业状态设置为“失败”，甚至可能会完成作业。 若要强制 runbook 在发生非终止性错误时停止，可以使用 `ErrorAction Stop` cmdlet。

## <a name="calling-processes"></a>调用进程

在 Azure 沙盒中运行的 Runbook 不支持调用进程，例如可执行文件（ **.exe** 文件）或子进程。 这是因为，Azure 沙盒是在容器中运行的共享进程，它可能无法访问所有底层 API。 对于需要第三方软件或调用子进程的方案，应在[混合 Runbook 辅助角色](automation-hybrid-runbook-worker.md)上执行 Runbook。

## <a name="device-and-application-characteristics"></a>设备和应用程序特征

Azure 沙盒中的 runbook 作业无法访问任何设备或应用程序特征。 最常用于在 Windows 上查询性能指标的 API 是 WMI，其中的一些常见指标是内存和 CPU 使用率。 但是，使用哪个 API 并不重要，因为在云中运行的作业无法访问基于 Web 的企业管理 (WBEM) 的 Microsoft 实现。 此平台构建在通用信息模型 (CIM) 的基础之上。CIM 提供有关定义设备和应用程序特征的行业标准。

## <a name="webhooks"></a>Webhook

外部服务（例如 Azure DevOps Services 和 GitHub）可以在 Azure 自动化中启动 Runbook。 若要执行这种类型的启动，服务需通过单个 HTTP 请求使用 [Webhook](automation-webhooks.md)。 使用 Webhook 可在未实现完整 Azure 自动化解决方案的情况下启动 Runbook。 

## <a name="shared-resources"></a><a name="fair-share"></a>共享资源

为了在云中的所有 Runbook 之间共享资源，Azure 使用名为“公平共享”的概念。 使用公平共享时，Azure 会临时卸载或停止运行超过三个小时的任何作业。 [PowerShell Runbook](automation-runbook-types.md#powershell-runbooks) 和 [Python Runbook](automation-runbook-types.md#python-runbooks) 的作业将会停止且不会重启，作业状态变为“已停止”。

对于长时间运行的 Azure 自动化任务，建议使用混合 Runbook 辅助角色。 混合 Runbook 辅助角色不受公平份额限制，并且不会限制 runbook 的执行时间。 其他作业[限制](../azure-resource-manager/management/azure-subscription-service-limits.md#automation-limits)适用于 Azure 沙盒和混合 Runbook 辅助角色。 虽然混合 Runbook 辅助角色不受 3 小时公平份额限制的约束，但你应该开发在辅助角色上运行的 Runbook，以便在出现意外的本地基础结构问题时支持重启。

另一种做法是使用子 Runbook 来优化某个 Runbook。 例如，你的 Runbook 可能会循环访问多个资源上的同一个函数，例如针对多个数据库执行某个数据库操作。 可将此函数移到某个[子 Runbook](automation-child-runbooks.md)，并让 Runbook 使用 [Start-AzureRmAutomationRunbook](https://docs.microsoft.com/powershell/module/azurerm.automation/start-azurermautomationrunbook?view=azurermps-6.13.0) 调用此函数。 子 Runbook 在单独的进程中并行执行。

使用子 Runbook 可以减少完成父 Runbook 所需的总时间。 如果 Runbook 在某个子级完成后还有更多操作，则它可以使用 `Get-AzRmAutomationJob` cmdlet 来检查该子 Runbook 的作业状态。

## <a name="next-steps"></a>后续步骤

* 若要开始使用 Runbook，请参阅[在 Azure 自动化中管理 Runbook](manage-runbooks.md)。
* 有关 PowerShell 的详细信息（包括语言参考和学习模块），请参阅 [PowerShell 文档](https://docs.microsoft.com/powershell/scripting/overview)。
