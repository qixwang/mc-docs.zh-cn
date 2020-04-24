---
title: Azure PowerShell 脚本示例 - 加密 Windows VM | Azure
description: Azure PowerShell 脚本示例 - 加密 Windows VM
services: virtual-machines-windows
documentationcenter: virtual-machines
author: rockboyfor
manager: digimobile
editor: tysonn
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-machines-windows
ms.topic: sample
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
origin.date: 12/12/2017
ms.date: 10/14/2019
ms.author: v-yeche
ms.openlocfilehash: 752d8e9fd1dbf55d26ffc7eec7035b83d085d525
ms.sourcegitcommit: c1ba5a62f30ac0a3acb337fb77431de6493e6096
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2020
ms.locfileid: "72272758"
---
# <a name="encrypt-a-windows-virtual-machine-with-azure-powershell"></a>使用 Azure PowerShell 加密 Windows 虚拟机

此脚本创建安全的 Azure Key Vault、加密密钥、Azure Active Directory 服务主体和 Windows 虚拟机 (VM)。 然后使用来自 Key Vault 和服务主体凭据的加密密钥对 VM 进行加密。

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [updated-for-az.md](../../../includes/updated-for-az.md)]

## <a name="sample-script"></a>示例脚本

```powershell
# Sign-in the Azure China Cloud
Connect-AzAccount -Environment AzureChinaCloud

# Edit these global variables with you unique Key Vault name, resource group name and location
#Name of the Key Vault
$keyVaultName = "myKeyVault00"
#Resource Group Name
$rgName = "myResourceGroup"
#Region
$location = "China East"
#Password to place w/in the KeyVault
$password = $([guid]::NewGuid()).Guid)
$securePassword = ConvertTo-SecureString -String $password -AsPlainText -Force
#Name for the Azure AD Application
$appName = "My App"
#Name for the VM to be encrypt
$vmName = "myEncryptedVM"
#user name for the admin account in the vm being created and then encrypted
$vmAdminName = "encryptedUser"

# Register the Key Vault provider and create a resource group
New-AzResourceGroup -Location $location -Name $rgName

# Create a Key Vault and enable it for disk encryption
New-AzKeyVault `
    -Location $location `
    -ResourceGroupName $rgName `
    -VaultName $keyVaultName `
    -EnabledForDiskEncryption

# Create a key in your Key Vault
Add-AzKeyVaultKey `
    -VaultName $keyVaultName `
    -Name "myKey" `
    -Destination "Software"

# Put the password in the Key Vault as a Key Vault Secret so we can use it later
# We should never put passwords in scripts.
Set-AzKeyVaultSecret -VaultName $keyVaultName -Name adminCreds -SecretValue $securePassword
Set-AzKeyVaultSecret -VaultName $keyVaultName -Name protectValue -SecretValue $securePassword

# Create Azure Active Directory app and service principal
$app = New-AzADApplication -DisplayName $appName `
    -HomePage "https://myapp0.contoso.com" `
    -IdentifierUris "https://contoso.com/myapp0" `
    -Password (Get-AzKeyVaultSecret -VaultName $keyVaultName -Name adminCreds).SecretValue

New-AzADServicePrincipal -ApplicationId $app.ApplicationId

# Set permissions to allow your AAD service principal to read keys from Key Vault
Set-AzKeyVaultAccessPolicy -VaultName $keyvaultName `
    -ServicePrincipalName $app.ApplicationId  `
    -PermissionsToKeys decrypt,encrypt,unwrapKey,wrapKey,verify,sign,get,list,update `
    -PermissionsToSecrets get,list,set,delete,backup,restore,recover,purge

# Create PSCredential object for VM
$cred = New-Object System.Management.Automation.PSCredential($vmAdminName, (Get-AzKeyVaultSecret -VaultName $keyVaultName -Name adminCreds).SecretValue)

# Create a virtual machine
New-AzVM `
  -ResourceGroupName $rgName `
  -Name $vmName `
  -Location $location `
  -ImageName "Win2016Datacenter" `
  -VirtualNetworkName "myVnet" `
  -SubnetName "mySubnet" `
  -SecurityGroupName "myNetworkSecurityGroup" `
  -PublicIpAddressName "myPublicIp" `
  -Credential $cred `
  -OpenPorts 3389

