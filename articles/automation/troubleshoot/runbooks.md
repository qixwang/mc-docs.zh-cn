---
title: 排查 Azure 自动化 Runbook 错误
description: 了解如何排查和解决在使用 Azure 自动化 Runbook 时可能遇到的问题。
services: automation
author: WenJason
ms.author: v-jay
origin.date: 01/24/2019
ms.date: 05/25/2020
ms.topic: conceptual
ms.service: automation
manager: digimobile
ms.custom: has-adal-ref
ms.openlocfilehash: af8203a12d407d08585fd2ca7694f53badab3677
ms.sourcegitcommit: 981a75a78f8cf74ab5a76f9e6b0dc5978387be4b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/22/2020
ms.locfileid: "83801148"
---
# <a name="troubleshoot-runbook-errors"></a>排查 Runbook 错误

 本文描述可能会发生的各种 Runbook 错误及其解决方法。

## <a name="diagnose-runbook-issues"></a>诊断 Runbook 问题

如果在 Azure 自动化中执行 Runbook 期间遇到错误，可使用以下步骤来帮助诊断问题：

1. 确保 Runbook 脚本在本地计算机上执行成功。

    有关语言参考和学习模块，请参阅 [PowerShell 文档](https://docs.microsoft.com/powershell/scripting/overview)或 [Python 文档](https://docs.python.org/3/)。在本地运行脚本即可发现并解决常见错误，例如：

      * 缺少模块
      * 语法错误
      * 逻辑错误

1. 调查 Runbook [错误流](../automation-runbook-output-and-messages.md#runbook-output)。

    查看这些流中的特定消息，并将其与本文中所述的错误进行比较。

1. 如果 Runbook 暂停或意外失败，请执行以下操作：

    * 如果运行方式帐户已过期，请[续订证书](../manage-runas-account.md#cert-renewal)。
    * 若要尝试使用过期的 Webhook 启动 Runbook，请[续订 Webhook](../automation-webhooks.md#renew-a-webhook)。
    * [检查作业状态](../automation-runbook-execution.md#job-statuses)，确定当前 Runbook 状态以及导致问题的一些可能原因。
    * [将更多输出添加到](../automation-runbook-output-and-messages.md#message-streams) runbook，以确定 runbook 挂起之前发生了什么情况。
    * [处理由作业引发的任何异常](../automation-runbook-execution.md#exceptions)。

1. 如果混合 Runbook 辅助角色中的 Runbook 作业或环境无响应，请执行此步骤。

    如果在混合 Runbook 辅助角色而不是 Azure 自动化中运行 Runbook，可能需要[排查混合辅助角色本身的问题](/automation/troubleshoot/hybrid-runbook-worker)。

## <a name="scenario-runbook-fails-with-a-no-permission-or-forbidden-403-error"></a><a name="runbook-fails-no-permission"></a>场景：Runbook 失败并出现“无权限”或“禁止 403”错误

### <a name="issue"></a>问题

Runbook 失败并出现“无权限”或“禁止 403”错误或者类似的错误。

### <a name="cause"></a>原因

运行方式帐户对 Azure 资源的权限可能不同于当前的自动化帐户。 

### <a name="resolution"></a>解决方法

确保运行方式帐户[有权访问](/role-based-access-control/role-assignments-portal)脚本中使用的任何资源。

## <a name="scenario-sign-in-to-azure-account-failed"></a><a name="sign-in-failed"></a>场景：登录到 Azure 帐户失败

### <a name="issue"></a>问题

使用 `Connect-AzAccount` cmdlet 时遇到以下错误之一：

```error
Unknown_user_type: Unknown User Type
```

```error
No certificate was found in the certificate store with thumbprint
```

### <a name="cause"></a>原因

如果凭据资产名称无效，将发生这些错误。 如果用于设置自动化凭据资产的用户名和密码无效，也可能会发生这些错误。

### <a name="resolution"></a>解决方法

若要确定问题所在，请执行以下步骤：

1. 请确保没有包含任何特殊字符。 这些字符包括用于连接 Azure 的自动化凭据资产名称中的 `\@` 字符。
1. 检查是否能够在本地 PowerShell ISE 编辑器中使用存储在 Azure 自动化凭据中的用户名和密码。 在 PowerShell ISE 中运行以下 cmdlet。

   ```powershell
   $Cred = Get-Credential
   #Using Azure Service Management
   Add-AzureAccount -Environment AzureChinaCloud -Credential $Cred  
   #Using Azure Resource Manager
   Connect-AzAccount -EnvironmentName AzureChinaCloud -Credential $Cred
   ```

1. 如果无法在本地进行身份验证，则表示尚未正确设置 Azure Active Directory (Azure AD) 凭据。 若要正确设置 Azure AD 帐户，请参阅博客文章：[使用 Azure Active Directory 在 Azure 中进行身份验证](https://azure.microsoft.com/blog/azure-automation-authenticating-to-azure-using-azure-active-directory/)。

1. 如果该错误看上去是暂时性的，请尝试向身份验证例程添加重试逻辑，使身份验证更加可靠。

   ```powershell
   # Get the connection "AzureRunAsConnection"
   $connectionName = "AzureRunAsConnection"
   $servicePrincipalConnection = Get-AutomationConnection -Name $connectionName

   $logonAttempt = 0
   $logonResult = $False

   while(!($connectionResult) -And ($logonAttempt -le 10))
   {
       $LogonAttempt++
       #Logging in to Azure...
       $connectionResult = Connect-AzureRmAccount `
                              -EnvironmentName AzureChinaCloud `
                              -ServicePrincipal `
                              -TenantId $servicePrincipalConnection.TenantId `
                              -ApplicationId $servicePrincipalConnection.ApplicationId `
                              -CertificateThumbprint $servicePrincipalConnection.CertificateThumbprint

       Start-Sleep -Seconds 30
   }
   ```

## <a name="scenario-unable-to-find-the-azure-subscription"></a><a name="unable-to-find-subscription"></a>场景：无法找到 Azure 订阅

### <a name="issue"></a>问题

使用 `Select-AzureSubscription` 或 `Select-AzureRMSubscription` cmdlet 时遇到以下错误：

```error
The subscription named <subscription name> cannot be found.
```

### <a name="error"></a>错误

如果出现以下情况，则可能会发生此错误：

* 订阅名称无效。
* 尝试获取订阅详细信息的 Azure AD 用户未配置为订阅的管理员。
* cmdlet 不可用。

### <a name="resolution"></a>解决方法

按以下步骤确定你是否已在 Azure 中完成身份验证并有权访问你尝试选择的订阅：

1. 为确保脚本可单独正常运行，请在 Azure 自动化外部对其进行测试。
1. 确保脚本先运行 [Connect-AzureRmAccount](https://docs.microsoft.com/powershell/module/azurerm.profile/connect-azurermaccount?view=azurermps-6.13.0) cmdlet，再运行 `Select-*` cmdlet。
1. 如果仍看到该错误消息，请通过为 `Connect-AzureRmAccount` 添加 `AzContext` 参数来修改代码，然后执行代码。

   ```powershell
   $Conn = Get-AutomationConnection -Name AzureRunAsConnection
   Connect-AzureRmAccount -ServicePrincipal -Tenant $Conn.TenantID -ApplicationId $Conn.ApplicationID -CertificateThumbprint $Conn.CertificateThumbprint

   $context = Get-AzureRmContext

   Get-AzureRmVM -ResourceGroupName myResourceGroup -AzContext $context
    ```

## <a name="scenario-authentication-to-azure-fails-because-multifactor-authentication-is-enabled"></a><a name="auth-failed-mfa"></a>场景：无法在 Azure 中进行身份验证，因为已启用多重身份验证

### <a name="issue"></a>问题

使用 Azure 用户名和密码在 Azure 中进行身份验证时遇到以下错误：

```error
Add-AzureAccount: AADSTS50079: Strong authentication enrollment (proof-up) is required
```

### <a name="cause"></a>原因

如果对 Azure 帐户设置了多重身份验证，则不能使用 Azure Active Directory 用户在 Azure 中进行身份验证。 需要使用证书或服务主体进行身份验证。

### <a name="resolution"></a>解决方法

若要在 Azure 经典部署模型 cmdlet 中使用证书，请参阅[创建并添加用于管理 Azure 服务的证书](https://blogs.technet.com/b/orchestrator/archive/2014/04/11/managing-azure-services-with-the-microsoft-azure-automation-preview-service.aspx)。 若要在 Azure 资源管理器 cmdlet 中使用服务主体，请参阅[使用 Azure 门户创建服务主体](../../active-directory/develop/howto-create-service-principal-portal.md)和[使用 Azure 资源管理器对服务主体进行身份验证](../../active-directory/develop/howto-authenticate-service-principal-powershell.md)。

## <a name="scenario-object-reference-not-set-to-an-instance-of-an-object"></a><a name="child-runbook-object"></a>场景：对象引用未设置为某个对象的实例

### <a name="issue"></a>问题

结合 `Wait` 参数调用子 Runbook 并且输出流包含对象时遇到以下错误：

```error
Object reference not set to an instance of an object
```

### <a name="cause"></a>原因

如果流包含对象，`Start-AzureRmAutomationRunbook` 不会正确处理输出流。

### <a name="resolution"></a>解决方法

实现轮询逻辑，并使用 [Get-AzureRmAutomationJobOutput](https://docs.microsoft.com/powershell/module/azurerm.automation/get-azurermautomationjoboutput) cmdlet 检索输出。 下面定义了此逻辑的示例：

```powershell
$automationAccountName = "ContosoAutomationAccount"
$runbookName = "ChildRunbookExample"
$resourceGroupName = "ContosoRG"

function IsJobTerminalState([string] $status) {
    return $status -eq "Completed" -or $status -eq "Failed" -or $status -eq "Stopped" -or $status -eq "Suspended"
}

$job = Start-AzureRmAutomationRunbook -AutomationAccountName $automationAccountName -Name $runbookName -ResourceGroupName $resourceGroupName
$pollingSeconds = 5
$maxTimeout = 10800
$waitTime = 0
while((IsJobTerminalState $job.Status) -eq $false -and $waitTime -lt $maxTimeout) {
   Start-Sleep -Seconds $pollingSeconds
   $waitTime += $pollingSeconds
   $job = $job | Get-AzureRmAutomationJob
}

$jobResults | Get-AzureRmAutomationJobOutput | Get-AzureRmAutomationJobOutputRecord | Select-Object -ExpandProperty Value
```

## <a name="scenario-runbook-fails-because-of-deserialized-object"></a><a name="fails-deserialized-object"></a>场景：Runbook 因反序列化的对象而失败

### <a name="issue"></a>问题

Runbook 失败并显示错误：

```error
Cannot bind parameter <ParameterName>.

Cannot convert the <ParameterType> value of type Deserialized <ParameterType> to type <ParameterType>.
```

### <a name="cause"></a>原因

如果 runbook 为 PowerShell 工作流，它会将复杂对象以反序列化格式进行存储，以便在工作流暂停的情况下保留 runbook 状态。

### <a name="resolution"></a>解决方法

使用以下任一解决方法来解决此问题：

* 如果将复杂对象从一个 cmdlet 传送到另一个 cmdlet，请将这些 cmdlet 包装在 `InlineScript` 活动中。
* 传递复杂对象中你所需要的名称或值，不必传递整个对象。
* 使用 PowerShell Runbook，而不使用 PowerShell 工作流 Runbook。



## <a name="scenario-400-bad-request-status-when-calling-a-webhook"></a><a name="expired webhook"></a>场景：调用 Webhook 时显示“400 错误的请求”状态

### <a name="issue"></a>问题

在尝试调用 Azure 自动化 Runbook 的 Webhook 时收到以下错误：

```error
400 Bad Request : This webhook has expired or is disabled
```

### <a name="cause"></a>原因

尝试调用的 Webhook 已禁用，或者已过期。 

### <a name="resolution"></a>解决方法

如果 Webhook 处于禁用状态，可以通过 Azure 门户重新启用 Webhook。 如果 Webhook 已过期，必须将其删除，然后重新创建。 如果尚未过期，只能[续订 Webhook](../automation-webhooks.md#renew-a-webhook)。 

## <a name="scenario-429-the-request-rate-is-currently-too-large"></a><a name="429"></a>场景：429：当前的请求速率过大

### <a name="issue"></a>问题

在运行 `Get-AzureRmAutomationJobOutput` cmdlet 时收到以下错误消息：

```error
429: The request rate is currently too large. Please try again
```

### <a name="cause"></a>原因

从包含多个[详细流](../automation-runbook-output-and-messages.md#verbose-stream)的 Runbook 中检索作业输出时，可能会发生此错误。

### <a name="resolution"></a>解决方法

执行以下操作之一来解决此错误：

* 编辑 Runbook，并减少它发出的作业流数量。
* 减少运行 cmdlet 时要检索的流数量。 为此，可以设置 [Get-AzureRmAutomationJobOutput](https://docs.microsoft.com/powershell/module/azurerm.automation/get-azurermautomationjoboutput?view=azurermps-6.13.0) cmdlet 的 `Stream` 参数值，以仅检索输出流。 

## <a name="scenario-runbook-job-fails-because-allocated-quota-was-exceeded"></a><a name="quota-exceeded"></a>场景：Runbook 作业因超过了分配的配额而失败

### <a name="issue"></a>问题

Runbook 作业失败并显示错误：

```error
The quota for the monthly total job run time has been reached for this subscription
```

## <a name="scenario-runbook-job-start-attempted-three-times-but-fails-to-start-each-time"></a><a name="job-attempted-3-times"></a>场景：已尝试启动 Runbook 作业三次，但每次都失败

### <a name="issue"></a>问题

Runbook 失败并出现以下错误：

```error
The job was tried three times but it failed
```

### <a name="cause"></a>原因

此错误是由以下问题之一造成的：

* **内存限制。** 如果作业使用的内存超过 400 MB，则它可能会失败。 [自动化服务限制](../../azure-resource-manager/management/azure-subscription-service-limits.md#automation-limits)中阐述了分配给沙盒的内存限制。 
* **网络套接字。** Azure 沙盒限制为 1000 个并发网络套接字。 有关详细信息，请参阅[自动化服务限制](../../azure-resource-manager/management/azure-subscription-service-limits.md#automation-limits)。
* **未为沙盒设置 Active Directory 身份验证。** Runbook 尝试调用在 Azure 沙盒中运行的可执行文件或子过程。 不支持将 Runbook 配置为使用 Azure Active Directory 身份验证库 (ADAL) 对 Azure AD 进行身份验证。
* **异常数据过多。** runbook 尝试向输出流写入太多异常数据。

### <a name="resolution"></a>解决方法

* **内存限制，网络套接字。** 在内存限制内工作的建议方法是在多个 Runbook 之间拆分工作负荷，在内存中处理更少的数据，避免从 Runbook 写入不必要的输出，并考虑将多少个检查点写入 PowerShell 工作流 Runbook。 可以使用 clear 方法（例如 `$myVar.clear`）清除变量并使用 `[GC]::Collect` 立即运行垃圾回收。 这将减少运行时期间 runbook 的内存占用情况。
* **未为沙盒设置 Active Directory 身份验证。** 使用 Runbook 向 Azure AD 进行身份验证时，请确保 Azure AD 模块在自动化帐户中可用。 确保为运行方式帐户授予所需的权限，使其能够执行 Runbook 自动执行的任务。

  如果 Runbook 无法调用 Azure 沙盒中运行的可执行文件或子进程，请在[混合 Runbook 辅助角色](../automation-hrw-run-runbooks.md)中使用 Runbook。 混合辅助角色不受内存和网络限制，而 Azure 沙盒则受限于此限制。

* **异常数据过多。** 作业输出流限制为 1-MB。 确保 Runbook 使用 `try` 和 `catch` 块，这样就会包含对可执行文件或子进程的调用。 如果操作引发异常，请让代码将该异常中的消息写入自动化变量中。 此方法可防止将消息写入作业输出流中。

## <a name="scenario-powershell-job-fails-with-cannot-invoke-method-error-message"></a><a name="cannot-invoke-method"></a>场景：PowerShell 作业失败并出现“无法调用方法”错误消息

### <a name="issue"></a>问题

在 Runbook 中启动 PowerShell 作业时，如果该 Runbook 在 Azure 中运行，你会收到以下错误消息：

```error
Exception was thrown - Cannot invoke method. Method invocation is supported only on core types in this language mode.
```

### <a name="cause"></a>原因

此错误可能表示在 Azure 沙盒中运行的 Runbook 无法在[“完整语言”模式](https://docs.microsoft.com/powershell/module/microsoft.powershell.core/about/about_language_modes)下运行。

### <a name="resolution"></a>解决方法

可通过两种方法来解决此错误：

* 使用 [Start-AzureRmAutomationRunbook](https://docs.microsoft.com/powershell/module/azurerm.automation/start-azurermautomationrunbook?view=azurermps-6.13.0) 而非 [Start-Job](https://docs.microsoft.com/powershell/module/microsoft.powershell.core/start-job?view=powershell-7) 来启动 Runbook。
* 尝试在混合 Runbook 辅助角色中运行 Runbook。

若要详细了解 Azure 自动化 Runbook 的此行为和其他行为，请参阅 [Azure 自动化中的 Runbook 执行](../automation-runbook-execution.md)。

## <a name="scenario-a-long-running-runbook-fails-to-complete"></a><a name="long-running-runbook"></a>场景：长时间运行的 Runbook 无法完成

### <a name="issue"></a>问题

运行三小时后，Runbook 显示处于“已停止”状态。 此外，可能会收到以下错误：

```error
The job was evicted and subsequently reached a Stopped state. The job cannot continue running.
```

此行为是 Azure 沙盒的设计使然，因为 Azure 自动化中会对进程进行[公平份额](../automation-runbook-execution.md#fair-share)监视。 如果进程的执行时间超过三小时，“公平份额”会自动停止 Runbook。 超过公平份额时间限制的 runbook 的状态因 runbook 类型而异。 PowerShell 和 Python Runbook 设置为“已停止”状态。 PowerShell 工作流 Runbook 设置为“失败”。

### <a name="cause"></a>原因

Runbook 运行时间超出了 Azure 沙盒中公平份额允许的三小时限制。

### <a name="resolution"></a>解决方法

建议的解决方案是在[混合 Runbook 辅助角色](../automation-hrw-run-runbooks.md)上运行 runbook。 混合辅助角色不受 Azure 沙盒的三小时公平份额 Runbook 限制。 应开发在混合 Runbook 辅助角色上运行的 Runbook，以便在出现意外的本地基础结构问题时支持重启行为。

另一种解决方法是通过创建[子 Runbook](../automation-child-runbooks.md) 来优化 Runbook。 如果 Runbook 在多个资源上循环访问同一函数（例如，对多个数据库上执行某个数据库操作），可将该函数移到子 Runbook。 每个子 Runbook 在单独的进程中并行执行。 此行为降低了完成父 runbook 所需的时间总量。

启用子 runbook 方案的 PowerShell cmdlet 是：

* [Start-AzureRmAutomationRunbook](https://docs.microsoft.com/powershell/module/azurerm.automation/start-azurermautomationrunbook?view=azurermps-6.13.0)。 此 cmdlet 用于启动 Runbook 并将参数传递给该 Runbook。
* [Get-AzureRmAutomationJob](https://docs.microsoft.com/powershell/module/azurerm.automation/get-azurermautomationjob?view=azurermps-6.13.0)。 如果在子 runbook 完成后需要执行操作，可使用此 cmdlet 检查每个子 runbook 的作业状态。

## <a name="scenario-access-denied-when-using-azure-sandbox-for-runbook-or-application"></a><a name="access-denied-azure-sandbox"></a>场景：对 Runbook 或应用程序使用 Azure 沙盒时出现“拒绝访问”

### <a name="issue"></a>问题

当 Runbook 或应用程序尝试在 Azure 沙盒中运行时，环境拒绝其访问。

### <a name="cause"></a>原因

之所以会出现此问题，是因为 Azure 沙盒不允许访问所有的进程外 COM 服务器。 例如，沙盒应用程序或 Runbook 无法调用 Windows Management Instrumentation (WMI) 或 Windows Installer 服务 (msiserver.exe)。 

### <a name="resolution"></a>解决方法

若要详细了解如何使用 Azure 沙盒，请参阅 [Runbook 执行环境](../automation-runbook-execution.md#runbook-execution-environment)。

## <a name="scenario-invalid-forbidden-status-code-when-using-key-vault-inside-a-runbook"></a>方案：在 Runbook 中使用 Key Vault 时出现“无效且被禁止”状态代码

### <a name="issue"></a>问题

尝试通过 Azure 自动化 Runbook 访问 Azure Key Vault 时遇到以下错误：

```error
Operation returned an invalid status code 'Forbidden'
```

### <a name="cause"></a>原因

此问题的可能原因包括：

* 未使用运行方式帐户。
* 权限不足。

### <a name="resolution"></a>解决方法

#### <a name="not-using-a-run-as-account"></a>未使用运行方式帐户

按[步骤 5 - 添加身份验证来管理 Azure 资源](/automation/automation-first-runbook-textual-powershell#add-authentication-to-manage-azure-resources)的要求操作，以确保使用运行方式帐户访问 Key Vault。

#### <a name="insufficient-permissions"></a>权限不足

[将权限添加到 Key Vault](/automation/manage-runas-account#add-permissions-to-key-vault)，以确保运行方式帐户拥有足够的权限，可以访问 Key Vault。

## <a name="recommended-documents"></a>建议的文档

* [在 Azure 自动化中执行 Runbook](../automation-runbook-execution.md)
* [在 Azure 自动化中启动 Runbook](/automation/automation-starting-a-runbook)

## <a name="next-steps"></a>后续步骤

如果你的问题未在本文中列出，或者你无法解决自己的问题，请尝试通过以下渠道之一获取更多支持：

* 通过 [Azure 论坛](https://social.msdn.microsoft.com/Forums/zh-cn/home)获取 Azure 专家的解答
* 如需更多帮助，可以提交 Azure 支持事件。 转到 [Azure 支持站点](https://www.azure.cn/support/)。