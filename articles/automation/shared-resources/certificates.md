---
title: 管理 Azure 自动化中的证书
description: Azure 自动化会安全地存储证书，使 Runbook 和 DSC 配置能够访问这些证书，以便对 Azure 和第三方资源进行身份验证。 本文介绍了有关证书的详细信息，以及如何在文本和图形创作中使用证书。
services: automation
ms.service: automation
ms.subservice: shared-capabilities
author: WenJason
ms.author: v-jay
origin.date: 04/02/2019
ms.date: 05/25/2020
ms.topic: conceptual
manager: digimobile
ms.openlocfilehash: b8ac74a601ce292ee1c2babc043db38fd370daa4
ms.sourcegitcommit: 9811bf312e0d037cb530eb16c8d85238fd276949
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/02/2020
ms.locfileid: "84275561"
---
# <a name="manage-certificates-in-azure-automation"></a>管理 Azure 自动化中的证书

证书将安全地存储在 Azure 自动化中，以便可以使用 Azure 资源管理器资源的 **Get-AzureRmAutomationCertificate** 活动通过 Runbook 或 DSC 配置访问这些证书。 此功能允许创建使用证书进行身份验证的 Runbook 和 DSC 配置，或者将证书添加到 Azure 或第三方资源。

>[!NOTE]
>Azure 自动化中的安全资产包括凭据、证书、连接和加密的变量。 这些资产已使用针对每个自动化帐户生成的唯一密钥加密并存储在自动化中。 自动化将密钥存储在系统托管的 Key Vault 服务中。 在存储安全资产之前，自动化会从 Key Vault 加载该密钥，然后用它加密资产。 

## <a name="powershell-cmdlets-to-access-certificates"></a>用于访问证书的 PowerShell cmdlet

