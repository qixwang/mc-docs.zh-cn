---
title: Azure CLI 脚本示例 - 使用虚拟硬盘创建 VM
description: Azure CLI 脚本示例 - 使用虚拟硬盘创建 VM。
services: virtual-machines-linux
documentationcenter: virtual-machines
author: rockboyfor
manager: digimobile
tags: azure-service-management
ms.assetid: ''
ms.service: virtual-machines-linux
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
origin.date: 03/09/2017
ms.date: 04/27/2020
ms.author: v-yeche
ms.custom: mvc
ms.openlocfilehash: df65baeea65a2fef0f743b1f486520ceec9e9004
ms.sourcegitcommit: b469d275694fb86bbe37a21227e24019043b9e88
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/30/2020
ms.locfileid: "82596023"
---
# <a name="create-a-vm-with-a-virtual-hard-disk"></a>使用虚拟硬盘创建 VM

本示例使用 VHD 创建虚拟机。
本示例创建资源组、存储帐户、容器，并通过将 VHD 上传到容器来创建 VM。
本示例将 ssh 公钥替换为用户的公钥，因此用户可以访问 VM。

用户需要可引导 VHD。 脚本会查找 `~/sample.vhd`。

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>示例脚本

```azurecli
#!/bin/bash

# Sign in the Azure China Cloud
az cloud set -n AzureChinaCloud
az login 

# Create a resource group
az group create -n myResourceGroup -l chinanorth

# Create the storage account to upload the vhd
az storage account create -g myResourceGroup -n mystorageaccount -l chinanorth --sku PREMIUM_LRS

# Get a storage key for the storage account
STORAGE_KEY=$(az storage account keys list -g myResourceGroup -n mystorageaccount --query "[?keyName=='key1'] | [0].value" -o tsv)

# Create the container for the vhd
az storage container create -n vhds --account-name mystorageaccount --account-key ${STORAGE_KEY}

# Upload the vhd to a blob
az storage blob upload -c vhds -f ~/sample.vhd -n sample.vhd --account-name mystorageaccount --account-key ${STORAGE_KEY}

# Create the vm from the vhd
az vm create -g myResourceGroup -n myVM --image "https://myStorageAccount.blob.core.chinacloudapi.cn/vhds/sample.vhd" \
        --os-type linux --admin-username deploy --generate-ssh-keys

# Update the deploy user with your ssh key
az vm user update --resource-group myResourceGroup -n custom-vm -u deploy --ssh-key-value "$(< ~/.ssh/id_rsa.pub)"

# Get public IP address for the VM
IP_ADDRESS=$(az vm list-ip-addresses -g az-cli-vhd -n custom-vm \
    --query "[0].virtualMachine.network.publicIpAddresses[0].ipAddress" -o tsv)

echo "You can now connect using 'ssh deploy@${IP_ADDRESS}'"
```

## <a name="clean-up-deployment"></a>清理部署 

运行以下命令来删除资源组、VM 和所有相关资源。

```azurecli 
az group delete -n az-cli-vhd
```

## <a name="script-explanation"></a>脚本说明

此脚本使用以下命令创建资源组、虚拟机、可用性集、负载均衡器和所有相关资源。 表中的每条命令均链接到特定于命令的文档。

| 命令 | 注释 |
|---|---|
| [az group create](https://docs.azure.cn/cli/group?view=azure-cli-latest#az-group-create) | 创建用于存储所有资源的资源组。 |
| [az storage account list](https://docs.azure.cn/cli/storage/account?view=azure-cli-latest#az-storage-account-list) | 列出存储帐户 |
| [az storage account check-name](https://docs.azure.cn/cli/storage/account?view=azure-cli-latest#az-storage-account-check-name) | 检查存储帐户名称是否有效且目前还不存在 |
| [az storage account keys list](https://docs.azure.cn/cli/storage/account/keys?view=azure-cli-latest#az-storage-account-keys-list) | 列出存储帐户的密钥 |
| [az storage blob exists](https://docs.azure.cn/cli/storage/blob?view=azure-cli-latest#az-storage-blob-exists) | 检查 Blob 是否存在 |
| [az storage container create](https://docs.azure.cn/cli/storage/container?view=azure-cli-latest#az-storage-container-create) | 在存储帐户中创建一个容器。 |
| [az storage blob upload](https://docs.azure.cn/cli/storage/blob?view=azure-cli-latest#az-storage-blob-upload) | 通过上传 VHD，在容器中创建一个 Blob。 |
| [az vm list](https://docs.azure.cn/cli/vm?view=azure-cli-latest#az-vm-list) | 与 `--query` 一起使用，用于检查 VM 名称是否已使用。 | 
| [az vm create](https://docs.azure.cn/cli/vm?view=azure-cli-latest#az-vm-create)| 创建虚拟机。 |
| [az vm list-ip-addresses](https://docs.azure.cn/cli/vm?view=azure-cli-latest#az-vm-list-ip-addresses) | 获取已创建虚拟机的 IP 地址。 |

<!--CORRECT ON [az vm create](https://docs.azure.cn/cli/vm?view=azure-cli-latest#az-vm-create)-->

## <a name="next-steps"></a>后续步骤

有关 Azure CLI 的详细信息，请参阅 [Azure CLI 文档](https://docs.azure.cn/cli/index?view=azure-cli-latest)。

可以在 [Azure Linux VM 文档](../linux/cli-samples.md?toc=%2fvirtual-machines%2flinux%2ftoc.json)中找到其他虚拟机 CLI 脚本示例。

<!-- Update_Description: update meta properties, wording update, update link -->