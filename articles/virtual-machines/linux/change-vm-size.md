---
title: 使用 Azure CLI 重设 Linux 虚拟机大小 | Azure
description: 如何通过更改 VM 大小来增加或减少 Linux 虚拟机。
services: virtual-machines-linux
documentationcenter: na
author: rockboyfor
manager: digimobile
editor: ''
tags: ''
ms.assetid: e163f878-b919-45c5-9f5a-75a64f3b14a0
ms.service: virtual-machines-linux
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
origin.date: 02/10/2017
ms.date: 08/12/2019
ms.author: v-yeche
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 2f27aead8ad698483bc56b5dbd938682f0a64f45
ms.sourcegitcommit: c1ba5a62f30ac0a3acb337fb77431de6493e6096
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2020
ms.locfileid: "68912742"
---
# <a name="resize-a-linux-virtual-machine-using-azure-cli"></a>使用 Azure CLI 重设 Linux 虚拟机大小 

预配虚拟机 (VM) 后，可以通过更改 [VM 大小][vm-sizes]来扩展或缩减 VM。 在某些情况下，必须先解除分配 VM。 如果所需大小在托管 VM 的硬件群集上不可用，则需要解除分配 VM。 本文详细介绍了如何使用 Azure CLI 重设 Linux VM 大小。 

## <a name="resize-a-vm"></a>调整 VM 的大小
若要重设 VM 大小，需要安装最新版 [Azure CLI](https://docs.azure.cn/cli/install-az-cli2?view=azure-cli-latest)，并使用 [az login](https://docs.azure.cn/cli/reference-index?view=azure-cli-latest#az-login) 登录 Azure 帐户。

[!INCLUDE [azure-cli-2-azurechinacloud-environment-parameter](../../../includes/azure-cli-2-azurechinacloud-environment-parameter.md)]

1. 使用 [az vm list-vm-resize-options](https://docs.azure.cn/cli/vm?view=azure-cli-latest#az-vm-list-vm-resize-options) 查看托管 VM 的硬件群集上可用的 VM 大小的列表。 以下示例列出资源组 `myResourceGroup` 区域中名为 `myVM` 的 VM 的 VM 大小：

    ```azurecli
    az vm list-vm-resize-options --resource-group myResourceGroup --name myVM --output table
    ```

2. 如果列出了所需的 VM 大小，则使用 [az vm resize](https://docs.azure.cn/cli/vm?view=azure-cli-latest#az-vm-resize) 调整 VM 大小。 以下示例将名为 `myVM` 的 VM 调整为 `Standard_DS3_v2` 大小：

    ```azurecli
    az vm resize --resource-group myResourceGroup --name myVM --size Standard_DS3_v2
    ```

    在此过程中，VM 将重新启动。 重新启动后，现有 OS 和数据磁盘将重新映射。 临时磁盘上的所有内容会丢失。

3. 如果所需的 VM 大小未列出，则需先使用 [az vm deallocate](https://docs.azure.cn/cli/vm?view=azure-cli-latest#az-vm-deallocate)解除分配 VM。 然后，可以通过此过程将 VM 的大小调整为区域支持的任何可用大小，再启动该 VM。 以下步骤会将名为 `myResourceGroup` 的资源组中名为 `myVM` 的 VM 解除分配、调整大小并启动：

    ```azurecli
    az vm deallocate --resource-group myResourceGroup --name myVM
    az vm resize --resource-group myResourceGroup --name myVM --size Standard_DS3_v2
    az vm start --resource-group myResourceGroup --name myVM
    ```

   > [!WARNING]
   > 解除分配 VM 也会释放分配给该 VM 的所有动态 IP 地址。 OS 和数据磁盘不受影响。

## <a name="next-steps"></a>后续步骤
若要提高可伸缩性，请运行多个 VM 实例并进行横向扩展。有关详细信息，请参阅[自动缩放虚拟机规模集中的 Linux 计算机][scale-set]。 

<!-- links -->

[boot-diagnostics]: https://azure.microsoft.com/blog/boot-diagnostics-for-virtual-machines-v2/
[scale-set]: ../../virtual-machine-scale-sets/tutorial-autoscale-cli.md

<!-- Notice: URL is Correct on tutorial-autoscale-cli.md to replace virtual-machine-scale-sets-linux-autoscale.md-->

[vm-sizes]:sizes.md

<!-- Update_Description: update meta properties, wording update, update cmdlet -->
