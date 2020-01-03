---
title: 'PowerShell：启用 BYOK TDE - Azure SQL 数据库托管实例 '
description: 了解如何配置 Azure SQL 托管实例，以开始使用 BYOK 透明数据加密 (TDE) 通过 PowerShell 进行静态加密。
services: sql-database
ms.service: sql-database
ms.subservice: security
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: WenJason
ms.author: v-jay
ms.reviewer: vanto, carlrab
origin.date: 11/05/2019
ms.date: 12/16/2019
ms.openlocfilehash: 9ef0909e5f6dae35b6ca3c9297cddfbe40aead92
ms.sourcegitcommit: 4a09701b1cbc1d9ccee46d282e592aec26998bff
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/25/2019
ms.locfileid: "75334605"
---
# <a name="manage-transparent-data-encryption-in-a-managed-instance-using-your-own-key-from-azure-key-vault"></a>使用 Azure Key Vault 中自己的密钥管理托管实例中的透明数据加密

此 PowerShell 脚本实例使用 Azure Key Vault 中的密钥为 Azure SQL 托管实例配置使用客户托管密钥的透明数据加密 (TDE)。 这通常称为 TDE 的自带密钥方案。 若要详细了解使用客户托管密钥的 TDE，请参阅[适用于 Azure SQL 的支持“创建自己的密钥”的 TDE](../transparent-data-encryption-byok-azure-sql.md)。

## <a name="prerequisites"></a>先决条件

- 现有托管实例。 请参阅[使用 PowerShell 创建 Azure SQL 数据库托管实例](sql-database-create-configure-managed-instance-powershell.md)。

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]
[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

使用 PowerShell 需要 AZ PowerShell 2.3.2 或更高版本。 如果需要升级，请参阅[安装 Azure PowerShell 模块](https://docs.microsoft.com/powershell/azure/install-az-ps)，或运行以下示例脚本，以便为当前用户安装该模块：

`Install-Module -Name Az -AllowClobber -Scope CurrentUser`

此外，还需要运行 `Connect-AzAccount` 以创建与 Azure 的连接。

## <a name="sample-scripts"></a>示例脚本

```powershell
# You will need an existing Managed Instance as a prerequisite for completing this script.
# See https://docs.azure.cn/sql-database/scripts/sql-database-create-configure-managed-instance-powershell

# Log in to your Azure account:
Connect-AzAccount -Environment AzureChinaCloud

# If there are multiple subscriptions, choose the one where AKV is created: 
Set-AzContext -SubscriptionId "subscription ID"

# Install the preview version of Az.Sql PowerShell package 1.1.1-preview if you are running this PowerShell locally (uncomment below):
# Install-Module -Name Az.Sql -RequiredVersion 1.1.1-preview -AllowPrerelease -Force

# 1. Create Resource and setup Azure Key Vault (skip if already done)

# Create Resource group (name the resource and specify the location)
$location = "chinaeast" # specify the location
$resourcegroup = "MyRG" # specify a new RG name
New-AzResourceGroup -Name $resourcegroup -Location $location

# Create new Azure Key Vault with a globally unique VaultName and soft-delete option turned on:
$vaultname = "MyKeyVault" # specify a globally unique VaultName
New-AzKeyVault -VaultName $vaultname -ResourceGroupName $resourcegroup -Location $location -EnableSoftDelete

# Authorize Managed Instance to use the AKV (wrap/unwrap key and get public part of key, if public part exists): 
$objectid = (Set-AzSqlInstance -ResourceGroupName $resourcegroup -Name "MyManagedInstance" -AssignIdentity).Identity.PrincipalId
Set-AzKeyVaultAccessPolicy -BypassObjectIdValidation -VaultName $vaultname -ObjectId $objectid -PermissionsToKeys get,wrapKey,unwrapKey

# Allow access from trusted Azure services: 
Update-AzKeyVaultNetworkRuleSet -VaultName $vaultname -Bypass AzureServices

# Turn the network rules ON by setting the default action to Deny: 
Update-AzKeyVaultNetworkRuleSet -VaultName $vaultname -DefaultAction Deny


# 2. Provide TDE Protector key (skip if already done)

# The recommended way is to import an existing key from a .pfx file. Replace "<PFX private key password>" with the actual password below:
$keypath = "c:\some_path\mytdekey.pfx" # Supply your .pfx path and name
$securepfxpwd = ConvertTo-SecureString -String "<PFX private key password>" -AsPlainText -Force 
$key = Add-AzKeyVaultKey -VaultName $vaultname -Name "MyTDEKey" -KeyFilePath $keypath -KeyFilePassword $securepfxpwd

# ...or get an existing key from the vault:
# $key = Get-AzKeyVaultKey -VaultName $vaultname -Name "MyTDEKey"

# Alternatively, generate a new key directly in Azure Key Vault (recommended for test purposes only - uncomment below):
# $key = Add-AzureKeyVaultKey -VaultName $vaultname -Name MyTDEKey -Destination Software -Size 2048

# 3. Set up BYOK TDE on Managed Instance:

# Assign the key to the Managed Instance:
# $key = 'https://contoso.vault.azure.cn/keys/contosokey/01234567890123456789012345678901'
Add-AzSqlInstanceKeyVaultKey -KeyId $key.id -InstanceName "MyManagedInstance" -ResourceGroupName $resourcegroup

# Set TDE operation mode to BYOK: 
Set-AzSqlInstanceTransparentDataEncryptionProtector -Type AzureKeyVault -InstanceName "MyManagedInstance" -ResourceGroup $resourcegroup -KeyId $key.id
```

## <a name="next-steps"></a>后续步骤

有关 Azure PowerShell 的详细信息，请参阅 [Azure PowerShell 文档](https://docs.microsoft.com/powershell/azure/overview)。

可以在 [Azure SQL 数据库 PowerShell 脚本](../sql-database-powershell-samples.md)中找到更多 SQL 数据库 PowerShell 脚本示例。
