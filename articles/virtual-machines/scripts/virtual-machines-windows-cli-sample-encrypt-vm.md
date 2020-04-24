---
title: Azure CLI 脚本示例 - 加密 Windows VM | Azure
description: Azure CLI Script 示例 - 加密 Windows VM
services: virtual-machines-windows
documentationcenter: virtual-machines
author: rockboyfor
manager: digimobile
editor: tysonn
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-machines-windows
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
origin.date: 12/15/2017
ms.date: 08/12/2019
ms.author: v-yeche
ms.openlocfilehash: 2c13b29b6749612596c15135dd14038597f2c0ec
ms.sourcegitcommit: c1ba5a62f30ac0a3acb337fb77431de6493e6096
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2020
ms.locfileid: "69539086"
---
# <a name="encrypt-a-windows-virtual-machine-in-azure"></a>加密 Azure 中的 Windows 虚拟机

此脚本创建安全的 Azure Key Vault、加密密钥、Azure Active Directory 服务主体和 Windows 虚拟机 (VM)。 然后使用来自 Key Vault 和服务主体凭据的加密密钥对 VM 进行加密。

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>示例脚本

```azurecli
#!/bin/bash

# Sign in the Azure China Cloud
az cloud set -n AzureChinaCloud
az login

# Provide your own unique Key Vault name
keyvault_name=<your_unique_keyvault_name>

# Register the Key Vault provider and create a resource group.
az provider register -n Microsoft.KeyVault
az group create --name myResourceGroup --location chinaeast

# Create a Key Vault for storing keys and enabled for disk encryption.
az keyvault create --name $keyvault_name --resource-group myResourceGroup --location chinaeast \
    --enabled-for-disk-encryption True

# Create a key within the Key Vault.
az keyvault key create --vault-name $keyvault_name --name myKey --protection software

# Create an Azure Active Directory service principal for authenticating requests to Key Vault.
# Read in the service principal ID and password for use in later commands.
read sp_id sp_password <<< $(az ad sp create-for-rbac --query [appId,password] -o tsv)

# Grant permissions on the Key Vault to the AAD service principal.
az keyvault set-policy --name $keyvault_name \
    --spn $sp_id \
    --key-permissions wrapKey \
    --secret-permissions set

# Create a virtual machine.
az vm create \
    --resource-group myResourceGroup \
    --name myVM \
    --name myVM --image win2016datacenter \
    --admin-username azureuser \
    --admin-password myPassword12

# Encrypt the VM disks.
az vm encryption enable --resource-group myResourceGroup --name myVM \
  --aad-client-id $sp_id \
  --aad-client-secret $sp_password \
  --disk-encryption-keyvault $keyvault_name \
  --key-encryption-key myKey \
  --volume-type all

# Output how to monitor the encryption status and next steps.
echo "The encryption process can take some time. View status with:

    az vm encryption show --resource-group myResourceGroup --name myVM --query [osDisk] -o tsv

When encryption status shows \`Encrypted\`, restart the VM with:

    az vm restart --resource-group myResourceGroup --name myVM"

```

## <a name="clean-up-deployment"></a>清理部署 

运行以下命令来删除资源组、VM 和所有相关资源。

```azurecli
az group delete --name myResourceGroup
```

## <a name="script-explanation"></a>脚本说明

此脚本使用以下命令创建资源组、Azure Key Vault、服务主体、虚拟机和所有相关资源。 表中的每条命令均链接到特定于命令的文档。

| Command | 说明 |
|---|---|
| [az group create](https://docs.azure.cn/cli/group?view=azure-cli-latest#az-group-create) | 创建用于存储所有资源的资源组。 |
| [az keyvault create](https://docs.azure.cn/cli/keyvault?view=azure-cli-latest#az-keyvault-create) | 创建 Azure Key Vault 来存储安全的数据，例如加密密钥。 |
| [az keyvault key create](https://docs.azure.cn/cli/keyvault/key?view=azure-cli-latest#az-keyvault-key-create) | 在 Key Vault 中创建加密密钥。 |
| [az ad sp create-for-rbac](https://docs.azure.cn/cli/ad/sp?view=azure-cli-latest#az-ad-sp-create-for-rbac) | 创建 Azure Active Directory 服务主体来安全地进行身份验证并控制对加密密钥的访问。 |
| [az keyvault set-policy](https://docs.azure.cn/cli/keyvault?view=azure-cli-latest#az-keyvault-set-policy) | 设置对 Key Vault 的权限来向服务主体授予对加密密钥的访问权限。 |
| [az vm create](https://docs.azure.cn/cli/vm?view=azure-cli-latest#az-vm-create) | 创建虚拟机并将其连接到网卡、虚拟网络、子网和 NSG。 此命令还指定要使用的虚拟机映像和管理凭据。  |
| [az vm encryption enable](https://docs.azure.cn/cli/vm/encryption?view=azure-cli-latest#az-vm-encryption-enable) | 使用服务主体凭据和加密密钥在 VM 上启用加密。 |
| [az vm encryption show](https://docs.azure.cn/cli/vm/encryption?view=azure-cli-latest#az-vm-encryption-show) | 显示 VM 加密过程的状态。 |
| [az group delete](https://docs.azure.cn/cli/group?view=azure-cli-latest#az-group-delete) | 删除资源组，包括所有嵌套的资源。 |

## <a name="next-steps"></a>后续步骤

有关 Azure CLI 的详细信息，请参阅 [Azure CLI 文档](https://docs.azure.cn/cli/index?view=azure-cli-latest)。

可以在 [Azure Windows VM 文档](../windows/cli-samples.md?toc=%2fvirtual-machines%windows%2ftoc.json)中找到其他虚拟机 CLI 脚本示例。

<!--Update_Description: update meta propreties, update link -->