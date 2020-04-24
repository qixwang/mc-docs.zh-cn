---
title: 创建具有静态公共 IP 地址的 VM - Azure CLI | Azure
description: 了解如何使用 Azure 命令行接口 (CLI) 创建具有静态公共 IP 地址的 VM。
services: virtual-network
documentationcenter: na
author: rockboyfor
manager: digimobile
editor: ''
tags: azure-resource-manager
ms.assetid: 55bc21b0-2a45-4943-a5e7-8d785d0d015c
ms.service: virtual-network
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
origin.date: 08/08/2018
ms.date: 02/24/2020
ms.author: v-yeche
ms.openlocfilehash: 3c4ed50d13be324de65d36c32d0dd0326cd968b2
ms.sourcegitcommit: c1ba5a62f30ac0a3acb337fb77431de6493e6096
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2020
ms.locfileid: "80109744"
---
# <a name="create-a-virtual-machine-with-a-static-public-ip-address-using-the-azure-cli"></a>使用 Azure CLI 创建具有静态公共 IP 地址的虚拟机

可以创建具有静态公共 IP 地址的虚拟机。 使用公共 IP 地址可以通过 Internet 来与虚拟机通信。 分配静态公共 IP 地址而非动态地址可以确保地址永远不会改变。 详细了解[静态公共 IP 地址](virtual-network-ip-addresses-overview-arm.md#allocation-method)。 若要将分配给现有虚拟机的公共 IP 地址从动态更改为静态，或者要使用专用 IP 地址，请参阅[添加、更改或删除 IP 地址](virtual-network-network-interface-addresses.md)。 公共 IP 地址会产生[少许费用](https://www.azure.cn/pricing/details/ip-addresses/)，可为每个订阅使用的公共 IP 地址数有[限制](../azure-resource-manager/management/azure-subscription-service-limits.md?toc=%2fvirtual-network%2ftoc.json#azure-resource-manager-virtual-networking-limits)。

<a name = "create"></a>
## <a name="create-a-virtual-machine"></a>创建虚拟机

可以从本地计算机完成以下步骤。 若要使用本地计算机，请确保[安装 Azure CLI](https://docs.azure.cn/cli/install-azure-cli?toc=%2fvirtual-network%2ftoc.json?view=azure-cli-latest)。 

[!INCLUDE [azure-cli-2-azurechinacloud-environment-parameter](../../includes/azure-cli-2-azurechinacloud-environment-parameter.md)]

1. 打开命令会话并使用 `az login` 登录到 Azure。
2. 使用 [az group create](https://docs.azure.cn/cli/group?view=azure-cli-latest#az-group-create) 命令创建资源组。 以下示例在“中国东部”Azure 区域中创建一个资源组：

    ```azurecli
    az group create --name myResourceGroup --location chinaeast
    ```

3. 使用 [az vm create](https://docs.azure.cn/cli/vm?view=azure-cli-latest#az-vm-create) 命令创建虚拟机。 `--public-ip-address-allocation=static` 选项向虚拟机分配静态公共 IP 地址。 以下示例使用名为 *myPublicIpAddress* 的静态基本 SKU 公共 IP 地址创建 Ubuntu 虚拟机：

    ```azurecli
    az vm create \
     --resource-group myResourceGroup \
     --name myVM \
     --image UbuntuLTS \
     --admin-username azureuser \
     --generate-ssh-keys \
     --public-ip-address myPublicIpAddress \
     --public-ip-address-allocation static
    ```

    如果公共 IP 地址必须是标准 SKU，请将 `--public-ip-sku Standard` 添加到上述命令。 详细了解[公共 IP 地址 SKU](virtual-network-ip-addresses-overview-arm.md#sku)。 如果虚拟机将添加到公共 Azure 负载均衡器的后端池，则虚拟机公共 IP 地址的 SKU 必须与负载均衡器的公共 IP 地址的 SKU 相匹配。 有关详细信息，请参阅 [Azure 负载均衡器](../load-balancer/load-balancer-overview.md?toc=%2fvirtual-network%2ftoc.json)。
    
    <!--Not Available #skus-->

4. 使用 [az network public-ip show](https://docs.azure.cn/cli/network/public-ip?view=azure-cli-latest#az-network-public-ip-show) 查看分配的公共 IP 地址，并确认它是否创建为静态基本 SKU 地址：

    ```azurecli
    az network public-ip show \
     --resource-group myResourceGroup \
     --name myPublicIpAddress \
     --query [ipAddress,publicIpAllocationMethod,sku] \
     --output table
    ```

    Azure 从你在其中创建虚拟机的区域使用的地址中分配了一个公共 IP 地址。 可以下载 Azure [中国](https://www.microsoft.com/download/confirmation.aspx?id=57062)云的范围（前缀）列表。

> [!WARNING]
> 不要修改虚拟机操作系统中的 IP 地址设置。 操作系统不知道 Azure 公共 IP 地址。 虽然可以向操作系统添加专用 IP 地址设置，但除非必要，否则我们建议不要这样做，而只能阅读[向操作系统添加专用 IP 地址](virtual-network-network-interface-addresses.md#private)之后才执行此操作。

## <a name="clean-up-resources"></a>清理资源

如果不再需要资源组及其包含的所有资源，可以使用 [az group delete](https://docs.azure.cn/cli/group?view=azure-cli-latest#az-group-delete) 将其删除：

```azurecli
az group delete --name myResourceGroup --yes
```

## <a name="next-steps"></a>后续步骤

- 详细了解 Azure 中的[公共 IP 地址](virtual-network-ip-addresses-overview-arm.md#public-ip-addresses)
- 详细了解所有[公共 IP 地址设置](virtual-network-public-ip-address.md#create-a-public-ip-address)
- 详细了解[专用 IP 地址](virtual-network-ip-addresses-overview-arm.md#private-ip-addresses)以及如何为 Azure 虚拟机分配[静态公共 IP 地址](virtual-network-network-interface-addresses.md#add-ip-addresses)
- 详细了解如何创建 [Linux](../virtual-machines/windows/tutorial-manage-vm.md?toc=%2fvirtual-network%2ftoc.json) 和 [Windows](../virtual-machines/windows/tutorial-manage-vm.md?toc=%2fvirtual-network%2ftoc.json) 虚拟机

<!-- Update_Description: update meta properties, wording update, update link -->