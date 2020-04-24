---
title: Azure CLI 脚本示例 - 装载操作系统磁盘 | Azure
description: Azure CLI 脚本示例 - 装载操作系统磁盘
services: virtual-machines-linux
documentationcenter: virtual-machines
author: rockboyfor
manager: digimobile
editor: tysonn
tags: azure-service-management
ms.assetid: ''
ms.service: virtual-machines-linux
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
origin.date: 02/27/2017
ms.date: 08/12/2019
ms.author: v-yeche
ms.custom: mvc
ms.openlocfilehash: 6012ba0f1a2cbb721ea1ad9bae93be2acb7389d6
ms.sourcegitcommit: c1ba5a62f30ac0a3acb337fb77431de6493e6096
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2020
ms.locfileid: "69539027"
---
# <a name="troubleshoot-a-vms-operating-system-disk"></a>对 VM 操作系统磁盘进行故障排除

此脚本会失败或有问题的虚拟机的操作系统磁盘作为数据磁盘装载到第二个虚拟机。 排查磁盘问题或恢复数据时，此脚本会很有用。

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>示例脚本

```azurecli
#!/bin/bash

# Sign in the Azure China Cloud
az cloud set -n AzureChinaCloud
az login

# Source virtual machine details.
sourcevm=<Replace with vm name>
resourcegroup=<Replace with resource group name>

# Get the disk id for the source VM operating system disk.
diskid="$(az vm show -g $resourcegroup -n $sourcevm --query [storageProfile.osDisk.managedDisk.id] -o tsv)"

# Delete the source virtual machine, this will not delete the disk.
az vm delete -g $resourcegroup -n $sourcevm --yes

# Create a new virtual machine, this creates SSH keys if not present.
az vm create --resource-group $resourcegroup --name myVM --image UbuntuLTS --generate-ssh-keys

# Attach disk as a data disk to the newly created VM.
az vm disk attach --resource-group $resourcegroup --vm-name myVM --disk $diskid

# Configure disk on new VM.
ip=$(az vm list-ip-addresses --resource-group $resourcegroup --name myVM --query '[].virtualMachine.network.publicIpAddresses[0].ipAddress' -o tsv)
ssh $ip 'sudo mkdir /mnt/remountedOsDisk'
ssh $ip 'sudo mount -t ext4 /dev/sdc1 /mnt/remountedOsDisk'

```

## <a name="script-explanation"></a>脚本说明

此脚本使用以下命令创建资源组、虚拟机和所有相关资源。 表中的每条命令均链接到特定于命令的文档。

| Command | 说明 |
|---|---|
| [az vm show](https://docs.azure.cn/cli/vm?view=azure-cli-latest#az-vm-show) | 返回虚拟机列表。 在此示例中，查询选项用于返回虚拟机操作系统磁盘。 然后，将此值添加到名为“uri”的变量。 |
| [az vm delete](https://docs.azure.cn/cli/vm?view=azure-cli-latest#az-vm-delete) | 删除虚拟机。 |
| [az vm create](https://docs.azure.cn/cli/vm?view=azure-cli-latest#az-vm-create) | 创建虚拟机。  |
| [az vm disk attach](https://docs.azure.cn/cli/vm/disk?view=azure-cli-latest#az-vm-disk-attach) | 将磁盘附加到虚拟机。 |
| [az vm list-ip-addresses](https://docs.azure.cn/cli/vm?view=azure-cli-latest#az-vm-list-ip-addresses) | 返回虚拟机的 IP 地址。 |

## <a name="next-steps"></a>后续步骤

有关 Azure CLI 的详细信息，请参阅 [Azure CLI 文档](https://docs.azure.cn/cli/index?view=azure-cli-latest)。

可以在 [Azure Linux VM 文档](../linux/cli-samples.md?toc=%2fvirtual-machines%2flinux%2ftoc.json)中找到其他虚拟机 CLI 脚本示例。

<!--Update_Description: update meta properties, update cmdlet -->