---
title: 如何在 Azure 中创建具有多个网络接口卡的 Linux 虚拟机
description: 了解如何使用 Azure CLI 或 Resource Manager 模板创建附有多个 NIC 的 Linux VM。
services: virtual-machines-linux
documentationcenter: ''
author: rockboyfor
manager: digimobile
editor: ''
ms.assetid: 5d2d04d0-fc62-45fa-88b1-61808a2bc691
ms.service: virtual-machines-linux
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
origin.date: 06/07/2018
ms.date: 02/10/2020
ms.author: v-yeche
ms.openlocfilehash: 69ba626778dbf21839f5c96b7568c59147abf69d
ms.sourcegitcommit: ada94ca4685855f58616e4bf1dd5ca757878dfdc
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/18/2020
ms.locfileid: "77428581"
---
# <a name="how-to-create-a-linux-virtual-machine-in-azure-with-multiple-network-interface-cards"></a>如何在 Azure 中创建具有多个网络接口卡的 Linux 虚拟机

本文详述了如何使用 Azure CLI 创建具有多个 NIC 的 VM。

## <a name="create-supporting-resources"></a>创建支持资源
安装最新的 [Azure CLI](https://docs.azure.cn/cli/install-az-cli2?view=azure-cli-latest) 并使用 [az login](https://docs.azure.cn/cli/reference-index?view=azure-cli-latest#az-login) 登录到 Azure 帐户。

[!INCLUDE [azure-cli-2-azurechinacloud-environment-parameter](../../../includes/azure-cli-2-azurechinacloud-environment-parameter.md)]

在以下示例中，请将示例参数名称替换成自己的值。 示例参数名称包括 myResourceGroup  、mystorageaccount  和 myVM  。

首先，使用 [az group create](https://docs.azure.cn/cli/group?view=azure-cli-latest#az-group-create) 创建资源组。 以下示例在“chinaeast”  位置创建名为“myResourceGroup”  的资源组：

```azurecli
az group create --name myResourceGroup --location chinaeast
```

使用 [az network vnet create](https://docs.azure.cn/cli/network/vnet?view=azure-cli-latest#az-network-vnet-create) 创建虚拟网络。 以下示例创建一个名为 myVnet  的虚拟网络和一个名为 mySubnetFrontEnd  的子网：

```azurecli
az network vnet create \
    --resource-group myResourceGroup \
    --name myVnet \
    --address-prefix 10.0.0.0/16 \
    --subnet-name mySubnetFrontEnd \
    --subnet-prefix 10.0.1.0/24
```

使用 [az network vnet subnet create](https://docs.azure.cn/cli/network/vnet/subnet?view=azure-cli-latest#az-network-vnet-subnet-create) 为后端通信流创建子网。 以下示例创建名为 mySubnetBackEnd  的子网：

```azurecli
az network vnet subnet create \
    --resource-group myResourceGroup \
    --vnet-name myVnet \
    --name mySubnetBackEnd \
    --address-prefix 10.0.2.0/24
```

使用 [az network nsg create](https://docs.azure.cn/cli/network/nsg?view=azure-cli-latest#az-network-nsg-create) 创建网络安全组。 以下示例创建名为“myNetworkSecurityGroup”  的网络安全组：

```azurecli
az network nsg create \
    --resource-group myResourceGroup \
    --name myNetworkSecurityGroup
```

## <a name="create-and-configure-multiple-nics"></a>创建和配置多个 NIC
使用 [az network nic create](https://docs.azure.cn/cli/network/nic?view=azure-cli-latest#az-network-nic-create) 创建两个 NIC。 以下示例创建两个连接到网络安全组的 NIC（名为 myNic1  和 myNic2  ），其中一个 NIC 连接到每个子网：

```azurecli
az network nic create \
    --resource-group myResourceGroup \
    --name myNic1 \
    --vnet-name myVnet \
    --subnet mySubnetFrontEnd \
    --network-security-group myNetworkSecurityGroup
az network nic create \
    --resource-group myResourceGroup \
    --name myNic2 \
    --vnet-name myVnet \
    --subnet mySubnetBackEnd \
    --network-security-group myNetworkSecurityGroup
```

## <a name="create-a-vm-and-attach-the-nics"></a>创建 VM 并附加 NIC
创建 VM 时，请使用 `--nics`指定所创建的 NIC。 还需要谨慎选择 VM 的大小。 可添加到 VM 的 NIC 数目有限制。 详细了解 [Linux VM 大小](sizes.md)。

使用 [az vm create](https://docs.azure.cn/cli/vm?view=azure-cli-latest#az-vm-create) 创建 VM。 以下示例创建一个名为 myVM  的 VM：

```azurecli
az vm create \
    --resource-group myResourceGroup \
    --name myVM \
    --image UbuntuLTS \
    --size Standard_DS3_v2 \
    --admin-username azureuser \
    --generate-ssh-keys \
    --nics myNic1 myNic2
```

通过完成[为多个 NIC 配置来宾 OS](#configure-guest-os-for-multiple-nics) 中的步骤，将路由表添加到来宾 OS。

## <a name="add-a-nic-to-a-vm"></a>将 NIC 添加到 VM
之前的步骤创建了具有多个 NIC 的 VM。 还可使用 Azure CLI 将 NIC 添加到现有 VM。 不同的 [VM 大小](sizes.md)支持不同数目的 NIC，因此请相应地调整 VM 的大小。 如果需要，可[调整 VM 的大小](change-vm-size.md)。

使用 [az network nic create](https://docs.azure.cn/cli/network/nic?view=azure-cli-latest#az-network-nic-create) 创建另一 NIC。 以下示例创建一个名为 myNic3  的 NIC，该 NIC 连接到后端子网和之前步骤中创建的网络安全组：

```azurecli
az network nic create \
    --resource-group myResourceGroup \
    --name myNic3 \
    --vnet-name myVnet \
    --subnet mySubnetBackEnd \
    --network-security-group myNetworkSecurityGroup
```

若要将 NIC 添加到现有 VM，请先使用 [az vm deallocate](https://docs.azure.cn/cli/vm?view=azure-cli-latest#az-vm-deallocate) 解除分配 VM。 以下示例解除分配名为 myVM  的 VM：

```azurecli
az vm deallocate --resource-group myResourceGroup --name myVM
```

使用 [az vm nic add](https://docs.azure.cn/cli/vm/nic?view=azure-cli-latest#az-vm-nic-add) 添加 NIC。 以下示例将 myNic3  添加到 myVM  ：

```azurecli
az vm nic add \
    --resource-group myResourceGroup \
    --vm-name myVM \
    --nics myNic3
```

使用 [az vm start](https://docs.azure.cn/cli/vm?view=azure-cli-latest#az-vm-start) 启动 VM：

```azurecli
az vm start --resource-group myResourceGroup --name myVM
```

通过完成[为多个 NIC 配置来宾 OS](#configure-guest-os-for-multiple-nics) 中的步骤，将路由表添加到来宾 OS。

## <a name="remove-a-nic-from-a-vm"></a>从 VM 中删除 NIC
若要从现有 VM 中删除 NIC，请先使用 [az vm deallocate](https://docs.azure.cn/cli/vm?view=azure-cli-latest#az-vm-deallocate) 解除分配 VM。 以下示例解除分配名为 myVM  的 VM：

```azurecli
az vm deallocate --resource-group myResourceGroup --name myVM
```

使用 [az vm nic remove](https://docs.azure.cn/cli/vm/nic?view=azure-cli-latest#az-vm-nic-remove) 删除 NIC。 以下示例从 myVM  中删除 myNic3  ：

```azurecli
az vm nic remove \
    --resource-group myResourceGroup \
    --vm-name myVM \
    --nics myNic3
```

使用 [az vm start](https://docs.azure.cn/cli/vm?view=azure-cli-latest#az-vm-start) 启动 VM：

```azurecli
az vm start --resource-group myResourceGroup --name myVM
```

## <a name="create-multiple-nics-using-resource-manager-templates"></a>使用 Resource Manager 模板创建多个 NIC
Azure Resource Manager 模板使用声明性 JSON 文件来定义环境。 可以阅读 [Azure Resource Manager 概述](../../azure-resource-manager/management/overview.md)。 Resource Manager 模板可让你在部署期间创建资源的多个实例，例如，创建多个 NIC。 使用 *copy* 指定要创建的实例数：

```json
"copy": {
    "name": "multiplenics"
    "count": "[parameters('count')]"
}
```

阅读有关[使用 *copy* 创建多个实例](../../resource-group-create-multiple.md)的详细信息。 

也可以使用 `copyIndex()` 并在资源名称中追加一个数字，来创建 `myNic1`、`myNic2`，等等。下面显示了追加索引值的示例：

```json
"name": "[concat('myNic', copyIndex())]", 
```

可以阅读[使用 Resource Manager 模板创建多个 NIC](../../virtual-network/template-samples.md) 的完整示例。

通过完成[为多个 NIC 配置来宾 OS](#configure-guest-os-for-multiple-nics) 中的步骤，将路由表添加到来宾 OS。

## <a name="configure-guest-os-for-multiple-nics"></a>为多个 NIC 配置来宾 OS

前面的步骤创建了虚拟网络和子网，附加了 NIC，然后创建了 VM。 尚未创建公共 IP 地址，以及允许 SSH 流量的网络安全组规则。 若要为多个 NIC 配置来宾 OS，需要允许远程连接并在 VM 本地运行命令。

若要允许 SSH 流量，请使用 [az network nsg rule create](https://docs.azure.cn/cli/network/nsg/rule?view=azure-cli-latest#az-network-nsg-rule-create) 创建网络安全组规则，如下所示：

```azurecli
az network nsg rule create \
    --resource-group myResourceGroup \
    --nsg-name myNetworkSecurityGroup \
    --name allow_ssh \
    --priority 101 \
    --destination-port-ranges 22
```

使用 [az network public-ip create](https://docs.azure.cn/cli/network/public-ip?view=azure-cli-latest#az-network-public-ip-create) 创建公共 IP 地址，并使用 [az network nic ip-config update](https://docs.azure.cn/cli/network/nic/ip-config?view=azure-cli-latest#az-network-nic-ip-config-update) 将其分配到第一个 NIC：

<!--MOONCAKE CUSTOMIZE BELOW public-ip create CORRECT-->

```azurecli
az network public-ip create --resource-group myResourceGroup --name myPublicIP

az network nic ip-config update \
    --resource-group myResourceGroup \
    --nic-name myNic1 \
    --name ipconfig1 \
    --public-ip-address myPublicIP
```

<!--MOONCAKE CUSTOMIZE ABOVE --public-ip-address CORRECT-->

若要查看 VM 的公共 IP 地址，请使用 [az vm show](https://docs.azure.cn/cli/vm?view=azure-cli-latest#az-vm-show)，如下所示：

```azurecli
az vm show --resource-group myResourceGroup --name myVM -d --query publicIps -o tsv
```

现在，通过 SSH 连接到 VM 的公共 IP 地址。 上一步骤中提供的默认用户名为 *azureuser*。 请提供自己的用户名和公共 IP 地址：

```bash
ssh azureuser@137.117.58.232
```

若要向/从辅助网络接口发送数据，必须手动将永久性路由添加到每个辅助网络接口的操作系统。 在本文中，*eth1* 是辅助接口。 有关将永久性路由添加到操作系统的说明根据分发版的不同而异。 请参阅分发版的文档来获取说明。

将路由添加到操作系统时，网络接口所在的每个子网的网关地址为 *.1*。 例如，如果为网络接口分配了地址 *10.0.2.4*，则为路由指定的网关就是 *10.0.2.1*。 如果希望接口的所有流量通过指定的网关，可为路由目标定义特定的网络，或指定目标 *0.0.0.0*。 每个子网的网关由虚拟网络管理。

添加辅助接口的路由后，使用 `route -n` 验证该路由是否包含在路由表中。 以下示例输出适用于其中的两个网络接口已添加到本文所述 VM 的路由表：

```bash
Kernel IP routing table
Destination     Gateway         Genmask         Flags Metric Ref    Use Iface
0.0.0.0         10.0.1.1        0.0.0.0         UG    0      0        0 eth0
0.0.0.0         10.0.2.1        0.0.0.0         UG    0      0        0 eth1
10.0.1.0        0.0.0.0         255.255.255.0   U     0      0        0 eth0
10.0.2.0        0.0.0.0         255.255.255.0   U     0      0        0 eth1
168.63.129.16   10.0.1.1        255.255.255.255 UGH   0      0        0 eth0
169.254.169.254 10.0.1.1        255.255.255.255 UGH   0      0        0 eth0
```

重新启动后请再次检查路由表，以确认每次重新启动后，添加的路由是否能够保留。 若要测试连接，可输入以下示例所示的命令，其中，*eth1* 是辅助网络接口的名称：

```bash
ping bing.com -c 4 -I eth1
```

## <a name="next-steps"></a>后续步骤
尝试创建具有多个 NIC 的 VM 时，请查看 [Lnux VM 大小](sizes.md)。 注意每个 VM 大小支持的 NIC 数目上限。

若要进一步保护 VM，请使用适时 VM 访问。 此功能会视需要打开针对 SSH 流量的网络安全组规则，并根据定义的时间段保持打开。 有关详细信息，请参阅[使用恰时功能管理虚拟机访问](../../security-center/security-center-just-in-time.md)。

<!--Update_Description: wording update, update meta properties -->