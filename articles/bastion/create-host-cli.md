---
title: 使用 Azure CLI 创建 Bastion 主机 | Azure Bastion
description: 本文介绍如何创建和删除 Bastion 主机
services: bastion
author: rockboyfor
ms.service: bastion
ms.topic: how-to
origin.date: 03/26/2020
ms.date: 07/27/2020
ms.testscope: yes
ms.testdate: 07/27/2020
ms.author: v-yeche
ms.openlocfilehash: fb2696286166b682fea4eea21338792d11a764b9
ms.sourcegitcommit: 4d9846bb03ac24bd98b0c9a781bb8912ff6d2f61
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/22/2020
ms.locfileid: "86926974"
---
<!--RELEASE BEFORE CONFIRMATION-->
<!--Verified NOT Passed-->
# <a name="create-an-azure-bastion-host-using-azure-cli"></a>使用 Azure CLI 创建 Azure Bastion 主机

本文介绍如何使用 Azure CLI 创建 Azure Bastion 主机。 在虚拟网络中预配 Azure Bastion 服务后，即可在该虚拟网络中的所有 VM 上获得无缝的 RDP/SSH 体验。 Azure Bastion 部署是按虚拟网络进行的，而不是按订阅/帐户或虚拟机进行的。

或者，可使用 [Azure 门户](bastion-create-host-portal.md)或 [Azure PowerShell](bastion-create-host-powershell.md) 创建 Azure Bastion 主机。

## <a name="before-you-begin"></a>准备阶段

确保拥有 Azure 订阅。 如果还没有 Azure 订阅，可以激活 [MSDN 订户权益](https://www.azure.cn/offers/ms-mc-arz-msdn/index.html)或注册[试用帐户](https://www.azure.cn/pricing/1rmb-trial)。

<!--Not Available on [!INCLUDE [cloudshell cli](../../includes/vpn-gateway-cloud-shell-cli.md)]-->

<a name="createhost"></a>
## <a name="create-a-bastion-host"></a>创建 Bastion 主机

本部分帮助你使用 Azure CLI 创建新的 Azure Bastion 资源。

1. 创建虚拟网络和 Azure Bastion 子网。 必须使用名称值 AzureBastionSubnet 创建 Azure Bastion 子网。 此值告知 Azure 要将 Bastion 资源部署到哪个子网。 这不同于网关子网。 必须使用至少为 /27 或更大（/27、/26 等）的子网。 创建不包含任何路由表或委托的 AzureBastionSubnet。 如果使用 AzureBastionSubnet 上的网络安全组，请参阅[使用 NSG](bastion-nsg.md) 一文。

   ```azurecli
   az network vnet create -g MyResourceGroup -n MyVnet --address-prefix 10.0.0.0/16 --subnet-name AzureBastionSubnet  --subnet-prefix 10.0.0.0/24
   ```

2. 为 Azure Bastion 创建一个公共 IP 地址。 此公共 IP 是将在其上访问 RDP/SSH（通过端口 443）的 Bastion 资源的公共 IP 地址。 公共 IP 地址必须与要创建的 Bastion 资源位于同一区域。

   ```azurecli
   az network public-ip create -g MyResourceGroup -n MyIp --sku Standard
   ```

3. 在虚拟网络的 AzureBastionSubnet 中创建新的 Azure Bastion 资源。 创建和部署 Bastion 资源大约需要 5 分钟。

   ```azurecli
   az network bastion create --name $name --public-ip-address $publicip --resource-group $RgName --vnet-name $VNetName --location $location

   ```

## <a name="next-steps"></a>后续步骤

* 有关其他信息，请参阅 [Bastion 常见问题解答](bastion-faq.md)。

* 若要在 Azure Bastion 子网中使用网络安全组，请参阅[使用 NSG](bastion-nsg.md)。

<!-- Update_Description: new article about create host cli -->
<!--NEW.date: 07/27/2020-->