对于 AzureRM，下表中的 cmdlet 用于通过 Windows PowerShell 创建和管理自动化凭据资产。 可在自动化 Runbook 和 DSC 配置中使用的 [AzureRM.Automation 模块](https://docs.microsoft.com/powershell/azure/overview)已随附了这些 cmdlet。

|Cmdlet |说明|
| --- | ---|
|[Get-AzureRmAutomationCertificate](https://docs.microsoft.com/powershell/module/azurerm.automation/get-azurermautomationcertificate)|检索有关要在 Runbook 或 DSC 配置中使用的证书的信息。 只能从 Get-AutomationCertificate 活动中检索证书本身。|
|[New-AzureRmAutomationCertificate](https://docs.microsoft.com/powershell/module/azurerm.automation/new-azurermautomationcertificate)|在自动化中创建新证书。|
|[Remove-AzureRmAutomationCertificate](https://docs.microsoft.com/powershell/module/azurerm.automation/remove-azurermautomationcertificate)|从自动化中删除证书。|
|[Set-AzureRmAutomationCertificate](https://docs.microsoft.com/powershell/module/azurerm.automation/set-azurermautomationcertificate)|设置现有证书的属性，包括上传证书文件和设置 **.pfx** 文件的密码。|

[Add-AzureCertificate](https://docs.microsoft.com/powershell/module/servicemanagement/azure/add-azurecertificate) cmdlet 还可用于上传指定云服务的服务证书。

## <a name="internal-cmdlets-to-access-certificates"></a>用于访问证书的内部 cmdlet

下表中的内部 cmdlet 用于在 Runbook 中访问证书。 此 cmdlet 附带全局模块 `Orchestrator.AssetManagement.Cmdlets`。

| 内部 Cmdlet | 说明 |
|:---|:---|
|`Get-AutomationCertificate`|在 Runbook 或 DSC 配置中获取要使用的证书。 返回一个 [System.Security.Cryptography.X509Certificates.X509Certificate2](https://docs.microsoft.com/dotnet/api/system.security.cryptography.x509certificates.x509certificate2) 对象。|

> [!NOTE] 
> 应避免在 Runbook 或 DSC 配置中的 `Get-AutomationCertificate` 的 `Name` 参数中使用变量。 此类变量可能会使设计时发现 Runbook 或 DSC 配置与自动化变量之间的依赖关系变得复杂。

## <a name="python-2-functions-to-access-certificates"></a>用于访问证书的 Python 2 函数

使用下表中的函数可在 Python2 Runbook 中访问证书。

| 函数 | 说明 |
|:---|:---|
| `automationassets.get_automation_certificate` | 检索有关证书资产的信息。 |

> [!NOTE]
> 必须在 Python Runbook 开头导入 `automationassets` 模块才能访问资产函数。

## <a name="create-a-new-certificate"></a>创建新证书

创建新证书时，需要将 .cer 或 .pfx 文件上传到自动化。 将证书标记为可导出后，可以将其传出自动化证书存储区。 如果证书不可导出，则它只可用于在 Runbook 或 DSC 配置中签名。 自动化要求证书包含 **Microsoft 增强型 RSA 和 AES 加密提供程序**。

### <a name="create-a-new-certificate-with-the-azure-portal"></a>使用 Azure 门户创建新证书

1. 在自动化帐户中，选择“资产” > “证书” > “添加证书”。  
1. 在“名称”字段中，键入证书的名称。
1. 若要浏览 **.cer** 或 **.pfx** 文件，请在“上传证书文件”下选择“选择文件”。  如果选择了 **.pfx** 文件，请指定密码并指明是否可以导出该文件。
1. 选择“创建”以保存新证书资产。

### <a name="create-a-new-certificate-with-powershell"></a>使用 PowerShell 创建新证书

以下示例演示了如何创建新的自动化证书并将其标记为可导出。 此示例导入现有的 **.pfx** 文件。

```powershell
$certificateName = 'MyCertificate'
$PfxCertPath = '.\MyCert.pfx'
$CertificatePassword = ConvertTo-SecureString -String 'P@$$w0rd' -AsPlainText -Force
$ResourceGroup = "ResourceGroup01"

New-AzureRmAutomationCertificate -AutomationAccountName "MyAutomationAccount" -Name $certificateName -Path $PfxCertPath -Password $CertificatePassword -Exportable -ResourceGroupName $ResourceGroup
```

### <a name="create-a-new-certificate-with-a-resource-manager-template"></a>使用资源管理器模板创建新证书

以下示例演示如何通过 PowerShell 使用资源管理器模板将证书部署到自动化帐户：

```powershell-interactive
$AutomationAccountName = "<automation account name>"
$PfxCertPath = '<PFX cert path>'
$CertificatePassword = '<password>'
$certificateName = '<certificate name>'
$AutomationAccountName = '<automation account name>'
$flags = [System.Security.Cryptography.X509Certificates.X509KeyStorageFlags]::Exportable `
    -bor [System.Security.Cryptography.X509Certificates.X509KeyStorageFlags]::PersistKeySet `
    -bor [System.Security.Cryptography.X509Certificates.X509KeyStorageFlags]::MachineKeySet
# Load the certificate into memory
$PfxCert = New-Object -TypeName System.Security.Cryptography.X509Certificates.X509Certificate2 -ArgumentList @($PfxCertPath, $CertificatePassword, $flags)
# Export the certificate and convert into base 64 string
$Base64Value = [System.Convert]::ToBase64String($PfxCert.Export([System.Security.Cryptography.X509Certificates.X509ContentType]::Pkcs12))
$Thumbprint = $PfxCert.Thumbprint


$json = @"
{
    '`$schema': 'https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#',
    'contentVersion': '1.0.0.0',
    'resources': [
        {
            'name': '$AutomationAccountName/$certificateName',
            'type': 'Microsoft.Automation/automationAccounts/certificates',
            'apiVersion': '2015-10-31',
            'properties': {
                'base64Value': '$Base64Value',
                'thumbprint': '$Thumbprint',
                'isExportable': true
            }
        }
    ]
}
"@

$json | out-file .\template.json
New-AzureRmResourceGroupDeployment -Name NewCert -ResourceGroupName TestAzureAuto -TemplateFile .\template.json
```

## <a name="get-a-certificate"></a>获取证书

若要检索证书，请使用内部 `Get-AutomationCertificate` cmdlet。 不能使用 [Get-AzureRmAutomationCertificate](https://docs.microsoft.com/powershell/module/azurerm.automation/get-azurermautomationcertificate) cmdlet，因为它返回有关证书资产的信息，而不是证书本身的信息。

### <a name="textual-runbook-example"></a>文本 Runbook 示例

以下示例演示如何将证书添加到 Runbook 中的云服务。 在此示例中，已从加密的自动化变量检索了密码。

```powershell
$serviceName = 'MyCloudService'
$cert = Get-AutomationCertificate -Name 'MyCertificate'
$certPwd = Get-AzureRmAutomationVariable -ResourceGroupName "ResourceGroup01" `
-AutomationAccountName "MyAutomationAccount" -Name 'MyCertPassword'
Add-AzureCertificate -ServiceName $serviceName -CertToDeploy $cert
```

### <a name="graphical-runbook-example"></a>图形 Runbook 示例

通过在“库”窗格中右键单击证书并选择“添加到画布”，将内部 `Get-AutomationCertificate` cmdlet 的活动添加到图形 Runbook。

![将证书添加到画布的屏幕截图](../media/certificates/automation-certificate-add-to-canvas.png)

下图显示了在图形 Runbook 中使用证书的示例。 

![图形创作示例的屏幕截图](../media/certificates/graphical-runbook-add-certificate.png)

### <a name="python-2-example"></a>Python 2 示例

以下示例演示如何在 Python2 Runbook 中访问证书。

```python
# get a reference to the Azure Automation certificate
cert = automationassets.get_automation_certificate("AzureRunAsCertificate")

# returns the binary cert content  
print cert
```

## <a name="next-steps"></a>后续步骤

* 若要了解关于 Runbook 的常规信息，请参阅 [Azure 自动化中的 Runbook 执行](../automation-runbook-execution.md)。
* 有关 DSC 配置的详细信息，请参阅 [State Configuration 概述](../automation-dsc-overview.md)。 
