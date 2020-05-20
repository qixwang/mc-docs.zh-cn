---
title: 加密 Windows VM 上的虚拟磁盘 | Azure
description: 使用 Azure PowerShell 加密 Windows VM 上的虚拟磁盘以增强安全性
services: virtual-machines-windows
documentationcenter: ''
author: rockboyfor
manager: digimobile
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-machines-windows
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
origin.date: 10/30/2018
ms.date: 11/11/2019
ms.author: v-yeche
ms.openlocfilehash: c4832c204e344ddf696b002e48080eb0f560abbe
ms.sourcegitcommit: c1ba5a62f30ac0a3acb337fb77431de6493e6096
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2020
ms.locfileid: "74116780"
---
# <a name="encrypt-virtual-disks-on-a-windows-vm"></a>加密 Windows VM 上的虚拟磁盘
为了增强虚拟机 (VM) 的安全性以及符合性，可以加密 Azure 中的虚拟磁盘。 磁盘是使用 Azure Key Vault 中受保护的加密密钥加密的。 可以控制这些加密密钥，以及审核对它们的使用。 本文介绍如何使用 Azure PowerShell 加密 Windows VM 上的虚拟磁盘。 还可以[加密 Linux 虚拟机](../linux/disk-encryption-overview.md)。

[!INCLUDE [updated-for-az.md](../../../includes/updated-for-az.md)]

## <a name="overview-of-disk-encryption"></a>磁盘加密概述
Windows VM 上的虚拟磁盘使用 BitLocker 进行静态加密。 加密 Azure 中的虚拟磁盘不会产生费用。 可以使用软件保护将加密密钥存储在 Azure Key Vault 中。 加密密钥用于加密和解密附加到 VM 的虚拟磁盘。 可以控制这些加密密钥，以及审核对它们的使用。 

<!--Not Available on or you can import or generate your keys in Hardware Security Modules (HSMs) certified to FIPS 140-2 level 2 standards.-->
<!-- Not Available on 或者，可在已通过 FIPS 140-2 级别 2 标准认证的硬件安全模块 (HSM) 中导入或生成密钥。-->

加密 VM 的过程如下：

1. 在 Azure 密钥保管库中创建加密密钥。
1. 配置可用于加密磁盘的加密密钥。
1. 为虚拟磁盘启用磁盘加密。
1. 从 Azure Key Vault 请求所需的加密密钥。
1. 使用提供的加密密钥加密虚拟磁盘。

## <a name="requirements-and-limitations"></a>要求和限制

磁盘加密支持的方案和要求

* 在来自 Azure 市场映像或自定义 VHD 映像的新 Windows VM 上启用加密。
* 在现有的 Azure Windows VM 上启用加密。
* 在使用存储空间配置的 Windows VM 上启用加密。
* 在 Windows VM 的 OS 和数据驱动器上禁用加密。
* 标准层 VM，例如 A、D 和 DS 系列 VM。

    <!-- Not Available on G and GS series VM-->

    > [!NOTE]
    > 所有资源（包括 Key Vault、存储帐户和 VM）必须在同一个 Azure 区域和订阅中。

以下方案目前不支持磁盘加密：

* 基本层 VM。
* 使用经典部署模型创建的 VM。
* 在已加密的 VM 上更新加密密钥。
* 与本地密钥管理服务集成。

