---
title: 在 Azure 自动化混合 Runbook 辅助角色上运行 runbook
description: 本文介绍如何使用混合 Runbook 辅助角色在本地数据中心或云提供商的计算机上运行 runbook。
services: automation
ms.subservice: process-automation
origin.date: 01/29/2019
ms.date: 03/30/2020
ms.topic: conceptual
ms.openlocfilehash: 3a4b7a5fea43d4ca31cd1ebcaeb84d554b34a7c2
ms.sourcegitcommit: 90d01d08faf8adb20083363a8e4e5aab139cd9b2
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/26/2020
ms.locfileid: "80290352"
---
# <a name="running-runbooks-on-a-hybrid-runbook-worker"></a>在混合 Runbook 辅助角色上运行 runbook

面向混合 Runbook 辅助角色的 Runbook 通常管理本地计算机上的资源，或者管理该辅助角色所在的本地环境中的资源。 Azure 自动化中的 runbook 通常管理 Azure 云中的资源。 尽管在 Azure 自动化中运行的 Runbook 与在混合 Runbook 辅助角色上运行的 Runbook 的使用方式不同，但它们的结构是相同的。

创建要在混合 Runbook 辅助角色上运行的 Runbook 时，应在托管该辅助角色的计算机上编辑并测试该 Runbook。 主机具有管理和访问本地资源所需的所有 PowerShell 模块与网络访问权限。 在混合 Runbook 辅助角色计算机上测试 Runbook 后，可将其上传到 Azure 自动化环境，然后，该 Runbook 便可以在辅助角色上运行。 

