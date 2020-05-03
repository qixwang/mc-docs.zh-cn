---
title: 对 Linux VM 使用邻近放置组
description: 了解如何针对 Azure 中的 Linux 虚拟机创建和使用邻近放置组。
services: virtual-machines-linux
author: rockboyfor
manager: digimobile
ms.service: virtual-machines-linux
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
origin.date: 10/30/2019
ms.date: 04/30/2020
ms.author: v-yeche
ms.openlocfilehash: ad3d813b99becc1265f20288a48a1e4635b5140f
ms.sourcegitcommit: 275203d0f144aa809792f2c570fd64797f405c80
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/24/2020
ms.locfileid: "82125799"
---
<!--Verified successfully-->
# <a name="deploy-vms-to-proximity-placement-groups-using-azure-cli"></a>使用 Azure CLI 将 VM 部署到邻近放置组

若要让 VM 尽可能靠近，将延迟尽可能降至最低，应将 VM 部署到一个[邻近放置组](co-location.md#proximity-placement-groups)中。

邻近放置组是一种逻辑分组，用于确保 Azure 计算资源在物理上彼此靠近。 邻近放置组用于要求低延迟的工作负荷。

## <a name="create-the-proximity-placement-group"></a>创建邻近放置组
使用 [`az ppg create`](https://docs.microsoft.com/cli/azure/ppg?view=azure-cli-latest#az-ppg-create) 创建邻近放置组。 

```azurecli
az group create --name myPPGGroup --location chinanorth
az ppg create \
   -n myPPG \
   -g myPPGGroup \
   -l chinanorth \
   -t standard 
```

## <a name="list-proximity-placement-groups"></a>列出邻近放置组

可以使用 [az ppg list](https://docs.microsoft.com/cli/azure/ppg?view=azure-cli-latest#az-ppg-list) 来列出所有邻近放置组。

```azurecli
az ppg list -o table
```

## <a name="create-a-vm"></a>创建 VM

使用 [new az vm](https://docs.azure.cn/cli/vm?view=azure-cli-latest#az-vm-create) 在邻近放置组中创建 VM。

```azurecli
az vm create \
   -n myVM \
   -g myPPGGroup \
   --image UbuntuLTS \
   --ppg myPPG  \
   --generate-ssh-keys \
   --size Standard_D1_v2  \
   -l chinanorth
```

可以使用 [az ppg show](https://docs.microsoft.com/cli/azure/ppg?view=azure-cli-latest#az-ppg-show) 查看邻近放置组中的 VM。

```azurecli
az ppg show --name myppg --resource-group myppggroup --query "virtualMachines"
```

## <a name="availability-sets"></a>可用性集
还可以在邻近放置组中创建可用性集。 将同一 `--ppg` 参数与 [az vm availability-set create](https://docs.azure.cn/cli/vm/availability-set?view=azure-cli-latest#az-vm-availability-set-create) 一起使用来创建一个可用性集，并且也将在同一邻近放置组中创建该可用性集中的所有 VM。

## <a name="scale-sets"></a>规模集

还可以在邻近放置组中创建规模集。 将同一 `--ppg` 参数与 [az vmss create](https://docs.azure.cn/cli/vmss?view=azure-cli-latest#az-vmss-create) 一起使用来创建规模集，并且将在同一邻近放置组中创建所有实例。

## <a name="next-steps"></a>后续步骤

详细了解用于邻近放置组的 [Azure CLI](https://docs.microsoft.com/cli/azure/ppg?view=azure-cli-latest) 命令。

<!-- Update_Description: update meta properties, wording update, update link -->