## <a name="create-an-azure-key-vault-and-keys"></a>创建 Azure Key Vault 和密钥
在开始之前，请确保已安装了 Azure PowerShell 模块的最新版本。 有关详细信息，请参阅[如何安装和配置 Azure PowerShell](https://docs.microsoft.com/powershell/azure/overview)。 在以下命令示例中，请将所有示例参数（例如 *myResourceGroup*、*myKeyVault*、*myVM* 等等）替换为自己的名称、位置和密钥值。

第一步是创建用于存储加密密钥的 Azure 密钥保管库。 Azure Key Vault 可以存储能够在应用程序和服务中安全实现的密钥、机密或密码。 对于虚拟磁盘加密，可以创建 Key Vault 来存储用于加密或解密虚拟磁盘的加密密钥。 

在 Azure 订阅中使用 [Register-AzResourceProvider](https://docs.microsoft.com/powershell/module/az.resources/register-azresourceprovider) 启用 Azure 密钥保管库提供程序，然后使用 [New-AzResourceGroup](https://docs.microsoft.com/powershell/module/az.resources/new-azresourcegroup) 创建资源组。 以下示例在“中国东部”位置创建名为 *myResourceGroup* 的资源组。 

```powershell
$rgName = "myResourceGroup"
$location = "China East"

Register-AzResourceProvider -ProviderNamespace "Microsoft.KeyVault"
New-AzResourceGroup -Location $location -Name $rgName
```

包含加密密钥和关联的计算资源（例如存储和 VM 本身）的 Azure Key Vault 必须位于同一区域。 使用 [New-AzKeyVault](https://docs.microsoft.com/powershell/module/az.keyvault/new-azkeyvault) 创建 Azure 密钥保管库，并启用该密钥保管库进行磁盘加密。 指定 keyVaultName  的唯一 Key Vault 名称，如下所示：

```powershell
$keyVaultName = "myKeyVault$(Get-Random)"
New-AzKeyVault -Location $location `
    -ResourceGroupName $rgName `
    -VaultName $keyVaultName `
    -EnabledForDiskEncryption
```

可以使用软件保护来存储加密密钥。 标准 Key Vault 仅存储受软件保护的密钥。 由于我们创建的是标准密钥保管库，以下示例使用了受软件保护的密钥。 

<!--Not Available on  or Hardware Security Model (HSM) protection-->
<!--Not Available on Using an HSM requires a premium Key Vault at an additional cost. To create a premium Key Vault, in the preceding step add the *-Sku "Premium"* parameter.-->
<!--Not Available on 或硬件安全模型 (HSM) 保护-->
<!--Not Available on 高级密钥保管库-->

对于这种保护模型，在启动 VM 以解密虚拟磁盘时，需要向 Azure 平台授予访问权限，使之能够请求加密密钥。 使用 [Add-AzureKeyVaultKey](https://docs.microsoft.com/powershell/module/az.keyvault/add-azkeyvaultkey) 在 Key Vault 中创建加密密钥。 以下示例创建名为 myKey  的密钥：

<!--Correct for this protection models-->

```powershell
Add-AzKeyVaultKey -VaultName $keyVaultName `
    -Name "myKey" `
    -Destination "Software"
```

<!--MOONCAKE CUSTOMIZED-->
<!--UPDATE BE CAREFULLY-->

## <a name="create-the-azure-active-directory-service-principal"></a><a name="create-the-azure-active-directory-service-principal"></a>创建 Azure Active Directory 服务主体

加密或解密虚拟磁盘时，会指定一个帐户来处理身份验证以及交换 Key Vault 中的加密密钥。 此帐户（Azure Active Directory 服务主体）允许 Azure 平台代表 VM 请求相应的加密密钥。 订阅中提供了一个默认的 Azure Active Directory 实例，不过，许多组织使用专用的 Azure Active Directory 目录。 

在 Azure Active Directory 中创建服务主体。 指定安全密码时，请遵循 [Azure Active Directory 中的密码策略和限制](../../active-directory/active-directory-passwords-policy.md)：

```powershell
$appName = "My App"
$Password = "P@ssword!"
$securePassword = ConvertTo-SecureString $Password -AsPlainText -Force
$app = New-AzADApplication -DisplayName $appName -HomePage "https://myapp.contoso.com" `
    -IdentifierUris "https://contoso.com/myapp" -Password $securePassword

New-AzADServicePrincipal -ApplicationId $app.ApplicationId
```

若要成功加密或解密虚拟磁盘，必须将 Key Vault 中存储的加密密钥设置为允许 Azure Active Directory 服务主体读取密钥。 使用以下命令在 Key Vault 上设置权限： 

```powershell
Set-AzKeyVaultAccessPolicy -VaultName $keyVaultName `
    -ServicePrincipalName $app.ApplicationId `
    -PermissionsToKeys "WrapKey" `
    -PermissionsToSecrets "Set"
```

<!--MOONCAKE CUSTOMIZED-->
<!--UPDATE BE CAREFULLY-->

## <a name="create-a-virtual-machine"></a>创建虚拟机
若要测试加密过程，请使用 [New-AzVm](https://docs.microsoft.com/powershell/module/az.compute/new-azvm) 创建 VM。 以下示例使用 Windows Server 2016 Datacenter 映像创建名为 myVM 的 VM。 系统提示输入凭据时，请输入用于 VM 的用户名和密码：

```powershell
$cred = Get-Credential

New-AzVm `
    -ResourceGroupName $rgName `
    -Name "myVM" `
    -Location $location `
    -VirtualNetworkName "myVnet" `
    -SubnetName "mySubnet" `
    -SecurityGroupName "myNetworkSecurityGroup" `
    -PublicIpAddressName "myPublicIpAddress" `
    -Credential $cred
```

## <a name="encrypt-a-virtual-machine"></a>加密虚拟机
使用 Azure 密钥保管库密钥通过 [Set-AzVMDiskEncryptionExtension](https://docs.microsoft.com/powershell/module/az.compute/set-azvmdiskencryptionextension) 加密 VM。 以下示例将检索所有密钥信息，然后对名为 myVM  的 VM 进行加密：

<!--MOONCAKE CUSTOMIZED-->
<!--UPDATE BE CAREFULLY-->

```powershell
$keyVault = Get-AzKeyVault -VaultName $keyVaultName -ResourceGroupName $rgName;
$diskEncryptionKeyVaultUrl = $keyVault.VaultUri;
$keyVaultResourceId = $keyVault.ResourceId;
$keyEncryptionKeyUrl = (Get-AzKeyVaultKey -VaultName $keyVaultName -Name myKey).Key.kid;

Set-AzVMDiskEncryptionExtension -ResourceGroupName $rgName `
    -VMName myVM -AadClientID $app.ApplicationId `
    -AadClientSecret $Password -DiskEncryptionKeyVaultUrl $diskEncryptionKeyVaultUrl `
    -DiskEncryptionKeyVaultId $keyVaultResourceId -KeyEncryptionKeyUrl $keyEncryptionKeyUrl `
    -KeyEncryptionKeyVaultId $keyVaultResourceId `
    -VolumeType all
```

<!--MOONCAKE CUSTOMIZED-->
<!--UPDATE BE CAREFULLY-->

接受提示以继续进行 VM 加密。 此过程中将重启 VM。 加密过程完成并重启 VM 后，使用 [Get-AzVmDiskEncryptionStatus](https://docs.microsoft.com/powershell/module/az.compute/get-azvmdiskencryptionstatus) 查看加密状态：

```powershell
Get-AzVmDiskEncryptionStatus  -ResourceGroupName $rgName -VMName "myVM"
```

输出类似于以下示例：

```powershell
OsVolumeEncrypted          : Encrypted
DataVolumesEncrypted       : Encrypted
OsVolumeEncryptionSettings : Microsoft.Azure.Management.Compute.Models.DiskEncryptionSettings
ProgressMessage            : OsVolume: Encrypted, DataVolumes: Encrypted
```

## <a name="next-steps"></a>后续步骤
* 有关 Azure Key Vault 管理的详细信息，请参阅[为虚拟机设置 Key Vault](key-vault-setup.md)。

<!--Not Avaialble on * [Azure Disk Encryption](../../security/azure-security-disk-encryption.md)-->

<!--Update_Description: wording update, update link, update cmdlet -->