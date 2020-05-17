---
title: 使用 Batch 安全地访问 Key Vault - Azure Batch
description: 了解如何使用 Azure Batch 以编程方式访问 Key Vault 中的凭据。
services: batch
author: laurenhughes
manager: gwallace
ms.service: batch
ms.workload: big-compute
ms.topic: article
ms.date: 04/29/2020
ms.author: v-tawe
origin.date: 02/13/2020
ms.openlocfilehash: 242693e31737ec4e60e399ef6ec5695aae117e86
ms.sourcegitcommit: 1fbdefdace8a1d3412900c6c3f89678d8a9b29bc
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/07/2020
ms.locfileid: "82886976"
---
# <a name="securely-access-key-vault-with-batch"></a>使用 Batch 安全地访问 Key Vault

本文介绍如何设置 Batch 节点，以便安全地访问 Azure Key Vault 中存储的凭据。 在 Key Vault 中放置管理员凭据并对凭据进行硬编码以通过脚本访问 Key Vault 的做法没有任何意义。 解决方法是使用一个向 Batch 节点授予 Key Vault 访问权限的证书。 只需执行几个步骤，就可以为 Batch 安全地存储密钥。

若要从 Batch 节点对 Azure Key Vault 进行身份验证，你需要：

- 一个 Azure Active Directory (Azure AD) 凭据
- 一个证书
- 一个 Batch 帐户
- 一个 Batch 池，其中至少包含一个节点

## <a name="obtain-a-certificate"></a>获取证书

如果没有证书，则可使用 `makecert` 命令行工具生成自签名证书，这是获取证书的最简单方法。

通常可在以下路径中找到 `makecert`：`C:\Program Files (x86)\Windows Kits\10\bin\<arch>`。 以管理员身份打开命令提示符，然后使用以下示例导航到 `makecert`。

```console
cd C:\Program Files (x86)\Windows Kits\10\bin\x64
```

接下来，使用 `makecert` 工具创建名为 `batchcertificate.cer` 和 `batchcertificate.pvk` 的自签名证书文件。 使用的公用名 (CN) 对于此应用程序而言并不重要，但它有助于辨别证书的用途。

```console
makecert -sv batchcertificate.pvk -n "cn=batch.cert.mydomain.org" batchcertificate.cer -b 09/23/2019 -e 09/23/2019 -r -pe -a sha256 -len 2048
```

Batch 需要一个 `.pfx` 文件。 使用 [pvk2pfx](https://docs.microsoft.com/windows-hardware/drivers/devtest/pvk2pfx) 工具将 `makecert` 创建的 `.cer` 和 `.pvk` 文件转换为单个 `.pfx` 文件。

```console
pvk2pfx -pvk batchcertificate.pvk -spc batchcertificate.cer -pfx batchcertificate.pfx -po
```

## <a name="create-a-service-principal"></a>创建服务主体

将向某个**用户**或**服务主体**授予对 Key Vault 的访问权限。 若要以编程方式访问 Key Vault，请使用我们在上一步骤中创建的证书的服务主体。

有关 Azure 服务主体的详细信息，请参阅 [Azure Active Directory 中的应用程序和服务主体对象](../active-directory/develop/app-objects-and-service-principals.md)。

> [!NOTE]
> 服务主体必须与 Key Vault 位于同一个 Azure AD 租户中。

```powershell
$now = [System.DateTime]::Parse("2020-02-10")
# Set this to the expiration date of the certificate
$expirationDate = [System.DateTime]::Parse("2021-02-10")
# Point the script at the cer file you created $cerCertificateFilePath = 'c:\temp\batchcertificate.cer'
$cer = New-Object System.Security.Cryptography.X509Certificates.X509Certificate2
$cer.Import($cerCertificateFilePath)
# Load the certificate into memory
$credValue = [System.Convert]::ToBase64String($cer.GetRawCertData())
# Create a new AAD application that uses this certificate
$newADApplication = New-AzureRmADApplication -DisplayName "Batch Key Vault Access" -HomePage "https://batch.mydomain.com" -IdentifierUris "https://batch.mydomain.com" -certValue $credValue -StartDate $now -EndDate $expirationDate
# Create new AAD service principal that uses this application
$newAzureAdPrincipal = New-AzureRmADServicePrincipal -ApplicationId $newADApplication.ApplicationId
```

应用程序的 URL 并不重要，因为我们只是将其用于访问 Key Vault。

## <a name="grant-rights-to-key-vault"></a>授予 Key Vault 的权限

在上一步骤中创建的服务主体需要有权从 Key Vault 检索机密。 可以通过 Azure 门户或以下 PowerShell 命令授予权限。

```powershell
Set-AzureRmKeyVaultAccessPolicy -VaultName 'BatchVault' -ServicePrincipalName '"https://batch.mydomain.com' -PermissionsToSecrets 'Get'
```

## <a name="assign-a-certificate-to-a-batch-account"></a>将证书分配到 Batch 帐户

创建 Batch 池，转到池中的证书选项卡，然后分配创建的证书。 现在，该证书已出现在所有 Batch 节点上。

接下来，需要将该证书分配到 Batch 帐户。 将证书分配到帐户之后，我们就可以将其分配到池，然后分配到节点。 若要执行此操作，最简单的方法是在门户中转到你的 Batch 帐户，导航到“证书”，然后选择“添加”。   上传我们在[获取证书](#obtain-a-certificate)过程中生成的 `.pfx` 文件，并提供密码。 完成后，证书会添加到列表中，然后你就可以验证指纹。

现在，在创建 Batch 池时，可以导航到池中的“证书”，并将创建的证书分配到该池。  执行此操作时，请确保选择“本地计算机”作为存储位置。  证书会加载到池中的所有 Batch 节点上。

## <a name="install-azure-powershell"></a>安装 Azure PowerShell

如果打算在节点上使用 PowerShell 脚本来访问 Key Vault，则需安装 Azure PowerShell 库。 可通过几种方式实现此目的：如果节点安装了 Windows Management Framework (WMF) 5，则可使用 install-module 命令下载该库。 如果所用节点上未安装 WMF 5，则安装该库的最简单方法是将 Azure PowerShell `.msi` 文件捆绑到 Batch 文件，然后在 Batch 启动脚本的第一部分调用安装程序。 有关详细信息，请参阅此示例：

```powershell
$psModuleCheck=Get-Module -ListAvailable -Name Azure -Refresh
if($psModuleCheck.count -eq 0) {
    $psInstallerPath = Join-Path $downloadPath "azure-powershell.3.4.0.msi" Start-Process msiexec.exe -ArgumentList /i, $psInstallerPath, /quiet -wait
}
```

## <a name="access-key-vault"></a>访问密钥保管库

现在，我们可以通过在 Batch 节点上运行的脚本访问 Key Vault 了。 若要从脚本访问 Key Vault，只需让脚本使用证书对 Azure AD 进行身份验证即可。 若要在 PowerShell 中执行此操作，请使用以下示例命令。 为“指纹”、“应用 ID”（服务主体的 ID）和“租户 ID”（服务主体所在的租户）指定适当的 GUID。   

```powershell
Add-AzureRmAccount -ServicePrincipal -CertificateThumbprint -ApplicationId
```

完成身份验证后，便可像往常一样访问 KeyVault。

```powershell
$adminPassword=Get-AzureKeyVaultSecret -VaultName BatchVault -Name batchAdminPass
```

这是需要在脚本中使用的凭据。
