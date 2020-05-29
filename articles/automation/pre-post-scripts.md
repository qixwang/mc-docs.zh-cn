---
title: 在 Azure 中的更新管理部署中管理前脚本和后脚本
description: 本文介绍如何配置和管理更新部署的前脚本和后脚本。
services: automation
ms.subservice: update-management
origin.date: 05/17/2019
ms.date: 05/25/2020
ms.topic: conceptual
ms.openlocfilehash: 0dc0dd80de9ea15d84eee55bc60e5ed5aff84639
ms.sourcegitcommit: 981a75a78f8cf74ab5a76f9e6b0dc5978387be4b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/22/2020
ms.locfileid: "83801912"
---
# <a name="manage-pre-scripts-and-post-scripts"></a>管理前脚本和后脚本

在执行更新部署之前（前任务）和之后（后任务），前脚本和后脚本是在 Azure 自动化帐户中运行的 Runbook。 前脚本和后脚本在 Azure 上下文中运行，而不是在本地运行。 前脚本在更新部署开始时运行。 后脚本在部署结束时以及在配置的任何重新启动之后运行。

## <a name="pre-script-and-post-script-requirements"></a>前脚本和后脚本要求

要将某个 Runbook 用作前脚本或后脚本，必须将其导入到自动化帐户中并[发布这个 Runbook](manage-runbooks.md#publish-a-runbook)。

## <a name="pre-script-and-post-script-parameters"></a>前脚本和后脚本参数

配置前脚本和后脚本时，可以像计划 Runbook 时一样传入参数。 参数是在创建更新部署时定义的。 前脚本和后脚本支持以下类型：

* [char]
* [byte]
* [int]
* [long]
* [decimal]
* [single]
* [double]
* [DateTime]
* [string]

前脚本和后脚本 Runbook 参数不支持布尔、对象或数组类型。 这些值会导致 Runbook 失败。 

如果需要其他对象类型，可以在 runbook 中使用自己的逻辑将它强制转换为其他类型。

除了标准的 Runbook 参数以外，还提供了一个 `SoftwareUpdateConfigurationRunContext` 参数（类型为 JSON 字符串）。 如果在前脚本或后脚本 Runbook 中定义该参数，更新部署会自动传入该参数。 该参数包含有关更新部署的信息（[SoftwareUpdateconfigurations API](https://docs.microsoft.com/rest/api/automation/softwareupdateconfigurations/getbyname#updateconfiguration) 返回的信息的子集）。 以下部分定义关联的属性。

### <a name="softwareupdateconfigurationruncontext-properties"></a>SoftwareUpdateConfigurationRunContext 属性

|属性  |说明  |
|---------|---------|
|SoftwareUpdateConfigurationName     | 软件更新配置的名称。        |
|SoftwareUpdateConfigurationRunId     | 运行的唯一 ID。        |
|SoftwareUpdateConfigurationSettings     | 与软件更新配置相关的属性集合。         |
|SoftwareUpdateConfigurationSettings.operatingSystem     | 面向更新部署的操作系统。         |
|SoftwareUpdateConfigurationSettings.duration     | 符合 ISO8601 的更新部署最长运行持续时间，格式为 `PT[n]H[n]M[n]S`，也称为“维护时段”。          |
|SoftwareUpdateConfigurationSettings.Windows     | 与 Windows 计算机相关的属性集合。         |
|SoftwareUpdateConfigurationSettings.Windows.excludedKbNumbers     | 从更新部署中排除的 KB 数列表。        |
|SoftwareUpdateConfigurationSettings.Windows.includedUpdateClassifications     | 为更新部署选择的更新分类。        |
|SoftwareUpdateConfigurationSettings.Windows.rebootSetting     | 更新部署的重新启动设置。        |
|azureVirtualMachines     | 更新部署中 Azure VM 的 resourceIds 列表。        |
|nonAzureComputerNames|更新部署中的非 Azure 计算机 FQDN 列表。|

下面是传入 **SoftwareUpdateConfigurationRunContext** 参数的 JSON 字符串示例：

```json
"SoftwareUpdateConfigurationRunContext":{
      "SoftwareUpdateConfigurationName":"sampleConfiguration",
      "SoftwareUpdateConfigurationRunId":"00000000-0000-0000-0000-000000000000",
      "SoftwareUpdateConfigurationSettings":{
         "operatingSystem":"Windows",
         "duration":"PT2H0M",
         "windows":{
            "excludedKbNumbers":[
               "168934",
               "168973"
            ],
            "includedUpdateClassifications":"Critical",
            "rebootSetting":"IfRequired"
         },
         "azureVirtualMachines":[
            "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myresources/providers/Microsoft.Compute/virtualMachines/vm-01",
            "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myresources/providers/Microsoft.Compute/virtualMachines/vm-02",
            "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myresources/providers/Microsoft.Compute/virtualMachines/vm-03"
         ],
         "nonAzureComputerNames":[
            "box1.contoso.com",
            "box2.contoso.com"
         ]
      }
   }
```

可以在以下位置找到包含所有属性的完整示例：[按名称获取软件更新配置](https://docs.microsoft.com/rest/api/automation/softwareupdateconfigurations/getbyname#examples)。

> [!NOTE]
> `SoftwareUpdateConfigurationRunContext` 对象可以包含计算机的重复项。 这可能会导致在同一台计算机上多次运行前脚本和后脚本。 若要解决此行为，请使用 `Sort-Object -Unique` 仅选择唯一的 VM 名称。

## <a name="using-a-pre-script-or-post-script-in-a-deployment"></a>在部署中使用脚本前脚本或后脚本

若要在更新部署中使用前脚本或后脚本，请先创建一个更新部署。 选择“前脚本 + 后脚本”。 此操作会打开“选择前脚本 + 后脚本”页面。

![选择脚本](./media/pre-post-scripts/select-scripts.png)

选择要使用的脚本。 在此示例中，我们将使用 UpdateManagement-TurnOnVms Runbook。 选择 Runbook 时，将打开“配置脚本”页。 选择“前脚本”，然后选择“确定” 。

针对 **UpdateManagement-TurnOffVms** 脚本重复此过程。 但是如果选择“脚本类型”，请选择“后脚本” 。

“选定的项”部分现在将显示所选的脚本。 一个是前脚本，另一个是后脚本：

![选定的项](./media/pre-post-scripts/selected-items.png)

完成更新部署的配置。

更新部署完成后，可以转到“更新部署”查看结果。 可以看到，此处提供了前脚本和后脚本的状态：

![更新结果](./media/pre-post-scripts/update-results.png)

选择更新部署运行后，你会看到前脚本和后脚本的其他详细信息。 此时会提供运行时的脚本源的链接。

![部署运行](./media/pre-post-scripts/deployment-run.png)

脚本中结果。

## <a name="stopping-a-deployment"></a>停止部署

如果要基于前脚本停止部署，则必须[引发](automation-runbook-execution.md#throw)异常。 否则，部署和后脚本仍将运行。 下面的代码片段演示如何引发异常。

```powershell
#In this case, we want to terminate the patch job if any run fails.
#This logic might not hold for all cases - you might want to allow success as long as at least 1 run succeeds
foreach($summary in $finalStatus)
{
    if ($summary.Type -eq "Error")
    {
        #We must throw in order to fail the patch deployment.
        throw $summary.Summary
    }
}
```



## <a name="interacting-with-machines"></a>与计算机交互

前脚本和后任务在自动化帐户中作为 Runbook 运行，而不是直接在部署中的计算机上运行。 前任务和后任务还可在 Azure 上下文中运行，无法访问非 Azure 计算机。 以下部分介绍如何直接与计算机交互，无论是 Azure VM 还是非 Azure 计算机。

### <a name="interact-with-azure-machines"></a>与 Azure 计算机交互

前任务和后任务作为 Runbook 运行，而不是在部署中的 Azure VM 上以本机的方式运行。 若要与 Azure VM 进行交互，必须具有以下项：

* 一个运行方式帐户
* 要运行的 Runbook

若要与 Azure 计算机交互，应使用 [Invoke-AzVMRunCommand](https://docs.microsoft.com/powershell/module/az.compute/invoke-azvmruncommand?view=azps-3.7.0) cmdlet 与 Azure VM 进行交互。 有关如何执行此操作的示例，请参阅 Runbook 示例[更新管理 - 使用 Run 命令运行脚本](https://gallery.technet.microsoft.com/Update-Management-Run-40f470dc)。

### <a name="interact-with-non-azure-machines"></a>与非 Azure 计算机交互

前任务和后任务在 Azure 上下文中运行，无法访问非 Azure 计算机。 若要与非 Azure 计算机进行交互，必须具有以下项：

* 一个运行方式帐户
* 在计算机上安装的混合 Runbook 辅助角色
* 要在本地运行的 Runbook
* 父 Runbook

若要与非 Azure 计算机交互，需在 Azure 上下文中运行一个父 Runbook。 此 Runbook 使用 [Start-AzureRmAutomationRunbook](https://docs.microsoft.com/powershell/module/azurerm.automation/start-azurermautomationrunbook?view=azurermps-6.13.0) cmdlet 调用子 Runbook。 必须指定 `RunOn` 参数，并提供运行脚本的混合 Runbook 辅助角色的名称。 请参阅 Runbook 示例[更新管理 - 本地运行脚本](https://gallery.technet.microsoft.com/Update-Management-Run-6949cc44)。

## <a name="aborting-patch-deployment"></a>中止修补程序部署

如果你的前脚本返回错误，建议中止部署。 若要执行此操作，必须在脚本中[引发](https://docs.microsoft.com/powershell/module/microsoft.powershell.core/about/about_throw)错误，以查找导致失败的任何逻辑。

```powershell
if (<My custom error logic>)
{
    #Throw an error to fail the patch deployment.
    throw "There was an error, abort deployment"
}
```

## <a name="samples"></a>示例

可以在[脚本中心库](https://gallery.technet.microsoft.com/scriptcenter/site/search?f%5B0%5D.Type=RootCategory&f%5B0%5D.Value=WindowsAzure&f%5B0%5D.Text=Windows%20Azure&f%5B1%5D.Type=SubCategory&f%5B1%5D.Value=WindowsAzure_automation&f%5B1%5D.Text=Automation&f%5B2%5D.Type=SearchText&f%5B2%5D.Value=update%20management&f%5B3%5D.Type=Tag&f%5B3%5D.Value=Patching&f%5B3%5D.Text=Patching&f%5B4%5D.Type=ProgrammingLanguage&f%5B4%5D.Value=PowerShell&f%5B4%5D.Text=PowerShell)和 [PowerShell 库](https://www.powershellgallery.com/packages?q=Tags%3A%22UpdateManagement%22+Tags%3A%22Automation%22)中找到前脚本和后脚本的示例。

![库列表](./media/pre-post-scripts/runbook-gallery.png)

也可按脚本名称搜索这些示例，如以下列表中所示：

* 更新管理 - 启用 VM
* 更新管理 - 禁用 VM
* 更新管理 - 本地运行脚本
* 更新管理 - 前脚本/后脚本的模板
* 更新管理 - 具有 Run 命令运行脚本

> [!IMPORTANT]
> 导入 Runbook 后，必须先发布，然后才可以使用它们。 为此，请在自动化帐户中找到该 Runbook，选择“编辑”，然后选择“发布” 。

所有这些示例都基于以下示例中定义的基本模板。 使用此模板可以创建自己的 Runbook 来配合前脚本和后脚本。 此模板还包含了用于在 Azure 中进行身份验证以及处理 `SoftwareUpdateConfigurationRunContext` 参数的必需逻辑。

```powershell
<#
.SYNOPSIS
 Barebones script for Update Management Pre/Post

.DESCRIPTION
  This script is intended to be run as a part of Update Management pre/post-scripts.
  It requires a RunAs account.

.PARAMETER SoftwareUpdateConfigurationRunContext
  This is a system variable which is automatically passed in by Update Management during a deployment.
#>

param(
    [string]$SoftwareUpdateConfigurationRunContext
)
#region BoilerplateAuthentication
#This requires a RunAs account
$ServicePrincipalConnection = Get-AutomationConnection -Name 'AzureRunAsConnection'

Add-AzureRmAccount `
    -EnvironmentName AzureChinaCloud `
    -ServicePrincipal `
    -TenantId $ServicePrincipalConnection.TenantId `
    -ApplicationId $ServicePrincipalConnection.ApplicationId `
    -CertificateThumbprint $ServicePrincipalConnection.CertificateThumbprint

$AzureContext = Select-AzureRmSubscription -SubscriptionId $ServicePrincipalConnection.SubscriptionID
#endregion BoilerplateAuthentication

#If you wish to use the run context, it must be converted from JSON
$context = ConvertFrom-Json  $SoftwareUpdateConfigurationRunContext
#Access the properties of the SoftwareUpdateConfigurationRunContext
$vmIds = $context.SoftwareUpdateConfigurationSettings.AzureVirtualMachines | Sort-Object -Unique
$runId = $context.SoftwareUpdateConfigurationRunId

Write-Output $context

#Example: How to create and write to a variable using the pre-script:
<#
#Create variable named after this run so it can be retrieved
New-AzureRmAutomationVariable -ResourceGroupName $ResourceGroup –AutomationAccountName $AutomationAccount –Name $runId -Value "" –Encrypted $false
#Set value of variable
Set-AutomationVariable –Name $runId -Value $vmIds
#>

#Example: How to retrieve information from a variable set during the pre-script
<#
$variable = Get-AutomationVariable -Name $runId
#>
```

> [!NOTE]
> 对于非图形 PowerShell Runbook，`Add-AzureRMAccount` 是 [Connect-AzureRmAccount](https://docs.microsoft.com/powershell/module/azurerm.profile/connect-azurermaccount?view=azurermps-6.13.0) 的别名。

## <a name="next-steps"></a>后续步骤

请继续学习以下教程，了解如何管理 Windows 虚拟机的更新：

> [!div class="nextstepaction"]
> [管理 Azure Windows VM 的更新和修补程序](automation-tutorial-update-management.md)