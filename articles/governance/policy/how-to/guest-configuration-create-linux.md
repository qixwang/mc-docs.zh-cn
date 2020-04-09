---
title: 如何创建适用于 Linux 的 Guest Configuration 策略
description: 了解如何创建适用于 Linux 的 Azure Policy Guest Configuration 策略。
origin.date: 03/20/2020
ms.date: 03/30/2020
ms.author: v-tawe
ms.topic: how-to
ms.openlocfilehash: 4fa99b126214671831654213afcf442bc99e382b
ms.sourcegitcommit: 260800ede66f48c886d1426a0fac18b4d402b4f2
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/02/2020
ms.locfileid: "80586827"
---
# <a name="how-to-create-guest-configuration-policies-for-linux"></a>如何创建适用于 Linux 的 Guest Configuration 策略

在创建自定义策略之前，请阅读 [Azure Policy Guest Configuration](../concepts/guest-configuration.md) 中的概述信息。
 
若要了解如何创建适用于 Windows 的 Guest Configuration 策略，请参阅[如何创建适用于 Windows 的 Guest Configuration 策略](./guest-configuration-create.md)页

审核 Linux 时，Guest Configuration 将使用 [Chef InSpec](https://www.inspec.io/)。 InSpec 配置文件定义计算机应该所处的状态。 如果配置评估失败，则会触发策略效应 auditIfNotExists，并将计算机视为不合规。  

[Azure Policy Guest Configuration](../concepts/guest-configuration.md) 只可用于审核计算机内部的设置。 目前尚未提供修正计算机内部设置的功能。

使用以下操作创建自己的配置用于验证 Azure 或非 Azure 计算机的状态。

> [!IMPORTANT]
> 使用 Guest Configuration 的自定义策略是一项预览版功能。

## <a name="install-the-powershell-module"></a>安装 PowerShell 模块

在 PowerShell 中使用 Guest Configuration 模块创建 Guest Configuration 项目、自动测试项目、创建策略定义和发布策略的过程完全可自动化。 该模块可以安装在运行 Windows、macOS 或 Linux 并装有 PowerShell 6.2 或更高版本的计算机本地，或者与 [Azure PowerShell Core Docker 映像](https://hub.docker.com/r/azuresdk/azure-powershell-core)一起安装。

> [!NOTE]
> Linux 不支持配置编译。

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

   ```azurepowershell
   # Install the Guest Configuration DSC resource module from PowerShell Gallery
   Install-Module -Name GuestConfiguration
   ```

1. 验证是否已导入该模块：

   ```azurepowershell
   # Get a list of commands for the imported GuestConfiguration module
   Get-Command -Module 'GuestConfiguration'
   ```

## <a name="guest-configuration-artifacts-and-policy-for-linux"></a>适用于 Linux 的 Guest Configuration 项目和策略

即使是在 Linux 环境中，Guest Configuration 也会使用 Desired State Configuration 作为语言抽象。 该实现基于本机代码 (C++)，因此无需加载 PowerShell。 但是，它确实需要一个用于描述有关环境的详细信息的配置 MOF。 DSC 充当 InSpec 的包装器，用于标准化 InSpec 的执行方式、参数的提供方式，以及向服务返回输出的方式。 只需对 DSC 稍有了解即可处理自定义的 InSpec 内容。

#### <a name="configuration-requirements"></a>配置要求

自定义配置的名称必须在每个位置保持一致。 内容包的 .zip 文件名称、MOF 文件中的配置名称，以及资源管理器模板中的来宾分配名称必须相同。

### <a name="custom-guest-configuration-configuration-on-linux"></a>Linux 上的自定义 Guest Configuration 配置

Linux 上的 Guest Configuration 使用 `ChefInSpecResource` 资源为引擎提供 [InSpec 配置文件](https://www.inspec.io/docs/reference/profiles/)的名称。 只有 **Name** 是必需的资源属性。 根据下面的详述创建 YaML 文件和 Ruby 脚本文件。

首先创建 InSpec 使用的 YaML 文件。 该文件提供有关环境的基本信息。 下面提供了一个示例：

```YaML
name: linux-path
title: Linux path
maintainer: Test
summary: Test profile
license: MIT
version: 1.0.0
supports:
    - os-family: unix
```

将此文件保存到项目目录中名为 `linux-path` 的文件夹内。

接下来，使用 InSpec 语言抽象创建 Ruby 文件用于审核计算机。

```Ruby
describe file('/tmp') do
    it { should exist }
end
```

将此文件保存到 `linux-path` 目录中名为 `controls` 的新文件夹内。

最后，创建配置，导入 GuestConfiguration 资源模块，并使用 `ChefInSpecResource` 资源设置 InSpec 配置文件的名称。 

```powershell
# Define the configuration and import GuestConfiguration
Configuration AuditFilePathExists
{
    Import-DscResource -ModuleName 'GuestConfiguration'

    Node AuditFilePathExists
    {
        ChefInSpecResource 'Audit Linux path exists'
        {
            Name = 'linux-path'
        }
    }
}

# Compile the configuration to create the MOF files
AuditFilePathExists -out ./Config
```

从技术上讲，`Node AuditFilePathExists` 命令不是必需的，但它会生成名为 `AuditFilePathExists.mof` 的文件，而不是默认文件 `localhost.mof`。 使 .mof 文件名遵循配置可以在大规模操作时轻松组织许多文件。

现在，应已获得如下所示的项目结构：

```file
/ AuditFilePathExists
    / Config
        AuditFilePathExists.mof
    / linux-path
        linux-path.yml
        / controls
            linux-path.rb 
```

支持文件必须打包在一起。 Guest Configuration 使用已完成的包来创建 Azure Policy 定义。

可以使用 `New-GuestConfigurationPackage` cmdlet 创建该包。 创建 Linux 内容时 `New-GuestConfigurationPackage` cmdlet 的参数：

- **名称**：Guest Configuration 包名称。
- **配置**：编译的配置文档的完整路径。
- **路径**：输出文件夹路径。 此参数是可选的。 如果未指定，将在当前目录中创建包。
- **ChefProfilePath**：InSpec 配置文件的完整路径。 仅当创建用于审核 Linux 的内容时才支持此参数。

运行以下命令，使用上一步骤中提供的配置创建一个包：

```azurepowershell
New-GuestConfigurationPackage `
  -Name 'AuditFilePathExists' `
  -Configuration './Config/AuditFilePathExists.mof'
  -ChefProfilePath './'
```

创建配置包之后、将其发布到 Azure 之前，可以从工作站或 CI/CD 环境测试该包。 GuestConfiguration cmdlet `Test-GuestConfigurationPackage` 在开发环境中包含 Azure 计算机中所用的同一代理。 使用此解决方案可以在发布到计费的云环境之前，在本地执行集成测试。

由于该代理实际上评估的是本地环境，因此，在大多数情况下，需要在你计划审核的同一个 OS 平台上运行 Test- cmdlet。

`Test-GuestConfigurationPackage` cmdlet 的参数：

- **名称**：Guest Configuration 策略名称。
- **参数**：以哈希表格式提供的策略参数。
- **路径**：Guest Configuration 包的完整路径。

运行以下命令来测试上一步骤创建的包：

```azurepowershell
Test-GuestConfigurationPackage `
  -Path ./AuditFilePathExists.zip
```

该 cmdlet 还支持来自 PowerShell 管道的输入。 通过管道将 `New-GuestConfigurationPackage` cmdlet 的输出传送到 `Test-GuestConfigurationPackage` cmdlet。

```azurepowershell
New-GuestConfigurationPackage -Name AuditFilePathExists -Configuration ./Config/AuditFilePathExists.mof -ChefProfilePath './' | Test-GuestConfigurationPackage
```

下一步是将文件发布到 Blob 存储。 以下脚本包含一个可用于自动完成此任务的函数。 `publish` 函数中使用的命令需要 `Az.Storage` 模块。

```azurepowershell
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
  -filePath ./AuditFilePathExists.zip `
  -blobName 'AuditFilePathExists'
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

```azurepowershell
New-GuestConfigurationPolicy `
    -ContentUri 'https://storageaccountname.blob.core.chinacloudapi.cn/packages/AuditFilePathExists.zip?st=2019-07-01T00%3A00%3A00Z&se=2024-07-01T00%3A00%3A00Z&sp=rl&sv=2018-03-28&sr=b&sig=JdUf4nOCo8fvuflOoX%2FnGo4sXqVfP5BYXHzTl3%2BovJo%3D' `
    -DisplayName 'Audit Linux file path.' `
    -Description 'Audit that a file path exists on a Linux machine.' `
    -Path './policies' `
    -Platform 'Linux' `
    -Version 1.0.0 `
    -Verbose
```

`New-GuestConfigurationPolicy` 创建以下文件：

- **auditIfNotExists.json**
- **deployIfNotExists.json**
- **Initiative.json**

cmdlet 输出中会返回一个对象，其中包含策略文件的计划显示名称和路径。

最后，使用 `Publish-GuestConfigurationPolicy` cmdlet 发布策略定义。
该 cmdlet 仅包含指向 `New-GuestConfigurationPolicy` 所创建的 JSON 文件的位置的 Path 参数。 

若要运行 Publish 命令，需要拥有在 Azure 中创建策略的访问权限。 [Azure Policy 概述](../overview.md)页中阐述了具体的授权要求。 最佳内置角色是“资源策略参与者”。 

```azurepowershell
Publish-GuestConfigurationPolicy `
  -Path '.\policyDefinitions'
```

 `Publish-GuestConfigurationPolicy` cmdlet 接受源自 PowerShell 管道的路径。 此功能意味着，可以在一组管道命令中创建并发布策略文件。

 ```azurepowershell
 New-GuestConfigurationPolicy `
  -ContentUri 'https://storageaccountname.blob.core.chinacloudapi.cn/packages/AuditFilePathExists.zip?st=2019-07-01T00%3A00%3A00Z&se=2024-07-01T00%3A00%3A00Z&sp=rl&sv=2018-03-28&sr=b&sig=JdUf4nOCo8fvuflOoX%2FnGo4sXqVfP5BYXHzTl3%2BovJo%3D' `
  -DisplayName 'Audit Linux file path.' `
  -Description 'Audit that a file path exists on a Linux machine.' `
  -Path './policies' `
 | Publish-GuestConfigurationPolicy
 ```

在 Azure 中创建策略后，最后一步是分配计划。 请参阅如何使用[门户](../assign-policy-portal.md)、[Azure CLI](../assign-policy-azurecli.md) 和 [Azure PowerShell](../assign-policy-powershell.md) 分配计划。

> [!IMPORTANT]
> **始终**必须使用结合了 _AuditIfNotExists_ 和 _DeployIfNotExists_ 策略的计划来分配 Guest Configuration 策略。 如果仅分配 _AuditIfNotExists_ 策略，则不会部署必备组件，并且该策略始终显示“0”个服务器合规。

分配包含 DeployIfNotExists 效应的策略定义需要额外的访问权限级别。  若要授予最低特权，可以创建一个用于扩展“资源策略参与者”的自定义角色定义。  以下示例创建名为“资源策略参与者 DINE”的、拥有“Microsoft.Authorization/roleAssignments/write”权限的角色。  

```azurepowershell
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

使用 InSpec 时，参数通常会在运行时作为输入进行处理，或者使用特性作为代码进行处理。 Guest Configuration 将此过程模糊化，因此可在分配策略时提供输入。 系统会自动在计算机中创建一个特性文件。 你不需要在项目中创建并添加文件。 将参数添加到 Linux 审核项目需要执行两个步骤。

在计算机上脚本要审核的 Ruby 文件中定义输入。 下面提供了一个示例。

```Ruby
attr_path = attribute('path', description: 'The file path to validate.')

describe file(attr_path) do
    it { should exist }
end
```

cmdlet `New-GuestConfigurationPolicy` 和 `Test-GuestConfigurationPolicyPackage` 包含名为 **Parameters** 的参数。 此参数采用哈希表（其中包含有关每个参数的所有详细信息），并自动创建全部所需的文件节来创建每个 Azure Policy 定义。

以下示例创建一个用于审核文件路径的策略定义，其中，用户将在分配策略时提供路径。

```azurepowershell
$PolicyParameterInfo = @(
    @{
        Name = 'FilePath'                             # Policy parameter name (mandatory)
        DisplayName = 'File path.'                    # Policy parameter display name (mandatory)
        Description = "File path to be audited."      # Policy parameter description (optional)
        ResourceType = "ChefInSpecResource"           # Configuration resource type (mandatory)
        ResourceId = 'Audit Linux path exists'        # Configuration resource property name (mandatory)
        ResourcePropertyName = "AttributesYmlContent" # Configuration resource property name (mandatory)
        DefaultValue = '/tmp'                         # Policy parameter default value (optional)
    }
)

# The hashtable also supports a property named 'AllowedValues' with an array of strings to limit input to a list

New-GuestConfigurationPolicy
    -ContentUri 'https://storageaccountname.blob.core.chinacloudapi.cn/packages/AuditFilePathExists.zip?st=2019-07-01T00%3A00%3A00Z&se=2024-07-01T00%3A00%3A00Z&sp=rl&sv=2018-03-28&sr=b&sig=JdUf4nOCo8fvuflOoX%2FnGo4sXqVfP5BYXHzTl3%2BovJo%3D' `
    -DisplayName 'Audit Linux file path.' `
    -Description 'Audit that a file path exists on a Linux machine.' `
    -Path './policies' `
    -Parameters $PolicyParameterInfo `
    -Version 1.0.0
```

对于 Linux 策略，请在配置中包含属性 **AttributesYmlContent** 并根据需要覆盖值。 来宾配置代理会自动创建 InSpec 用来存储属性的 YAML 文件。 请参阅以下示例。

```powershell
Configuration AuditFilePathExists
{
    Import-DscResource -ModuleName 'GuestConfiguration'

    Node AuditFilePathExists
    {
        ChefInSpecResource 'Audit Linux path exists'
        {
            Name = 'linux-path'
            AttributesYmlContent = "path: /tmp"
        }
    }
}
```

## <a name="policy-lifecycle"></a>策略生命周期

若要发布对策略定义的更新，需要注意两个字段。

- **版本**：运行 `New-GuestConfigurationPolicy` cmdlet 时，必须指定大于当前发布版本的版本号。 该属性更新 Guest Configuration 分配版本，使代理能够识别更新的包。
- **contentHash**：此属性由 `New-GuestConfigurationPolicy` cmdlet 自动更新。 它是 `New-GuestConfigurationPackage` 创建的包的哈希值。 对于发布的 `.zip` 文件，该属性必须正确。 如果仅更新了 contentUri 属性，扩展不会接受内容包。 

发布已更新的包的最简单方法是重复本文所述的过程，并提供更新的版本号。 该过程可保证正确更新所有属性。

## <a name="optional-signing-guest-configuration-packages"></a>可选：为 Guest Configuration 包签名

Guest Configuration 自定义策略使用 SHA256 哈希来验证策略包是否未更改。
客户还可以选择性地使用证书来为该包签名，并强制 Guest Configuration 扩展仅允许已签名的内容。

若要实现此方案，需要完成两个步骤。 运行 cmdlet 以便为内容包签名，并将一个标记追加到要求为代码签名的计算机。

若要使用签名验证功能，请在发布该包之前，运行 `Protect-GuestConfigurationPackage` cmdlet 为其签名。 此 cmdlet 需要“代码签名”证书。

`Protect-GuestConfigurationPackage` cmdlet 的参数：

- **路径**：Guest Configuration 包的完整路径。
- **PublicGpgKeyPath**：GPG 公钥路径。 仅当为适用于 Linux 的内容签名时才支持此参数。

GitHub 上的[生成新的 GPG 密钥](https://help.github.com/en/articles/generating-a-new-gpg-key)一文中全面介绍了如何创建可在 Linux 计算机上使用的 GPG 密钥。

GuestConfiguration 代理预期证书公钥在 Linux 计算机上的路径 `/usr/local/share/ca-certificates/extra` 中存在。 要使节点能够验证已签名的内容，请在应用自定义策略之前在计算机上安装证书公钥。 可以使用 VM 中的任何技术或使用 Azure Policy 来完成此过程。 [此处提供](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vm-push-certificate-windows)了一个示例模板。
Key Vault 访问策略必须允许计算资源提供程序在部署期间访问证书。 有关详细步骤，请参阅[在 Azure 资源管理器中为虚拟机设置 Key Vault](../../../virtual-machines/windows/key-vault-setup.md#use-templates-to-set-up-key-vault)。

发布内容后，将名为 `GuestConfigPolicyCertificateValidation`、值为 `enabled` 的标记追加到需要代码签名的所有虚拟机。 请参阅[标记示例](../samples/built-in-policies.md#tags)，了解如何使用 Azure Policy 大规模传递标记。 追加此标记后，使用 `New-GuestConfigurationPolicy` cmdlet 生成的策略定义可通过 Guest Configuration 扩展来满足要求。

## <a name="troubleshooting-guest-configuration-policy-assignments-preview"></a>排查 Guest Configuration 策略分配问题（预览版）

我们已提供一个预览版工具用于帮助排查 Azure Policy Guest Configuration 分配问题。 该工具目前为预览版，已发布到 PowerShell 库，其名称为 [Guest Configuration 故障排除工具](https://www.powershellgallery.com/packages/GuestConfigurationTroubleshooter/)。

有关此工具中的 cmdlet 的详细信息，请在 PowerShell 中使用 Get-Help 命令显示内置的指导。 在该工具的频繁更新过程中，此命令是获取最新信息的最佳方式。

## <a name="next-steps"></a>后续步骤

- 了解如何使用 [Guest Configuration](../concepts/guest-configuration.md) 审核 VM。
- 了解如何[以编程方式创建策略](programmatically-create.md)。
- 了解如何[获取合规性数据](get-compliance-data.md)。
