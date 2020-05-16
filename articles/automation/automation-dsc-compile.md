---
title: 在 Azure Automation State Configuration 中编译 DSC 配置
description: 本文介绍如何为 Azure 自动化编译 Desired State Configuration (DSC) 配置。
services: automation
ms.subservice: dsc
origin.date: 04/06/2020
ms.date: 05/11/2020
ms.topic: conceptual
ms.openlocfilehash: 987dafbd82e0c884aee96591fe6390eb53faad53
ms.sourcegitcommit: 7443ff038ea8afe511f7419d9c550d27fb642246
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/09/2020
ms.locfileid: "83001649"
---
# <a name="compile-dsc-configurations-in-azure-automation-state-configuration"></a>在 Azure Automation State Configuration 中编译 DSC 配置

可通过以下方式在 Azure 自动化 State Configuration 中编译 Desired State Configuration (DSC) 配置：

- Azure State Configuration 编译服务
  - 使用交互式用户界面的初级方法
   - 轻松跟踪作业状态

- Windows PowerShell
  - 从本地工作站上的 Windows PowerShell 调用或生成服务
  - 与开发测试管道集成
  - 提供复杂的参数值
  - 大规模使用节点和非节点数据
  - 显著的性能提升

还可使用包含 Azure Desired State Configuration (DSC) 扩展的 Azure 资源管理器模板将配置推送到 Azure VM。 Azure DSC 扩展使用 Azure VM 代理框架来传送、启用和报告 Azure VM 上运行的 DSC 配置。 有关使用 Azure 资源管理器模板的编译详细信息，请参阅[将 Desired State Configuration 扩展与 Azure 资源管理器模板配合使用](/virtual-machines/extensions/dsc-template#details)。 

## <a name="compiling-a-dsc-configuration-in-azure-state-configuration"></a>在 Azure State Configuration 中编译 DSC 配置

### <a name="portal"></a>门户

1. 在自动化帐户中，单击“State Configuration (DSC)”  。
1. 单击“配置”选项卡，然后单击要编译的配置名称。 
1. 单击“编译”  。
1. 如果该配置没有参数，系统会提示确认是否要进行编译。 如果该配置有参数，则会打开“编译配置”  边栏选项卡，使你能够提供参数值。
1. 此时会打开“编译作业”页，可在其中跟踪编译作业的状态。 还可以使用此页来跟踪放置在 Azure 自动化 State Configuration 拉取服务器上的节点配置（MOF 配置文档）。

### <a name="azure-powershell"></a>Azure PowerShell

可以在 Windows PowerShell 中使用 [Start-AzAutomationDscCompilationJob](https://docs.microsoft.com/powershell/module/az.automation/start-azautomationdsccompilationjob) 开始编译。 以下示例代码开始编译名为“SampleConfig”  的 DSC 配置。

```powershell
Start-AzAutomationDscCompilationJob -ResourceGroupName 'MyResourceGroup' -AutomationAccountName 'MyAutomationAccount' -ConfigurationName 'SampleConfig'
```

`Start-AzAutomationDscCompilationJob` 返回可用于跟踪作业状态的编译作业对象。 然后，可以在 [Get-AzAutomationDscCompilationJob](https://docs.microsoft.com/powershell/module/az.automation/get-azautomationdsccompilationjob) 中使用此编译作业对象来确定编译作业的状态，并使用 [Get-AzAutomationDscCompilationJobOutput](https://docs.microsoft.com/powershell/module/az.automation/get-azautomationdscconfiguration) 查看其流（输出）。 以下示例启动 SampleConfig 配置的编译，并在编译完成后显示其流。

```powershell
$CompilationJob = Start-AzAutomationDscCompilationJob -ResourceGroupName 'MyResourceGroup' -AutomationAccountName 'MyAutomationAccount' -ConfigurationName 'SampleConfig'

while($null -eq $CompilationJob.EndTime -and $null -eq $CompilationJob.Exception)
{
    $CompilationJob = $CompilationJob | Get-AzAutomationDscCompilationJob
    Start-Sleep -Seconds 3
}

$CompilationJob | Get-AzAutomationDscCompilationJobOutput –Stream Any
```

### <a name="declare-basic-parameters"></a>声明基本参数

DSC 配置中的参数声明（包括参数类型和属性）的工作方式与 Azure 自动化 Runbook 中相同。 若要详细了解 Runbook 参数，请参阅 [在 Azure 自动化中启动 Runbook](automation-starting-a-runbook.md) 。

以下示例使用 `FeatureName` 和 `IsPresent` 参数确定在编译期间生成的 ParametersExample.sample  节点配置中的属性值。

```powershell
Configuration ParametersExample
{
    param(
        [Parameter(Mandatory=$true)]
        [string] $FeatureName,

        [Parameter(Mandatory=$true)]
        [boolean] $IsPresent
    )

    $EnsureString = 'Present'
    if($IsPresent -eq $false)
    {
        $EnsureString = 'Absent'
    }

    Node 'sample'
    {
        WindowsFeature ($FeatureName + 'Feature')
        {
            Ensure = $EnsureString
            Name   = $FeatureName
        }
    }
}
```

可以在 Azure 自动化 State Configuration 门户或 Azure PowerShell 中编译使用基本参数的 DSC 配置。

#### <a name="portal"></a>门户

在门户中，可在单击“编译”  后输入参数值。

![配置编译参数](./media/automation-dsc-compile/DSC_compiling_1.png)

#### <a name="azure-powershell"></a>Azure PowerShell

PowerShell 要求将参数放入[哈希表](https://docs.microsoft.com/powershell/module/microsoft.powershell.core/about/about_hash_tables)中，其中的键必须与参数名称匹配，值等于参数值。

```powershell
$Parameters = @{
    'FeatureName' = 'Web-Server'
    'IsPresent' = $False
}

Start-AzAutomationDscCompilationJob -ResourceGroupName 'MyResourceGroup' -AutomationAccountName 'MyAutomationAccount' -ConfigurationName 'ParametersExample' -Parameters $Parameters
```

若要了解如何以参数形式传递 `PSCredential` 对象，请参阅[凭据资产](#credential-assets)。

### <a name="compile-configurations-containing-composite-resources-in-azure-automation"></a>在 Azure 自动化中编译包含复合资源的配置

借助“复合资源”功能，可将 DSC 配置用作某个配置中的嵌套资源。  使用此功能可将多个配置应用到单个资源。 请参阅[复合资源：将 DSC 配置用作资源](https://docs.microsoft.com/powershell/scripting/dsc/resources/authoringresourcecomposite)，了解有关复合资源的详细信息。

> [!NOTE]
> 若要正确编译包含复合资源的配置，首先必须将复合资源所依赖的所有 DSC 资源导入到 Azure 自动化中。

### <a name="manage-configurationdata-when-compiling-configurations-in-azure-automation"></a>在 Azure 自动化中编译配置时管理 ConfigurationData

`ConfigurationData` 是一个内置 DSC 参数，可让你在使用 PowerShell DSC 时将结构配置与任何特定于环境的配置区分开来。 有关详细信息，请参阅[区分 PowerShell DSC 中的“What”与“Where”](https://devblogs.microsoft.com/powershell/separating-what-from-where-in-powershell-dsc/)。

> [!NOTE]
> 在 Azure Automation State Configuration 中进行编译时，可以在 Azure PowerShell 中使用 `ConfigurationData`，但不能在 Azure 门户中使用它。

以下示例 DSC 配置通过 `$ConfigurationData` 和 `$AllNodes` 关键字使用 `ConfigurationData`。 此示例还需要 [xWebAdministration 模块](https://www.powershellgallery.com/packages/xWebAdministration/)。

```powershell
Configuration ConfigurationDataSample
{
    Import-DscResource -ModuleName xWebAdministration -Name MSFT_xWebsite

    Write-Verbose $ConfigurationData.NonNodeData.SomeMessage

    Node $AllNodes.Where{$_.Role -eq 'WebServer'}.NodeName
    {
        xWebsite Site
        {
            Name         = $Node.SiteName
            PhysicalPath = $Node.SiteContents
            Ensure       = 'Present'
        }
    }
}
```

可以使用 Windows PowerShell 编译上述 DSC 配置。 以下脚本将两个节点配置添加到 Azure 自动化 State Configuration 拉取服务：ConfigurationDataSample.MyVM1  和 ConfigurationDataSample.MyVM3  。

```powershell
$ConfigData = @{
    AllNodes = @(
        @{
            NodeName = 'MyVM1'
            Role = 'WebServer'
        },
        @{
            NodeName = 'MyVM2'
            Role = 'SQLServer'
        },
        @{
            NodeName = 'MyVM3'
            Role = 'WebServer'
        }
    )

    NonNodeData = @{
        SomeMessage = 'I love Azure Automation State Configuration and DSC!'
    }
}

Start-AzAutomationDscCompilationJob -ResourceGroupName 'MyResourceGroup' -AutomationAccountName 'MyAutomationAccount' -ConfigurationName 'ConfigurationDataSample' -ConfigurationData $ConfigData
```

### <a name="work-with-assets-in-azure-automation-during-compilation"></a>编译期间在 Azure 自动化中使用资产

Azure 自动化 State Configuration 和 Runbook 中的资产引用是相同的。 有关详细信息，请参阅以下部分：

- [Certificates](automation-certificates.md)
- [连接](automation-connections.md)
- [凭据](automation-credentials.md)
- [变量](automation-variables.md)

#### <a name="credential-assets"></a>凭据资产

Azure 自动化中的 DSC 配置可以使用 `Get-AutomationPSCredential` cmdlet 引用自动化凭据资产。 如果配置的参数指定了 `PSCredential` 对象，请使用 `Get-AutomationPSCredential`，方法是将 Azure 自动化凭据资产的字符串名称传递给该 cmdlet 以检索凭据。 然后对需要 `PSCredential` 对象的参数使用该对象。 在后台将检索具有该名称的 Azure 自动化凭据资产并将其传递给配置。 以下示例演示了此方案的运作方式。

要在节点配置（MOF 配置文档）中保持凭据的安全，需要在节点配置 MOF 文件中为凭据加密。 目前，必须授予 PowerShell DSC 在生成节点配置 MOF 期间以纯文本形式输出凭据的权限。 PowerShell DSC 并不知道在通过编译作业生成 MOF 文件之后 Azure 自动化将加密整个文件。

你可以告知 PowerShell DSC：可以使用配置数据在生成的节点配置 MOF 中以纯文本格式输出凭据。 对于每个出现在 DSC 配置中且使用凭据的节点块名称，应该通过 `ConfigurationData` 传递 `PSDscAllowPlainTextPassword = $true`。

以下示例演示使用自动化凭据资产的 DSC 配置。

```powershell
Configuration CredentialSample
{
    Import-DscResource -ModuleName PSDesiredStateConfiguration
    $Cred = Get-AutomationPSCredential 'SomeCredentialAsset'

    Node $AllNodes.NodeName
    {
        File ExampleFile
        {
            SourcePath      = '\\Server\share\path\file.ext'
            DestinationPath = 'C:\destinationPath'
            Credential      = $Cred
        }
    }
}
```

可以使用 PowerShell 编译上述 DSC 配置。 以下 PowerShell 代码将两个节点配置添加到 Azure 自动化 State Configuration 拉取服务器：**CredentialSample.MyVM1** 和 **CredentialSample.MyVM2**。

```powershell
$ConfigData = @{
    AllNodes = @(
        @{
            NodeName = '*'
            PSDscAllowPlainTextPassword = $True
        },
        @{
            NodeName = 'MyVM1'
        },
        @{
            NodeName = 'MyVM2'
        }
    )
}

Start-AzAutomationDscCompilationJob -ResourceGroupName 'MyResourceGroup' -AutomationAccountName 'MyAutomationAccount' -ConfigurationName 'CredentialSample' -ConfigurationData $ConfigData
```

> [!NOTE]
> 编译完成后，可能会收到错误消息 `The 'Microsoft.PowerShell.Management' module was not imported because the 'Microsoft.PowerShell.Management' snap-in was already imported.`。你可以放心地忽略此消息。

## <a name="compiling-your-dsc-configuration-in-windows-powershell"></a>在 Windows PowerShell 中编译 DSC 配置

还可以导入已在 Azure 外部编译的节点配置（MOF 文件）。 导入内容包括开发人员工作站或 [Azure DevOps](https://dev.azure.com) 等服务中的编译内容。 此方法有多个缺点，其中包括性能和可靠性方面的缺点。

在 Windows PowerShell 中编译还提供了签署配置内容的选项。 DSC 代理将验证托管节点上本地签名的节点配置。 验证可确保应用到节点的配置来自经过授权的源。

> [!NOTE]
> 节点配置文件不得大于 1 MB，这样 Azure 自动化才能将其导入。

有关为节点配置签名的详细信息，请参阅 [WMF 5.1 中的改进 - 如何为配置和模块签名](https://docs.microsoft.com/powershell/scripting/wmf/whats-new/dsc-improvements#dsc-module-and-configuration-signing-validations)。

### <a name="compile-the-dsc-configuration"></a>编译 DSC 配置

在 Windows PowerShell 中编译 DSC 配置的过程包含在以下 PowerShell DSC 文档中：[编写、编译和应用配置](https://docs.microsoft.com/powershell/scripting/dsc/configurations/write-compile-apply-configuration#compile-the-configuration)。
可以从开发人员工作站或在生成服务（例如 [Azure DevOps](https://dev.azure.com)）内部执行此过程。 然后，可以将通过编译配置生成的 MOF 文件导入到 Azure State Configuration 服务中。

### <a name="import-a-node-configuration-in-the-azure-portal"></a>在 Azure 门户中导入节点配置

1. 在“自动化帐户”中，单击“配置管理”下的“State Configuration (DSC)”。  
1. 在“State Configuration (DSC)”页上单击“配置”选项卡，然后单击“添加”。  
1. 在“导入”页上，单击“节点配置文件”字段旁边的文件夹图标，在本地计算机上浏览节点配置 MOF 文件。 

   ![以浏览方式查找本地文件](./media/automation-dsc-compile/import-browse.png)

1. 在“配置名称”字段中输入名称。  此名称必须与编译节点配置的配置名称匹配。
1. 单击 **“确定”** 。

### <a name="import-a-node-configuration-with-azure-powershell"></a>使用 Azure PowerShell 导入节点配置

可以使用 [Import-AzAutomationDscNodeConfiguration](https://docs.microsoft.com/powershell/module/az.automation/import-azautomationdscnodeconfiguration) cmdlet 将节点配置导入自动化帐户中。

```powershell
Import-AzAutomationDscNodeConfiguration -AutomationAccountName 'MyAutomationAccount' -ResourceGroupName 'MyResourceGroup' -ConfigurationName 'MyNodeConfiguration' -Path 'C:\MyConfigurations\TestVM1.mof'
```

## <a name="next-steps"></a>后续步骤

- 有关入门信息，请参阅 [Azure 自动化 State Configuration 入门](automation-dsc-getting-started.md)。
- 若要了解如何编译 DSC 配置，以便将它们分配给目标节点，请参阅[在 Azure 自动化 State Configuration 中编译配置](automation-dsc-compile.md)。
- 有关 PowerShell cmdlet 参考，请参阅 [Azure 自动化 State Configuration cmdlet](https://docs.microsoft.com/powershell/module/az.automation)。
- 有关定价信息，请参阅 [Azure Automation State Configuration 定价](https://azure.cn/pricing/details/automation/)
- 若要查看在持续部署管道中使用 Azure 自动化 State Configuration 的示例，请参阅[使用 Azure 自动化 State Configuration 和 Chocolatey 持续部署到虚拟机](automation-dsc-cd-chocolatey.md)。
