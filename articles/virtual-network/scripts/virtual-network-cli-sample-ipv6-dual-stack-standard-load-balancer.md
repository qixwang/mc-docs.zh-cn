---
title: Azure CLI 脚本示例 - 配置 IPv6 前端 - 标准负载均衡器
titlesuffix: Azure Virtual Network
description: 使用 Azure CLI 在 Azure 虚拟网络中启用 IPv6 终结点
services: virtual-network
documentationcenter: na
author: rockboyfor
manager: digimobile
ms.service: virtual-network
ms.devlang: NA
ms.topic: article
ms.workload: infrastructure-services
ms.date: 04/13/2020
ms.author: v-yeche
ms.openlocfilehash: 252c59b5e185272a36af1b911a353c40432092a9
ms.sourcegitcommit: 564739de7e63e19a172122856ebf1f2f7fb4bd2e
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/23/2020
ms.locfileid: "82093272"
---
# <a name="configure-ipv6-endpoints-in-virtual-network-script-sample-using-standard-load-balancer"></a>使用标准负载均衡器在虚拟网络中配置 IPv6 终结点的脚本示例

<!--Not Available on preview-->

本文介绍如何在 Azure 中部署一个双堆栈 (IPv4 + IPv6) 应用程序，其中包含具有双堆栈子网的双堆栈虚拟网络、采用双重 (IPv4 + IPv6) 前端配置的标准负载均衡器、具有采用双重 IP 配置的 NIC 的 VM、双重网络安全组规则，以及双重公共 IP。