>[!NOTE]
>本文进行了更新，以便使用新的 Azure PowerShell Az 模块。 你仍然可以使用 AzureRM 模块，至少在 2020 年 12 月之前，它将继续接收 bug 修补程序。 若要详细了解新的 Az 模块和 AzureRM 兼容性，请参阅[新 Azure Powershell Az 模块简介](https://docs.microsoft.com/powershell/azure/new-azureps-module-az?view=azps-3.5.0)。 有关适用于混合 Runbook 辅助角色的 Az 模块安装说明，请参阅安装 [Azure PowerShell 模块](https://docs.microsoft.com/powershell/azure/install-az-ps?view=azps-3.5.0)。

## <a name="runbook-permissions-for-a-hybrid-runbook-worker"></a>混合 Runbook 辅助角色的 Runbook 权限

混合 Runbook 辅助角色上运行的 Runbook 在访问非 Azure 资源时，无法使用通常由 Runbook 对 Azure 资源进行身份验证时所用的身份验证机制。 Runbook 可以针对本地资源提供其自身的身份验证，或者配置使用 [Azure 资源托管标识](../active-directory/managed-identities-azure-resources/tutorial-windows-vm-access-arm.md#grant-your-vm-access-to-a-resource-group-in-resource-manager)的身份验证。 你还可以指定运行方式帐户，为所有 Runbook 提供用户上下文。

### <a name="runbook-authentication"></a>Runbook 身份验证

Runbook 默认在本地计算机上运行。 对于 Windows，它们在本地系统帐户的上下文中运行。 对于 Linux，它们在特殊用户帐户 **nxautomation** 的上下文中运行。 对于任一情况，Runbook 都必须对它们要访问的资源提供自身的身份验证。

可以通过 cmdlet 在 Runbook 中使用[凭据](automation-credentials.md)和[证书](automation-certificates.md)资产，这些 cmdlet 允许指定凭据，使 Runbook 可对不同的资源进行身份验证。 下面的示例显示了用于重新启动计算机的 Runbook 的一部分。 它从凭据资产检索凭据，从变量资产检索计算机的名称，并将这些值用于 `Restart-Computer` cmdlet。

```powershell
$Cred = Get-AutomationPSCredential -Name "MyCredential"
$Computer = Get-AutomationVariable -Name "ComputerName"

Restart-Computer -ComputerName $Computer -Credential $Cred
```

还可以使用 [InlineScript](automation-powershell-workflow.md#inlinescript) 活动。 `InlineScript` 允许使用 [PSCredential 通用参数](https://docs.microsoft.com/powershell/module/psworkflow/about/about_workflowcommonparameters)指定的凭据在另一台计算机上运行代码块。

### <a name="run-as-account"></a>运行方式帐户

无需让 Runbook 将自身的身份验证提供给本地资源，可以针对混合 Runbook 辅助角色组指定一个运行方式帐户。 为此，必须定义有权访问本地资源的[凭据资产](automation-credentials.md)。 这些资源包括证书存储，以及组中混合 Runbook 辅助角色上的这些凭据下运行的所有 Runbook。

凭据的用户名必须采用以下格式之一：

* 域\用户名
* username@domain
* 用户名（适用于本地计算机的本地帐户）

使用以下过程为混合 Runbook 辅助角色组指定运行方式帐户。

1. 创建具有本地资源访问权限的 [凭据资产](automation-credentials.md) 。
2. 在 Azure 门户中打开自动化帐户。
3. 选择“混合辅助角色组”  磁贴，并选择组。
4. 依次选择“所有设置”、“混合辅助角色组设置”。  
5. 将“运行方式”的值从“默认”更改为“自定义”。   
6. 选择凭据，并单击“保存”  。

### <a name="managed-identities-for-azure-resources"></a><a name="managed-identities-for-azure-resources"></a>Azure 资源的托管标识

Azure 虚拟机上的混合 Runbook 辅助角色可以使用 Azure 资源的托管标识对 Azure 资源进行身份验证。 使用 Azure 资源的托管标识而不使用运行方式帐户可以带来一些优势，因为无需执行以下操作：

* 导出运行方式证书，然后将其导入到混合 Runbook 辅助角色。
* 续订运行方式帐户使用的证书。
* 在 Runbook 代码中处理运行方式连接对象。

遵循后续步骤为混合 Runbook 辅助角色上的 Azure 资源使用托管标识。

1. 创建一个 Azure VM。
2. 为该 VM 上的 Azure 资源配置托管标识。 请参阅[使用 Azure 门户为 VM 上的 Azure 资源配置托管标识](../active-directory/managed-identities-azure-resources/qs-configure-portal-windows-vm.md#enable-system-assigned-managed-identity-on-an-existing-vm)。
3. 授予该 VM 对资源管理器中资源组的访问权限。 请参阅[使用 Windows VM 系统分配的托管标识访问资源管理器](../active-directory/managed-identities-azure-resources/tutorial-windows-vm-access-arm.md#grant-your-vm-access-to-a-resource-group-in-resource-manager)。
4. 在该 VM 上安装混合 Runbook 辅助角色。 请参阅[部署 Windows 混合 Runbook 辅助角色](automation-windows-hrw-install.md)。
5. 更新 runbook，将 [Add-azurermaccount](https://docs.microsoft.com/powershell/module/azurerm.profile/connect-azurermaccount?view=azurermps-6.13.0) cmdlet 与 `Identity` 参数一起使用来对 Azure 资源进行身份验证。 此配置减少了使用运行方式帐户以及执行相关帐户管理的需要。

```powershell
    # Connect to Azure using the managed identities for Azure resources identity configured on the Azure VM that is hosting the hybrid runbook worker
    Connect-AzureRmAccount -EnvironmentName AzureChinaCloud -Identity

    # Get all VM names from the subscription
    Get-AzureRmVm | Select Name
```

> [!NOTE]
> `Connect-AzureRmAccount -Identity` 适用于使用系统分配标识和单个用户分配标识的混合 Runbook 辅助角色。 如果在混合 Runbook 辅助角色上使用多个用户分配标识，则 Runbook 必须为 `Connect-AzureRmAccount` 指定 *AccountId* 参数，以选择特定的用户分配标识。

### <a name="automation-run-as-account"></a><a name="runas-script"></a>自动化运行方式帐户

在 Azure 中部署资源时，可能需要在自动生成过程中访问本地系统，以支持部署过程中的某个任务或某组步骤。 若要使用运行方式帐户针对 Azure 提供身份验证，必须安装运行方式帐户证书。

以下名为 **Export-RunAsCertificateToHybridWorker** 的 PowerShell Runbook 从你的 Azure 自动化帐户导出运行方式证书。 该 Runbook 将下载该证书，并将其导入到已连接同一帐户的混合 Runbook 辅助角色上的本地计算机证书存储中。 完成该步骤后，该 Runbook 将验证该辅助角色是否能够成功地使用运行方式帐户对 Azure 进行身份验证。

```azurepowershell
<#PSScriptInfo
.VERSION 1.0
.GUID 3a796b9a-623d-499d-86c8-c249f10a6986
.AUTHOR Azure Automation Team
.COMPANYNAME Microsoft
.COPYRIGHT
.TAGS Azure Automation
.LICENSEURI
.PROJECTURI
.ICONURI
.EXTERNALMODULEDEPENDENCIES
.REQUIREDSCRIPTS
.EXTERNALSCRIPTDEPENDENCIES
.RELEASENOTES
#>

<#
.SYNOPSIS
Exports the Run As certificate from an Azure Automation account to a hybrid worker in that account.

.DESCRIPTION
This runbook exports the Run As certificate from an Azure Automation account to a hybrid worker in that account. Run this runbook on the hybrid worker where you want the certificate installed. This allows the use of the AzureRunAsConnection to authenticate to Azure and manage Azure resources from runbooks running on the hybrid worker.

.EXAMPLE
.\Export-RunAsCertificateToHybridWorker

.NOTES
LASTEDIT: 2016.10.13
#>

# Generate the password used for this certificate
Add-Type -AssemblyName System.Web -ErrorAction SilentlyContinue | Out-Null
$Password = [System.Web.Security.Membership]::GeneratePassword(25, 10)

# Stop on errors
$ErrorActionPreference = 'stop'

# Get the management certificate that will be used to make calls into Azure Service Management resources
$RunAsCert = Get-AutomationCertificate -Name "AzureRunAsCertificate"

# location to store temporary certificate in the Automation service host
$CertPath = Join-Path $env:temp  "AzureRunAsCertificate.pfx"

# Save the certificate
$Cert = $RunAsCert.Export("pfx",$Password)
Set-Content -Value $Cert -Path $CertPath -Force -Encoding Byte | Write-Verbose

Write-Output ("Importing certificate into $env:computername local machine root store from " + $CertPath)
$SecurePassword = ConvertTo-SecureString $Password -AsPlainText -Force
Import-PfxCertificate -FilePath $CertPath -CertStoreLocation Cert:\LocalMachine\My -Password $SecurePassword -Exportable | Write-Verbose

# Test to see if authentication to Azure Resource Manager is working
$RunAsConnection = Get-AutomationConnection -Name "AzureRunAsConnection"

Connect-AzureRmAccount `
    -EnvironmentName AzureChinaCloud `
    -ServicePrincipal `
    -TenantId $RunAsConnection.TenantId `
    -ApplicationId $RunAsConnection.ApplicationId `
    -CertificateThumbprint $RunAsConnection.CertificateThumbprint | Write-Verbose

Set-AzureRmContext -SubscriptionId $RunAsConnection.SubscriptionID | Write-Verbose

# List automation accounts to confirm that Azure Resource Manager calls are working
Get-AzureRmAutomationAccount | Select-Object AutomationAccountName
```

>[!NOTE]
>**Add-AzureRmAccount** 现在是 **Connect-AzureRmAccount** 的别名。 搜索库项时，如果未看到 **Connect-AzureRmAccount**，则可以使用 **Add-AzureRmAccount**。

若要完成运行方式帐户的准备工作：

1. 将 **Export-RunAsCertificateToHybridWorker** Runbook 保存到扩展名为 **.ps1** 的计算机。
2. 将其导入到自动化帐户。
3. 编辑该 Runbook，将变量 `Password` 的值更改为你自己的密码。 
4. 发布 Runbook。
5. 针对运行 Runbook 并使用运行方式帐户对其进行身份验证的混合 Runbook 辅助角色组运行该 Runbook。 
6. 检查作业流，可以看到，其中报告了某个对象尝试将证书导入本地计算机存储，其后是多个行。 此行为取决于在订阅中定义的自动化帐户数目，以及身份验证的成功度。

## <a name="job-behavior-on-hybrid-runbook-workers"></a>混合 Runbook 辅助角色上的作业行为

Azure 自动化处理混合 Runbook 辅助角色上的作业的方式，与处理 Azure 沙盒中运行的作业的方式稍有不同。 一个主要区别在于，Runbook 辅助角色不限制作业持续时间。 由于[公平份额](automation-runbook-execution.md#fair-share)的原则，Azure 沙盒中运行的 Runbook 限制为 3 小时。

对于长时间运行的 Runbook，最好是确保它在可能发生的重启（例如，托管辅助角色的计算机重启）后能够复原。 如果混合 Runbook 辅助角色主机重新启动，则任何正在运行的 Runbook 作业将从头重启，或者从 PowerShell 工作流 Runbook 的最后一个检查点重启。 Runbook 作业重启 3 次以上后将会暂停。

请记住，混合 Runbook 辅助角色的作业在 Windows 上的本地系统帐户下运行，或者在 Linux 上的 **nxautomation** 帐户下运行。 对于 Linux，必须确保 **nxautomation** 帐户有权访问 Runbook 模块存储到的位置。 使用 [Install-Module](https://docs.microsoft.com/powershell/module/powershellget/install-module) cmdlet 时，请确保为 `Scope` 参数指定 AllUsers，以确保 **nxautomation** 帐户拥有访问权限。 有关 Linux 上 PowerShell 的详细信息，请参阅[非 Windows 平台上 PowerShell 的已知问题](https://docs.microsoft.com/powershell/scripting/whats-new/known-issues-ps6?view=powershell-6#known-issues-for-powershell-on-non-windows-platforms)。

## <a name="starting-a-runbook-on-a-hybrid-runbook-worker"></a>启动混合 Runbook 辅助角色上的 Runbook

[在 Azure 自动化中启动 Runbook](automation-starting-a-runbook.md) 介绍了启动 Runbook 的不同方法。 启动混合 Runbook 辅助角色上的 Runbook 需使用“运行位置”选项，以便能够指定混合 Runbook 辅助角色组的名称。  指定组后，该组中的某个辅助角色将检索并运行该 Runbook。 如果 Runbook 未指定此选项，Azure 自动化将像往常一样运行 Runbook。

在 Azure 门户中启动 Runbook 时，会看到一个“运行位置”选项，可为该选项选择“Azure”或“混合辅助角色”。    如果选择“混合辅助角色”，可以从下拉列表中选择混合 Runbook 辅助角色组。 

将 `RunOn` 参数与 `Start-AzureAutomationRunbook` cmdlet 一起使用。 以下示例使用 Windows PowerShell 在名为 MyHybridGroup 的混合 Runbook 辅助角色组中启动名为 **Test-Runbook** 的 Runbook。

```azurepowershell
Start-AzureAutomationRunbook –AutomationAccountName "MyAutomationAccount" –Name "Test-Runbook" -RunOn "MyHybridGroup"
```

> [!NOTE]
> 在 Azure PowerShell 版本 0.9.1 中，`Start-AzureAutomationRunbook` 中已添加了 `RunOn` 参数。 如果安装的是旧版，则应 [下载最新版本](https://azure.microsoft.com/downloads/)。 只需在要在其中通过 PowerShell 启动 Runbook 的工作站上安装此版本。 不需要在混合 Runbook 辅助角色计算机上安装它，除非要从该计算机启动 Runbook。

## <a name="working-with-signed-runbooks-on-a-windows-hybrid-runbook-worker"></a>使用 Windows 混合 Runbook 辅助角色上已签名的 Runbook

可将 Windows 混合 Runbook 辅助角色配置为仅运行已签名的 Runbook。

> [!IMPORTANT]
> 将混合 Runbook 辅助角色配置为仅运行已签名的 Runbook 后，未签名的 Runbook 无法在该辅助角色上执行。

### <a name="create-signing-certificate"></a>创建签名证书

以下示例创建可用于对 Runbook 签名的自签名证书。 此代码创建并导出证书，使混合 Runbook 辅助角色稍后可以导入该证书。 此外还会返回指纹，以备稍后在引用证书时使用。

```powershell
# Create a self-signed certificate that can be used for code signing
$SigningCert = New-SelfSignedCertificate -CertStoreLocation cert:\LocalMachine\my `
                                        -Subject "CN=contoso.com" `
                                        -KeyAlgorithm RSA `
                                        -KeyLength 2048 `
                                        -Provider "Microsoft Enhanced RSA and AES Cryptographic Provider" `
                                        -KeyExportPolicy Exportable `
                                        -KeyUsage DigitalSignature `
                                        -Type CodeSigningCert


# Export the certificate so that it can be imported to the hybrid workers
Export-Certificate -Cert $SigningCert -FilePath .\hybridworkersigningcertificate.cer

# Import the certificate into the trusted root store so the certificate chain can be validated
Import-Certificate -FilePath .\hybridworkersigningcertificate.cer -CertStoreLocation Cert:\LocalMachine\Root

# Retrieve the thumbprint for later use
$SigningCert.Thumbprint
```

### <a name="import-certificate-and-configure-workers-for-signature-validation"></a>导入证书并配置辅助角色以进行签名验证

将创建的证书复制到组中的每个混合 Runbook 辅助角色。 运行以下脚本，以导入证书并将辅助角色配置为在 Runbook 中使用签名验证。

```powershell
# Install the certificate into a location that will be used for validation.
New-Item -Path Cert:\LocalMachine\AutomationHybridStore
Import-Certificate -FilePath .\hybridworkersigningcertificate.cer -CertStoreLocation Cert:\LocalMachine\AutomationHybridStore

# Import the certificate into the trusted root store so the certificate chain can be validated
Import-Certificate -FilePath .\hybridworkersigningcertificate.cer -CertStoreLocation Cert:\LocalMachine\Root

# Configure the hybrid worker to use signature validation on runbooks.
Set-HybridRunbookWorkerSignatureValidation -Enable $true -TrustedCertStoreLocation "Cert:\LocalMachine\AutomationHybridStore"
```

### <a name="sign-your-runbooks-using-the-certificate"></a>使用证书为 Runbook 签名

将混合 Runbook 辅助角色配置为仅使用已签名的 Runbook 后，必须对要在混合 Runbook 辅助角色上使用的 Runbook 签名。 使用以下示例 PowerShell 代码为这些 Runbook 进行签名。

```powershell
$SigningCert = ( Get-ChildItem -Path cert:\LocalMachine\My\<CertificateThumbprint>)
Set-AuthenticodeSignature .\TestRunbook.ps1 -Certificate $SigningCert
```

为 Runbook 签名后，必须将其导入自动化帐户，并使用签名块将其发布。 若要了解如何导入 Runbook，请参阅[将 Runbook 从文件导入 Azure 自动化](manage-runbooks.md#import-a-runbook)。

## <a name="working-with-signed-runbooks-on-a-linux-hybrid-runbook-worker"></a>使用 Linux 混合 Runbook 辅助角色上已签名的 Runbook

为了能够使用已签名的 Runbook，Linux 混合 Runbook 辅助角色必须在本地计算机上包含 [GPG](https://gnupg.org/index.html) 可执行文件。

> [!IMPORTANT]
> 将混合 Runbook 辅助角色配置为仅运行已签名的 Runbook 后，未签名的 Runbook 无法在该辅助角色上执行。

### <a name="create-a-gpg-keyring-and-keypair"></a>创建 GPG keyring 和密钥对

若要创建 GPG 密钥环和密钥对，请使用混合 Runbook 辅助角色 **nxautomation** 帐户。

1. 使用 sudo 应用程序以 **nxautomation** 帐户身份登录。

    ```bash
    sudo su – nxautomation
    ```

2. 使用 **nxautomation** 后，生成 GPG 密钥对。 GPG 将引导你完成这些步骤。 必须提供姓名、电子邮件地址、过期时间和通行短语。 然后，等到计算机上提供了足够的熵用于生成密钥。

    ```bash
    sudo gpg --generate-key
    ```

3. 由于 GPG 目录是使用 sudo 生成的，因此必须使用以下命令将其所有者更改为 **nxautomation**。

    ```bash
    sudo chown -R nxautomation ~/.gnupg
    ```

### <a name="make-the-keyring-available-to-the-hybrid-runbook-worker"></a>使密钥环可供混合 Runbook 辅助角色使用

创建密钥环后，使其可供混合 Runbook 辅助角色使用。 修改设置文件 **/var/opt/microsoft/omsagent/state/automationworker/diy/worker.conf**，以在文件节 `[worker-optional]` 下包括以下示例代码。

```bash
gpg_public_keyring_path = /var/opt/microsoft/omsagent/run/.gnupg/pubring.kbx
```

### <a name="verify-that-signature-validation-is-on"></a>验证签名验证是否已启用

如果计算机上已禁用签名验证，必须运行以下 sudo 命令将其启用。 请将 `<LogAnalyticsworkspaceId>` 替换为你的工作区 ID。

```bash
sudo python /opt/microsoft/omsconfig/modules/nxOMSAutomationWorker/DSCResources/MSFT_nxOMSAutomationWorkerResource/automationworker/scripts/require_runbook_signature.py --true <LogAnalyticsworkspaceId>
```

### <a name="sign-a-runbook"></a>对 runbook 签名

配置签名验证后，使用以下 GPG 命令为 Runbook 签名。

```bash
gpg –-clear-sign <runbook name>
```

已签名的 Runbook 名为 `<runbook name>.asc`。

现在，可将已签名的 Runbook 上传到 Azure 自动化，并像执行普通的 Runbook 一样执行它。

## <a name="next-steps"></a>后续步骤

* 若要详细了解 Runbook 的启动方法，请参阅[在 Azure 自动化中启动 Runbook](automation-starting-a-runbook.md)。
* 若要了解如何使用文本编辑器在 Azure 自动化中处理 PowerShell Runbook，请参阅[在 Azure 自动化中编辑 Runbook](automation-edit-textual-runbook.md)。
* 如果 Runbook 不能成功完成，请查看针对 [Runbook 执行失败](troubleshoot/hybrid-runbook-worker.md#runbook-execution-fails)的故障排除指南。
* 有关 PowerShell 的详细信息（包括语言参考和学习模块），请参阅 [PowerShell 文档](https://docs.microsoft.com/powershell/scripting/overview)。