# Define required information for our Key Vault and keys
$keyVault = Get-AzKeyVault -VaultName $keyVaultName -ResourceGroupName $rgName;
$diskEncryptionKeyVaultUrl = $keyVault.VaultUri;
$keyVaultResourceId = $keyVault.ResourceId;
$keyEncryptionKeyUrl = (Get-AzKeyVaultKey -VaultName $keyVaultName -Name "myKey").Key.kid;

# Encrypt our virtual machine
Set-AzVMDiskEncryptionExtension `
    -ResourceGroupName $rgName `
    -VMName $vmName `
    -AadClientID $app.ApplicationId `
    -AadClientSecret (Get-AzKeyVaultSecret -VaultName $keyVaultName -Name adminCreds).SecretValueText `
    -DiskEncryptionKeyVaultUrl $diskEncryptionKeyVaultUrl `
    -DiskEncryptionKeyVaultId $keyVaultResourceId `
    -KeyEncryptionKeyUrl $keyEncryptionKeyUrl `
    -KeyEncryptionKeyVaultId $keyVaultResourceId

# View encryption status
Get-AzVmDiskEncryptionStatus  -ResourceGroupName $rgName -VMName $vmName

<#
#clean up
Remove-AzResourceGroup -Name $rgName
#removes all of the Azure AD Applications you created w/ the same name
Remove-AzADApplication -ObjectId $app.ObjectId -Force
#>

```

## <a name="clean-up-deployment"></a>清理部署

运行以下命令来删除资源组、VM 和所有相关资源。

```powershell
Remove-AzResourceGroup -Name myResourceGroup
```

## <a name="script-explanation"></a>脚本说明

此脚本使用以下命令创建部署。 表中的每一项均链接到特定于命令的文档。

| Command | 说明 |
|---|---|
| [New-AzResourceGroup](https://docs.microsoft.com/powershell/module/az.resources/new-azresourcegroup) | 创建用于存储所有资源的资源组。 |
| [New-AzKeyVault](https://docs.microsoft.com/powershell/module/az.keyvault/new-azkeyvault) | 创建 Azure Key Vault 来存储安全的数据，例如加密密钥。 |
| [Add-AzKeyVaultKey](https://docs.microsoft.com/powershell/module/az.keyvault/add-azkeyvaultkey) | 在 Key Vault 中创建加密密钥。 |
| [New-AzADServicePrincipal](https://docs.microsoft.com/powershell/module/az.resources/new-azadserviceprincipal) | 创建 Azure Active Directory 服务主体来安全地进行身份验证并控制对加密密钥的访问。 |
| [Set-AzKeyVaultAccessPolicy](https://docs.microsoft.com/powershell/module/az.keyvault/set-azkeyvaultaccesspolicy) | 设置对 Key Vault 的权限来向服务主体授予对加密密钥的访问权限。 |
| [New-AzVM](https://docs.microsoft.com/powershell/module/az.compute/new-azvm) | 创建虚拟机并将其连接到网卡、虚拟网络、子网和网络安全组。 此命令还将打开端口 80，并设置管理凭据。 |
| [Get-AzKeyVault](https://docs.microsoft.com/powershell/module/az.keyvault/get-azkeyvault) | 获取关于 Key Vault 的必需信息 |
| [Set-AzVMDiskEncryptionExtension](https://docs.microsoft.com/powershell/module/az.compute/set-azvmdiskencryptionextension) | 使用服务主体凭据和加密密钥在 VM 上启用加密。 |
| [Get-AzVmDiskEncryptionStatus](https://docs.microsoft.com/powershell/module/az.compute/get-azvmdiskencryptionstatus) | 显示 VM 加密过程的状态。 |
| [Remove-AzResourceGroup](https://docs.microsoft.com/powershell/module/az.resources/remove-azresourcegroup) | 删除资源组及其中包含的所有资源。 |

## <a name="next-steps"></a>后续步骤

有关 Azure PowerShell 模块的详细信息，请参阅 [Azure PowerShell 文档](https://docs.microsoft.com/powershell/azure/overview)。

可以在 [Azure Windows VM 文档](../windows/powershell-samples.md?toc=%2fvirtual-machines%2fwindows%2ftoc.json)中找到其他虚拟机 PowerShell 脚本示例。

<!-- Update_Description: update meta properties, update cmdlet content -->