可以通过本地 Azure CLI 安装来执行脚本。 如果在本地使用 CLI，此脚本要求运行版本 2.0.28 或更高版本。 要查找已安装的版本，请运行 `az --version`。 如需进行安装或升级，请参阅[安装 Azure CLI](https://docs.azure.cn/cli/install-azure-cli?view=azure-cli-latest)。 如果在本地运行 CLI，则还需运行 `az login` 以创建与 Azure 的连接。

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

<!--Not Avaialble on ## Prerequisites-->

[!INCLUDE [azure-cli-2-azurechinacloud-environment-parameter](../../../includes/azure-cli-2-azurechinacloud-environment-parameter.md)]

## <a name="sample-script"></a>示例脚本

```azurecli
# Create a resource group
az group create \
--name DsResourceGroup01 \
--location chinaeast

# Create an IPV4 IP address
az network public-ip create \
--name dsPublicIP_v4  \
--resource-group DsResourceGroup01  \
--location chinaeast  \
--sku STANDARD  \
--allocation-method static  \
--version IPv4

# Create an IPV6 IP address
az network public-ip create \
--name dsPublicIP_v6  \
--resource-group DsResourceGroup01  \
--location chinaeast \
--sku STANDARD  \
--allocation-method static  \
--version IPv6

# Create public IP addresses for remote access to VMs
az network public-ip create \
--name dsVM0_remote_access  \
--resource-group DsResourceGroup01 \
--location chinaeast  \
--sku Standard  \
--allocation-method static  \
--version IPv4

az network public-ip create \
--name dsVM1_remote_access  \
--resource-group DsResourceGroup01  \
--location chinaeast  \
--sku Standard  \
--allocation-method static  \
--version IPv4

# Create load balancer

az network lb create \
--name dsLB  \
--resource-group DsResourceGroup01 \
--sku Standard \
--location chinaeast \
--frontend-ip-name dsLbFrontEnd_v4  \
--public-ip-address dsPublicIP_v4  \
--backend-pool-name dsLbBackEndPool_v4

# Create IPv6 front-end
az network lb frontend-ip create \
--lb-name dsLB  \
--name dsLbFrontEnd_v6  \
--resource-group DsResourceGroup01  \
--public-ip-address dsPublicIP_v6

# Configure IPv6 back-end address pool
az network lb address-pool create \
--lb-name dsLB  \
--name dsLbBackEndPool_v6  \
--resource-group DsResourceGroup01

# Create a load balancer rule

az network lb rule create \
--lb-name dsLB  \
--name dsLBrule_v4  \
--resource-group DsResourceGroup01  \
--frontend-ip-name dsLbFrontEnd_v4  \
--protocol Tcp  \
--frontend-port 80  \
--backend-port 80  \
--backend-pool-name dsLbBackEndPool_v4

az network lb rule create \
--lb-name dsLB  \
--name dsLBrule_v6  \
--resource-group DsResourceGroup01 \
--frontend-ip-name dsLbFrontEnd_v6  \
--protocol Tcp  \
--frontend-port 80 \
--backend-port 80  \
--backend-pool-name dsLbBackEndPool_v6

# Create an availability set
az vm availability-set create \
--name dsAVset  \
--resource-group DsResourceGroup01  \
--location chinaeast \
--platform-fault-domain-count 2  \
--platform-update-domain-count 2  

# Create network security group

az network nsg create \
--name dsNSG1  \
--resource-group DsResourceGroup01  \
--location chinaeast

# Create inbound rule for port 3389
az network nsg rule create \
--name allowRdpIn  \
--nsg-name dsNSG1  \
--resource-group DsResourceGroup01  \
--priority 100  \
--description "Allow Remote Desktop In"  \
--access Allow  \
--protocol "*"  \
--direction Inbound  \
--source-address-prefixes "*"  \
--source-port-ranges "*"  \
--destination-address-prefixes "*"  \
--destination-port-ranges 3389

# Create inbound rule for port 80
az network nsg rule create \
--name allowHTTPIn  \
--nsg-name dsNSG1  \
--resource-group DsResourceGroup01  \
--priority 200  \
--description "Allow HTTP In"  \
--access Allow  \
--protocol "*"  \
--direction Inbound  \
--source-address-prefixes "*"  \
--source-port-ranges 80  \
--destination-address-prefixes "*"  \
--destination-port-ranges 80

# Create outbound rule

az network nsg rule create \
--name allowAllOut  \
--nsg-name dsNSG1  \
--resource-group DsResourceGroup01  \
--priority 300  \
--description "Allow All Out"  \
--access Allow  \
--protocol "*"  \
--direction Outbound  \
--source-address-prefixes "*"  \
--source-port-ranges "*"  \
--destination-address-prefixes "*"  \
--destination-port-ranges "*"

# Create the virtual network
az network vnet create \
--name dsVNET \
--resource-group DsResourceGroup01 \
--location chinaeast  \
--address-prefixes "10.0.0.0/16" "ace:cab:deca::/48"

# Create a single dual stack subnet

az network vnet subnet create \
--name dsSubNET \
--resource-group DsResourceGroup01 \
--vnet-name dsVNET \
--address-prefixes "10.0.0.0/24" "ace:cab:deca:deed::/64" \
--network-security-group dsNSG1

# Create NICs
az network nic create \
--name dsNIC0  \
--resource-group DsResourceGroup01 \
--network-security-group dsNSG1  \
--vnet-name dsVNET  \
--subnet dsSubNet  \
--private-ip-address-version IPv4 \
--lb-address-pools dsLbBackEndPool_v4  \
--lb-name dsLB  \
--public-ip-address dsVM0_remote_access

az network nic create \
--name dsNIC1 \
--resource-group DsResourceGroup01 \
--network-security-group dsNSG1 \
--vnet-name dsVNET \
--subnet dsSubNet \
--private-ip-address-version IPv4 \
--lb-address-pools dsLbBackEndPool_v4 \
--lb-name dsLB \
--public-ip-address dsVM1_remote_access

# Create IPV6 configurations for each NIC

az network nic ip-config create \
--name dsIp6Config_NIC0  \
--nic-name dsNIC0  \
--resource-group DsResourceGroup01 \
--vnet-name dsVNET \
--subnet dsSubNet \
--private-ip-address-version IPv6 \
--lb-address-pools dsLbBackEndPool_v6 \
--lb-name dsLB

az network nic ip-config create \
--name dsIp6Config_NIC1 \
--nic-name dsNIC1 \
--resource-group DsResourceGroup01 \
--vnet-name dsVNET \
--subnet dsSubNet \
--private-ip-address-version IPv6 \
--lb-address-pools dsLbBackEndPool_v6 \
--lb-name dsLB

# Create virtual machine dsVM0

 az vm create \
--name dsVM0 \
--resource-group DsResourceGroup01 \
--nics dsNIC0 \
--size Standard_A2 \
--availability-set dsAVset \
--image MicrosoftWindowsServer:WindowsServer:2019-Datacenter:latest  

# Create virtual machine dsVM1

az vm create \
--name dsVM1 \
--resource-group DsResourceGroup01 \
--nics dsNIC1 \
--size Standard_A2 \
--availability-set dsAVset \
--image MicrosoftWindowsServer:WindowsServer:2019-Datacenter:latest 
```
## <a name="view-ipv6-dual-stack-virtual-network-in-azure-portal"></a>在 Azure 门户中查看 IPv6 双堆栈虚拟网络
可以在 Azure 门户中查看 IPv6 双堆栈虚拟网络，如下所示：
1. 在门户的搜索栏中输入 *dsVnet*。
2. 当“myVirtualNetwork”出现在搜索结果中时，将其选中。  此时会启动名为 *dsVnet* 的双堆栈虚拟网络的“概述”页。  该双堆栈虚拟网络显示了位于 *dsSubnet* 双堆栈子网中的两个 NIC，这些 NIC 采用 IPv4 和 IPv6 配置。 

> [!NOTE]
> 当前版本的 Azure 虚拟网络 IPv6 在 Azure 门户中以只读的形式提供。

<!--Not Available on preview-->

## <a name="clean-up-deployment"></a>清理部署

运行以下命令来删除资源组、VM 和所有相关资源：

```azurecli
az group delete --name <resourcegroupname> --yes
```

## <a name="script-explanation"></a>脚本说明

此脚本使用以下命令创建资源组、虚拟机、可用性集、负载均衡器和所有相关资源。 表中的每条命令均链接到特定于命令的文档。

| 命令 | 注释 |
|---|---|
| [az group create](https://docs.azure.cn/cli/group?view=azure-cli-latest#az-group-create) | 创建用于存储所有资源的资源组。 |
| [az network vnet create](https://docs.azure.cn/cli/network/vnet?view=azure-cli-latest#az-network-vnet-create) | 创建 Azure 虚拟网络和子网。 |
| [az network public-ip create](https://docs.azure.cn/cli/network/public-ip?view=azure-cli-latest#az-network-public-ip-create) | 使用静态 IP 地址和关联的 DNS 名称创建公共 IP 地址。 |
| [az network lb create](https://docs.azure.cn/cli/network/lb?view=azure-cli-latest#az-network-lb-create) | 创建 Azure 负载均衡器。 |
| [az network lb probe create](https://docs.azure.cn/cli/network/lb/probe?view=azure-cli-latest#az-network-lb-probe-create) | 创建负载均衡器探测。 负载均衡器探测用于监视负载均衡器集中的每个 VM。 如果任何 VM 无法访问，流量不会路由到该 VM。 |
| [az network lb rule create](https://docs.azure.cn/cli/network/lb/rule?view=azure-cli-latest#az-network-lb-rule-create) | 创建负载均衡器规则。 在此示例中，为端口 80 创建一个规则。 当 HTTP 流量到达负载均衡器时，它会路由到 LB 集中某个 VM 的端口 80。 |
| [az network lb inbound-nat-rule create](https://docs.azure.cn/cli/network/lb/inbound-nat-rule?view=azure-cli-latest#az-network-lb-inbound-nat-rule-create) | 创建负载均衡器网络地址转换 (NAT) 规则。  NAT 规则将负载均衡器的端口映射到 VM 上的端口。 在本示例中，将为发往负载均衡器集中的每个 VM 的 SSH 流量创建 NAT 规则。  |
| [az network nsg create](https://docs.azure.cn/cli/network/nsg?view=azure-cli-latest#az-network-nsg-create) | 创建网络安全组 (NSG)，这是 Internet 和虚拟机之间的安全边界。 |
| [az network nsg rule create](https://docs.azure.cn/cli/network/nsg/rule?view=azure-cli-latest#az-network-nsg-rule-create) | 创建 NSG 规则以允许入站流量。 在此示例中，为 SSH 流量打开端口 22。 |
| [az network nic create](https://docs.azure.cn/cli/network/nic?view=azure-cli-latest#az-network-nic-create) | 创建虚拟网卡并将其连接到虚拟网络、子网和 NSG。 |
| [az vm availability-set create](https://docs.azure.cn/cli/network/lb/rule?view=azure-cli-latest#az-network-lb-rule-create) | 创建可用性集。 可用性集通过将虚拟机分布到各个物理资源上（以便发生故障时，不会影响整个集）来确保应用程序运行时间。 |
| [az vm create](https://docs.azure.cn/cli/vm?view=azure-cli-latest#az-vm-create) | 创建虚拟机并将其连接到网卡、虚拟网络、子网和 NSG。 此命令还指定要使用的虚拟机映像和管理凭据。  |
| [az group delete](https://docs.azure.cn/cli/vm/extension?view=azure-cli-latest#az-vm-extension-set) | 删除资源组，包括所有嵌套的资源。 |

## <a name="next-steps"></a>后续步骤

有关 Azure CLI 的详细信息，请参阅 [Azure CLI 文档](https://docs.azure.cn/cli/index?view=azure-cli-latest)。

可在 [Azure 网络文档](../cli-samples.md)中找到其他 Azure 网络 CLI 脚本示例。

<!-- Update_Description: new articles of virtual network cli sample ipv6 dual stack standard load balancer -->