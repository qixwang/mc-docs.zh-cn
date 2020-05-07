---
title: 如何创建适用于 Windows 的 Guest Configuration 策略
description: 了解如何创建适用于 Windows 的 Azure Policy Guest Configuration 策略。
ms.author: v-tawe
origin.date: 03/20/2019
ms.date: 04/20/2020
ms.topic: how-to
ms.openlocfilehash: dcae4ae1ffd432716630646495df98ac511e12df
ms.sourcegitcommit: 89ca2993f5978cd6dd67195db7c4bdd51a677371
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/30/2020
ms.locfileid: "82588767"
---
# <a name="how-to-create-guest-configuration-policies-for-windows"></a>如何创建适用于 Windows 的 Guest Configuration 策略

在创建自定义策略之前，最好是先阅读 [Azure Policy Guest Configuration](../concepts/guest-configuration.md) 页中的概念概述信息。
 
若要了解如何创建适用于 Linux 的 Guest Configuration 策略，请参阅[如何创建适用于 Linux 的 Guest Configuration 策略](./guest-configuration-create-linux.md)页

审核 Windows 时，Guest Configuration 将使用 [Desired State Configuration](https://docs.microsoft.com/powershell/scripting/dsc/overview/overview) (DSC) 资源模块和配置文件。 DSC 配置定义计算机的应有状态。
如果配置评估失败，则会触发策略效应 auditIfNotExists，并将计算机视为不合规。  

[Azure Policy Guest Configuration](../concepts/guest-configuration.md) 只可用于审核计算机内部的设置。 目前尚未提供修正计算机内部设置的功能。

使用以下操作创建自己的配置用于验证 Azure 或非 Azure 计算机的状态。

> [!IMPORTANT]
> 使用 Guest Configuration 的自定义策略是一项预览版功能。

## <a name="install-the-powershell-module"></a>安装 PowerShell 模块

在 PowerShell 中使用 Guest Configuration 模块创建 Guest Configuration 项目、自动测试项目、创建策略定义和发布策略的过程完全可自动化。 该模块可以安装在运行 Windows、macOS 或 Linux 并装有 PowerShell 6.2 或更高版本的计算机本地，或者与 [Azure PowerShell Core Docker 映像](https://hub.docker.com/r/azuresdk/azure-powershell-core)一起安装。

> [!NOTE]
> Linux 尚不支持配置编译。

### <a name="base-requirements"></a>基本要求

可安装该模块的操作系统：

- Linux
- macOS
- Windows

Guest Configuration 资源模块需要以下软件：

- PowerShell 6.2 或更高版本。 若尚未安装，请遵循[这些说明](https://docs.microsoft.com/powershell/scripting/install/installing-powershell)。
- Azure PowerShell 1.5.0 或更高版本。 若尚未安装，请遵循[这些说明](https://docs.microsoft.com/powershell/azure/install-az-ps)。
  - 只有 AZ 模块“Az.Accounts”和“Az.Resources”是必需的。

### <a name="install-the-module"></a>安装模块

若要在 PowerShell 中安装 GuestConfiguration 模块： 

1. 在 PowerShell 提示符下，运行以下命令：

   ```powershell
   # Install the Guest Configuration DSC resource module from PowerShell Gallery
   Install-Module -Name GuestConfiguration
   ```

1. 验证是否已导入该模块：

   ```powershell
   # Get a list of commands for the imported GuestConfiguration module
   Get-Command -Module 'GuestConfiguration'
   ```

## <a name="guest-configuration-artifacts-and-policy-for-windows"></a>适用于 Windows 的 Guest Configuration 项目和策略

Guest Configuration 使用 PowerShell Desired State Configuration 作为语言抽象来编写要在 Windows 中审核的内容。 代理将加载 PowerShell 6.2 的独立实例，因此，在 Windows PowerShell 5.1 中使用 PowerShell DSC 不会产生冲突，并且不要求预先安装 PowerShell 6.2 或更高版本。

有关 DSC 的概念和术语概述，请参阅 [PowerShell DSC 概述](https://docs.microsoft.com/powershell/scripting/dsc/overview/overview)。

### <a name="how-guest-configuration-modules-differ-from-windows-powershell-dsc-modules"></a>Guest Configuration 模块与 Windows PowerShell DSC 模块的差别

当 Guest Configuration 审核计算机时：

1. 代理首先运行 `Test-TargetResource` 以确定配置是否处于正确的状态。
1. 该函数返回的布尔值确定来宾分配的 Azure 资源管理器状态是合规还是不合规。
1. 提供程序会运行 `Get-TargetResource` 以返回每项设置的当前状态。因此，会获得有关计算机为何不合规的详细信息，以及用于确认当前状态是否合规的详细信息。

### <a name="get-targetresource-requirements"></a>Get-TargetResource 要求

函数 `Get-TargetResource` 对 Guest Configuration 提出了特殊的要求，而 Windows Desired State Configuration 并不需要满足这些要求。

- 返回的哈希表必须包含名为 **Reasons** 的属性。
- Reasons 属性必须是数组。
- 该数组中的每个项应是包含名为 **Code** 和 **Phrase** 的键的哈希表。

当计算机不合规时，服务使用 Reasons 属性来标准化信息的呈现方式。 可将 Reasons 中的每个项视为资源不合规的一个“原因”。 该属性之所以是数组，是因为资源可能出于多种原因而不合规。

服务需要 **Code** 和 **Phrase** 属性。 创作自定义资源时，请将要作为资源不合规原因显示的文本（通常是 stdout）设置为 **Phrase** 的值。 Code 具有特定的格式要求，因此报告可以清楚地显示有关用于执行审核的资源的信息。  此解决方案使得 Guest Configuration 可扩展。 只要能够为 Phrase 属性返回字符串值形式的输出，就可以运行任何命令。 

- **Code**（字符串）：资源的名称，重复该名称，后接一个不包含空格的短名称（作为原因标识符）。 这三个值应以冒号分隔，且不包含空格。
  - 例如 `registry:registry:keynotpresent`
- **Phrase**（字符串）：用户可读的文本，用于解释设置不合规的原因。
  - 例如 `The registry key $key is not present on the machine.`

```powershell
$reasons = @()
$reasons += @{
  Code = 'Name:Name:ReasonIdentifer'
  Phrase = 'Explain why the setting is not compliant'
}
return @{
    reasons = $reasons
}
```

还必须将 Reasons 属性添加到嵌入类形式的资源的架构 MOF。

```mof
[ClassVersion("1.0.0.0")] 
class Reason
{
    [Read] String Phrase;
    [Read] String Code;
};

[ClassVersion("1.0.0.0"), FriendlyName("ResourceName")]
class ResourceName : OMI_BaseResource
{
    [Key, Description("Example description")] String Example;
    [Read, EmbeddedInstance("Reason")] String Reasons[];
};
```

### <a name="configuration-requirements"></a>配置要求

自定义配置的名称必须在每个位置保持一致。 内容包的 .zip 文件名称、MOF 文件中的配置名称，以及资源管理器模板中的来宾分配名称必须相同。

### <a name="scaffolding-a-guest-configuration-project"></a>搭建 Guest Configuration 项目

想要更快入门并参考示例代码的开发人员可以安装一个名为“Guest Configuration 项目”的社区项目。  该项目将安装 [Plaster](https://github.com/powershell/plaster) PowerShell 模块的模板。 此工具可用于搭建项目（包括工作配置和示例资源）以及一组 [Pester](https://github.com/pester/pester) 测试用于验证项目。 该模板还包含适用于 Visual Studio Code 的任务运行程序，可自动生成和验证 Guest Configuration 包。 有关详细信息，请参阅 GitHub 项目 [Guest Configuration 项目](https://github.com/microsoft/guestconfigurationproject)。

有关处理配置的一般详细信息，请参阅[编写、编译和应用配置](https://docs.microsoft.com/powershell/scripting/dsc/configurations/write-compile-apply-configuration)。

### <a name="expected-contents-of-a-guest-configuration-artifact"></a>Guest Configuration 项目的预期内容

Guest Configuration 使用已完成的包来创建 Azure Policy 定义。 该包中包括：

- 用作 MOF 的已编译 DSC 配置
- 模块文件夹
  - GuestConfiguration 模块
  - DscNativeResources 模块
  - (Windows) MOF 所需的 DSC 资源模块

PowerShell cmdlet 可帮助创建包。
不需要根级别文件夹或版本文件夹。
包格式必须是 .zip 文件。

### <a name="storing-guest-configuration-artifacts"></a>存储 Guest Configuration 项目

.zip 包必须存储在可由托管虚拟机访问的位置。
例如，存储在 GitHub 存储库、Azure 存储库或 Azure 存储中。 如果你不希望公开该包，可以在 URL 中包含 [SAS 令牌](../../../storage/common/storage-dotnet-shared-access-signature-part-1.md)。
还可以针对专用网络中的计算机实施[服务终结点](../../../storage/common/storage-network-security.md#grant-access-from-a-virtual-network)，不过，这种配置仅适用于访问包，而不适用于与服务之间的通信。

## <a name="step-by-step-creating-a-custom-guest-configuration-audit-policy-for-windows"></a>逐步创建适用于 Windows 的自定义 Guest Configuration 审核策略

创建 DSC 配置以审核设置。 以下 PowerShell 脚本示例创建名为 AuditBitLocker 的配置，导入 PsDscResources 资源模块，然后使用 `Service` 资源来审核正在运行的服务。   可以从 Windows 或 macOS 计算机执行配置脚本。

```powershell
# Define the DSC configuration and import GuestConfiguration
Configuration AuditBitLocker
{
    Import-DscResource -ModuleName 'PSDscResources'

    Node AuditBitlocker {
      Service 'Ensure BitLocker service is present and running'
      {
          Name = 'BDESVC'
          Ensure = 'Present'
          State = 'Running'
      }
    }
}

# Compile the configuration to create the MOF files
AuditBitLocker -out ./Config
```

从技术上讲，`Node AuditBitlocker` 命令不是必需的，但它会生成名为 `AuditBitlocker.mof` 的文件，而不是默认文件 `localhost.mof`。 使 .mof 文件名遵循配置可以在大规模操作时轻松组织许多文件。

编译 MOF 后，必须将支持文件打包在一起。 Guest Configuration 使用已完成的包来创建 Azure Policy 定义。

可以使用 `New-GuestConfigurationPackage` cmdlet 创建该包。 配置所需的模块必须在 `$Env:PSModulePath` 中提供。 创建 Windows 内容时 `New-GuestConfigurationPackage` cmdlet 的参数：

- **名称**：Guest Configuration 包名称。
- **配置**：编译的 DSC 配置文档的完整路径。
- **路径**：输出文件夹路径。 此参数是可选的。 如果未指定，将在当前目录中创建包。

运行以下命令，使用上一步骤中提供的配置创建一个包：

```azurepowershell-interactive
New-GuestConfigurationPackage `
  -Name 'AuditBitlocker' `
  -Configuration './Config/AuditBitlocker.mof'
```

创建配置包之后、将其发布到 Azure 之前，可以从工作站或 CI/CD 环境测试该包。 GuestConfiguration cmdlet `Test-GuestConfigurationPackage` 在开发环境中包含 Azure 计算机中所用的同一代理。 使用此解决方案可以在发布到计费的云环境之前，在本地执行集成测试。

由于该代理实际上评估的是本地环境，因此，在大多数情况下，需要在你计划审核的同一个 OS 平台上运行 Test- cmdlet。 该测试将仅使用内容包中包含的模块。

`Test-GuestConfigurationPackage` cmdlet 的参数：

- **名称**：Guest Configuration 策略名称。
- **参数**：以哈希表格式提供的策略参数。
- **路径**：Guest Configuration 包的完整路径。

运行以下命令来测试上一步骤创建的包：

```azurepowershell-interactive
Test-GuestConfigurationPackage `
  -Path ./AuditBitlocker.zip
```

该 cmdlet 还支持来自 PowerShell 管道的输入。 通过管道将 `New-GuestConfigurationPackage` cmdlet 的输出传送到 `Test-GuestConfigurationPackage` cmdlet。

```powershell
New-GuestConfigurationPackage -Name AuditBitlocker -Configuration ./Config/AuditBitlocker.mof | Test-GuestConfigurationPackage
```

下一步是将文件发布到 Blob 存储。 以下脚本包含一个可用于自动完成此任务的函数。 `publish` 函数中使用的命令需要 `Az.Storage` 模块。

```powershell
function publish {
    param(
    [Parameter(Mandatory=$true)]
    $resourceGroup,
    [Parameter(Mandatory=$true)]
    $storageAccountName,
    [Parameter(Mandatory=$true)]
    $storageContainerName,
    [Parameter(Mandatory=$true)]
    $filePath,
    [Parameter(Mandatory=$true)]
    $blobName
    )

    # Get Storage Context
    $Context = Get-AzStorageAccount -ResourceGroupName $resourceGroup `
        -Name $storageAccountName | `
        ForEach-Object { $_.Context }

    # Upload file
    $Blob = Set-AzStorageBlobContent -Context $Context `
        -Container $storageContainerName `
        -File $filePath `
        -Blob $blobName `
        -Force

    # Get url with SAS token
    $StartTime = (Get-Date)
    $ExpiryTime = $StartTime.AddYears('3')  # THREE YEAR EXPIRATION
    $SAS = New-AzStorageBlobSASToken -Context $Context `
        -Container $storageContainerName `
        -Blob $blobName `
        -StartTime $StartTime `
        -ExpiryTime $ExpiryTime `
        -Permission rl `
        -FullUri

    # Output
    return $SAS
}

# replace the $storageAccountName value below, it must be globally unique
$resourceGroup        = 'policyfiles'
$storageAccountName   = 'youraccountname'
$storageContainerName = 'artifacts'

$uri = publish `
  -resourceGroup $resourceGroup `
  -storageAccountName $storageAccountName `
  -storageContainerName $storageContainerName `
  -filePath ./AuditBitlocker.zip `
  -blobName 'AuditBitlocker'
```

创建并上传 Guest Configuration 自定义策略包后，创建 Guest Configuration 策略定义。 `New-GuestConfigurationPolicy` cmdlet 采用自定义策略包并创建策略定义。

`New-GuestConfigurationPolicy` cmdlet 的参数：

- **ContentUri**：Guest Configuration 内容包的公共 http(s) URI。
- **DisplayName**：策略显示名称。
- **说明**：策略说明。
- **参数**：以哈希表格式提供的策略参数。
- **版本**：策略版本。
- **路径**：要在其中创建策略定义的目标路径。
- **Platform**：Guest Configuration 策略和内容包的目标平台 (Windows/Linux)。

以下示例在自定义策略包的指定路径中创建策略定义：

```azurepowershell-interactive
New-GuestConfigurationPolicy `
    -ContentUri 'https://storageaccountname.blob.core.chinacloudapi.cn/packages/AuditBitLocker.zip?st=2019-07-01T00%3A00%3A00Z&se=2024-07-01T00%3A00%3A00Z&sp=rl&sv=2018-03-28&sr=b&sig=JdUf4nOCo8fvuflOoX%2FnGo4sXqVfP5BYXHzTl3%2BovJo%3D' `
    -DisplayName 'Audit BitLocker Service.' `
    -Description 'Audit if BitLocker is not enabled on Windows machine.' `
    -Path './policies' `
    -Platform 'Windows' `
    -Version 1.0.0 `
    -Verbose
```

`New-GuestConfigurationPolicy` 创建以下文件：

- **auditIfNotExists.json**
- **deployIfNotExists.json**
- **Initiative.json**

cmdlet 输出中会返回一个对象，其中包含策略文件的计划显示名称和路径。

最后，使用 `Publish-GuestConfigurationPolicy` cmdlet 发布策略定义。 该 cmdlet 仅包含指向 `New-GuestConfigurationPolicy` 所创建的 JSON 文件的位置的 Path 参数。 

若要运行 Publish 命令，需要拥有在 Azure 中创建策略的访问权限。 [Azure Policy 概述](../overview.md)页中阐述了具体的授权要求。 最佳内置角色是“资源策略参与者”。 

```azurepowershell-interactive
Publish-GuestConfigurationPolicy -Path '.\policyDefinitions'
```

`Publish-GuestConfigurationPolicy` cmdlet 接受源自 PowerShell 管道的路径。 此功能意味着，可以在一组管道命令中创建并发布策略文件。

```azurepowershell-interactive
New-GuestConfigurationPolicy `
 -ContentUri 'https://storageaccountname.blob.core.chinacloudapi.cn/packages/AuditBitLocker.zip?st=2019-07-01T00%3A00%3A00Z&se=2024-07-01T00%3A00%3A00Z&sp=rl&sv=2018-03-28&sr=b&sig=JdUf4nOCo8fvuflOoX%2FnGo4sXqVfP5BYXHzTl3%2BovJo%3D' `
  -DisplayName 'Audit BitLocker service.' `
  -Description 'Audit if the BitLocker service is not enabled on Windows machine.' `
  -Path './policies' `
 | Publish-GuestConfigurationPolicy
```

在 Azure 中创建策略后，最后一步是分配计划。 请参阅如何使用[门户](../assign-policy-portal.md)、[Azure CLI](../assign-policy-azurecli.md) 和 [Azure PowerShell](../assign-policy-powershell.md) 分配计划。

> [!IMPORTANT]
> **始终**必须使用结合了 _AuditIfNotExists_ 和 _DeployIfNotExists_ 策略的计划来分配 Guest Configuration 策略。 如果仅分配 _AuditIfNotExists_ 策略，则不会部署必备组件，并且该策略始终显示“0”个服务器合规。

分配包含 DeployIfNotExists 效应的策略定义需要额外的访问权限级别。  若要授予最低特权，可以创建一个用于扩展“资源策略参与者”的自定义角色定义。  以下示例创建名为“资源策略参与者 DINE”的、拥有“Microsoft.Authorization/roleAssignments/write”权限的角色。  

```azurepowershell-interactive
$subscriptionid = '00000000-0000-0000-0000-000000000000'
$role = Get-AzRoleDefinition "Resource Policy Contributor"
$role.Id = $null
$role.Name = "Resource Policy Contributor DINE"
$role.Description = "Can assign Policies that require remediation."
$role.Actions.Clear()
$role.Actions.Add("Microsoft.Authorization/roleAssignments/write")
$role.AssignableScopes.Clear()
$role.AssignableScopes.Add("/subscriptions/$subscriptionid")
New-AzRoleDefinition -Role $role
```

### <a name="using-parameters-in-custom-guest-configuration-policies"></a>使用自定义 Guest Configuration 策略中的参数

Guest Configuration 支持在运行时重写配置的属性。 此功能意味着，不一定要将包中 MOF 文件中的值视为静态值。 重写值是通过 Azure Policy 提供的，不会影响配置的创作或编译方式。

cmdlet `New-GuestConfigurationPolicy` 和 `Test-GuestConfigurationPolicyPackage` 包含名为 **Parameters** 的参数。 此参数采用一个包含有关每个参数的所有详细信息的哈希表定义，并创建每个文件的所需节用于定义 Azure Policy。

以下示例创建一个策略定义用于审核服务，其中，用户在分配策略时可从列表中进行选择。

```powershell
$PolicyParameterInfo = @(
    @{
        Name = 'ServiceName'                                            # Policy parameter name (mandatory)
        DisplayName = 'windows service name.'                           # Policy parameter display name (mandatory)
        Description = "Name of the windows service to be audited."      # Policy parameter description (optional)
        ResourceType = "Service"                                        # DSC configuration resource type (mandatory)
        ResourceId = 'windowsService'                                   # DSC configuration resource property name (mandatory)
        ResourcePropertyName = "Name"                                   # DSC configuration resource property name (mandatory)
        DefaultValue = 'winrm'                                          # Policy parameter default value (optional)
        AllowedValues = @('BDESVC','TermService','wuauserv','winrm')    # Policy parameter allowed values (optional)
    }
)

New-GuestConfigurationPolicy
    -ContentUri 'https://storageaccountname.blob.core.chinacloudapi.cn/packages/AuditBitLocker.zip?st=2019-07-01T00%3A00%3A00Z&se=2024-07-01T00%3A00%3A00Z&sp=rl&sv=2018-03-28&sr=b&sig=JdUf4nOCo8fvuflOoX%2FnGo4sXqVfP5BYXHzTl3%2BovJo%3D' `
    -DisplayName 'Audit Windows Service.' `
    -Description 'Audit if a Windows Service is not enabled on Windows machine.' `
    -Path '.\policyDefinitions' `
    -Parameters $PolicyParameterInfo `
    -Version 1.0.0
```

## <a name="policy-lifecycle"></a>策略生命周期

若要发布对策略的更新，需要注意两个字段。

- **版本**：运行 `New-GuestConfigurationPolicy` cmdlet 时，必须指定大于当前发布版本的版本号。 该属性更新 Guest Configuration 分配版本，使代理能够识别更新的包。
- **contentHash**：此属性由 `New-GuestConfigurationPolicy` cmdlet 自动更新。 它是 `New-GuestConfigurationPackage` 创建的包的哈希值。 对于发布的 `.zip` 文件，该属性必须正确。 如果仅更新了 contentUri 属性，扩展不会接受内容包。 

发布已更新的包的最简单方法是重复本文所述的过程，并提供更新的版本号。 该过程可保证正确更新所有属性。

## <a name="converting-windows-group-policy-content-to-azure-policy-guest-configuration"></a>将 Windows 组策略内容转换为 Azure Policy Guest Configuration

审核 Windows 计算机时，Guest Configuration 是 PowerShell Desired State Configuration 语法的实现。 DSC 社区已发布相应的工具用于将导出的组策略模板转换为 DSC 格式。 结合上述 Guest Configuration cmdlet 使用此工具，可以转换 Windows 组策略内容和包，并将其发布以供 Azure Policy 审核。 有关使用该工具的详细信息，请参阅文章[快速入门：将组策略转换为 DSC](https://docs.microsoft.com/powershell/scripting/dsc/quickstarts/gpo-quickstart)。
转换内容后，创建包并将其发布为 Azure Policy的步骤与处理任何 DSC 内容的相应步骤相同。

## <a name="optional-signing-guest-configuration-packages"></a>可选：为 Guest Configuration 包签名

Guest Configuration 自定义策略使用 SHA256 哈希来验证策略包是否未更改。
客户还可以选择性地使用证书来为该包签名，并强制 Guest Configuration 扩展仅允许已签名的内容。

若要实现此方案，需要完成两个步骤。 运行 cmdlet 以便为内容包签名，并将一个标记追加到要求为代码签名的计算机。

若要使用签名验证功能，请在发布该包之前，运行 `Protect-GuestConfigurationPackage` cmdlet 为其签名。 此 cmdlet 需要“代码签名”证书。

```powershell
$Cert = Get-ChildItem -Path cert:\LocalMachine\My | Where-Object {($_.Subject-eq "CN=mycert") }
Protect-GuestConfigurationPackage -Path .\package\AuditWindowsService\AuditWindowsService.zip -Certificate $Cert -Verbose
```

`Protect-GuestConfigurationPackage` cmdlet 的参数：

- **路径**：Guest Configuration 包的完整路径。
- **Certificate**：用来为包签名的代码签名证书。 仅当为适用于 Windows 的内容签名时才支持此参数。

GuestConfiguration 代理要求证书公钥在 Windows 计算机上的“受信任的根证书颁发机构”和 Linux 计算机上的 `/usr/local/share/ca-certificates/extra` 路径中存在。 要使节点能够验证已签名的内容，请在应用自定义策略之前在计算机上安装证书公钥。 可以使用 VM 中的任何技术或使用 Azure Policy 来完成此过程。 [此处提供](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vm-push-certificate-windows)了一个示例模板。
Key Vault 访问策略必须允许计算资源提供程序在部署期间访问证书。 有关详细步骤，请参阅[在 Azure 资源管理器中为虚拟机设置 Key Vault](../../../virtual-machines/windows/key-vault-setup.md#use-templates-to-set-up-key-vault)。

下面是从签名证书导出公钥并将其导入计算机的示例。

```powershell
$Cert = Get-ChildItem -Path cert:\LocalMachine\My | Where-Object {($_.Subject-eq "CN=mycert3") } | Select-Object -First 1
$Cert | Export-Certificate -FilePath "$env:temp\DscPublicKey.cer" -Force
```

发布内容后，将名为 `GuestConfigPolicyCertificateValidation`、值为 `enabled` 的标记追加到需要代码签名的所有虚拟机。 请参阅[标记示例](../samples/built-in-policies.md#tags)，了解如何使用 Azure Policy 大规模传递标记。 追加此标记后，使用 `New-GuestConfigurationPolicy` cmdlet 生成的策略定义可通过 Guest Configuration 扩展来满足要求。

## <a name="troubleshooting-guest-configuration-policy-assignments-preview"></a>排查 Guest Configuration 策略分配问题（预览版）

我们已提供一个预览版工具用于帮助排查 Azure Policy Guest Configuration 分配问题。 该工具目前为预览版，已发布到 PowerShell 库，其名称为 [Guest Configuration 故障排除工具](https://www.powershellgallery.com/packages/GuestConfigurationTroubleshooter/)。

有关此工具中的 cmdlet 的详细信息，请在 PowerShell 中使用 Get-Help 命令显示内置的指导。 在该工具的频繁更新过程中，此命令是获取最新信息的最佳方式。

## <a name="next-steps"></a>后续步骤

- 了解如何使用 [Guest Configuration](../concepts/guest-configuration.md) 审核 VM。
- 了解如何[以编程方式创建策略](programmatically-create.md)。
- 了解如何[获取合规性数据](get-compliance-data.md)。