---
title: CLI 示例 - 使用 Azure CLI 对多个网站进行负载均衡 | Microsoft Docs
description: 本 Azure CLI 脚本示例演示如何对指向同一虚拟机的多个网站进行负载均衡
services: load-balancer
documentationcenter: load-balancer
author: WenJason
manager: digimobile
editor: tysonn
tags: ''
ms.assetid: ''
ms.service: load-balancer
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: ''
ms.workload: infrastructure
origin.date: 04/20/2018
ms.date: 03/04/2019
ms.author: v-jay
ms.openlocfilehash: 6d0a4fb5729452ce18c8b887e9d69d2f08d4c13d
ms.sourcegitcommit: c1ba5a62f30ac0a3acb337fb77431de6493e6096
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2020
ms.locfileid: "63847755"
---
# <a name="azure-cli-script-example-load-balance-multiple-websites"></a>Azure CLI 脚本示例：对多个网站进行负载均衡

本 Azure CLI 脚本示例会使用可用性集中的两个虚拟机 (VM) 创建虚拟网络。 负载均衡器会将两个单独 IP 地址的流量定向到两台 VM。 运行脚本后，可将 Web 服务器软件部署到 VM 上，并可承载多个网站，其中每个网站都有其自身的 IP 地址。

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>示例脚本

```azurecli
#!/bin/bash

RgName="MyResourceGroup"
Location="chinaeast"

# Create a resource group.
az group create \
  --name $RgName \
  --location $Location

# Create an availability set for the two VMs that host both websites.
az vm availability-set create \
  --resource-group $RgName \
  --location $Location \
  --name MyAvailabilitySet \
  --platform-fault-domain-count 2 \
  --platform-update-domain-count 2

# Create a virtual network and a subnet.
az network vnet create \
  --resource-group $RgName \
  --name MyVnet \
  --address-prefix 10.0.0.0/16 \
  --location $Location \
  --subnet-name MySubnet \
  --subnet-prefix 10.0.0.0/24

# Create three public IP addresses; one for the load balancer and two for the front-end IP configurations.
az network public-ip create \
  --resource-group $RgName \
  --name MyPublicIp-LoadBalancer \
  --allocation-method Dynamic
az network public-ip create \
  --resource-group $RgName \
  --name MyPublicIp-Contoso \
  --allocation-method Dynamic
az network public-ip create \
  --resource-group $RgName \
  --name MyPublicIp-Fabrikam \
  --allocation-method Dynamic

# Create a load balancer.
az network lb create \
  --resource-group $RgName \
  --location $Location \
  --name MyLoadBalancer \
  --frontend-ip-name FrontEnd \
  --backend-pool-name BackEnd \
  --public-ip-address MyPublicIp-LoadBalancer

# Create two front-end IP configurations for both web sites.
az network lb frontend-ip create \
  --resource-group $RgName \
  --lb-name MyLoadBalancer \
  --public-ip-address MyPublicIp-Contoso \
  --name FeContoso
az network lb frontend-ip create \
  --resource-group $RgName \
  --lb-name MyLoadBalancer \
  --public-ip-address MyPublicIp-Fabrikam \
  --name FeFabrikam

# Create the back-end address pools.
az network lb address-pool create \
  --resource-group $RgName \
  --lb-name MyLoadBalancer \
  --name BeContoso
az network lb address-pool create \
  --resource-group $RgName \
  --lb-name MyLoadBalancer \
  --name BeFabrikam

# Create a probe on port 80.
az network lb probe create \
  --resource-group $RgName \
  --lb-name MyLoadBalancer \
  --name MyProbe \
  --protocol Http \
  --port 80 --path /

# Create the load balancing rules.
az network lb rule create \
  --resource-group $RgName \
  --lb-name MyLoadBalancer \
  --name LBRuleContoso \
  --protocol Tcp \
  --probe-name MyProbe \
  --frontend-port 5000 \
  --backend-port 5000 \
  --frontend-ip-name FeContoso \
  --backend-pool-name BeContoso
az network lb rule create \
  --resource-group $RgName \
  --lb-name MyLoadBalancer \
  --name LBRuleFabrikam \
  --protocol Tcp \
  --probe-name MyProbe \
  --frontend-port 5000 \
  --backend-port 5000 \
  --frontend-ip-name FeFabrikam \
  --backend-pool-name BeFabrikam

# ############## VM1 ###############

# Create an Public IP for the first VM.
az network public-ip create \
  --resource-group $RgName \
  --name MyPublicIp-Vm1 \
  --allocation-method Dynamic

# Create a network interface for VM1.
az network nic create \
  --resource-group $RgName \
  --vnet-name MyVnet \
  --subnet MySubnet \
  --name MyNic-Vm1 \
  --public-ip-address MyPublicIp-Vm1

# Create IP configurations for Contoso and Fabrikam.
az network nic ip-config create \
  --resource-group $RgName \
  --name ipconfig2 \
  --nic-name MyNic-Vm1 \
  --lb-name MyLoadBalancer \
  --lb-address-pools BeContoso
az network nic ip-config create \
  --resource-group $RgName \
  --name ipconfig3 \
  --nic-name MyNic-Vm1 \
  --lb-name MyLoadBalancer \
  --lb-address-pools BeFabrikam

# Create Vm1.
az vm create \
  --resource-group $RgName \
  --name MyVm1 \
  --nics MyNic-Vm1 \
  --image UbuntuLTS \
  --availability-set MyAvailabilitySet \
  --admin-username azureadmin \
  --generate-ssh-keys

############### VM2 ###############

# Create an Public IP for the second VM.
az network public-ip create \
  --resource-group $RgName \
  --name MyPublicIp-Vm2 \
  --allocation-method Dynamic

# Create a network interface for VM2.
az network nic create \
  --resource-group $RgName \
  --vnet-name MyVnet \
  --subnet MySubnet \
  --name MyNic-Vm2 \
  --public-ip-address MyPublicIp-Vm2

# Create IP-Configs for Contoso and Fabrikam.
az network nic ip-config create \
  --resource-group $RgName \
  --name ipconfig2 \
  --nic-name MyNic-Vm2 \
  --lb-name MyLoadBalancer \
  --lb-address-pools BeContoso
az network nic ip-config create \
  --resource-group $RgName \
  --name ipconfig3 \
  --nic-name MyNic-Vm2 \
  --lb-name MyLoadBalancer \
  --lb-address-pools BeFabrikam

# Create Vm2.
az vm create \
  --resource-group $RgName \
  --name MyVm2 \
  --nics MyNic-Vm2 \
  --image UbuntuLTS \
  --availability-set MyAvailabilitySet \
  --admin-username azureadmin \
  --generate-ssh-keys

```

## <a name="clean-up-deployment"></a>清理部署 

运行以下命令来删除资源组、VM 和所有相关资源。

```azurecli
az group delete --name myResourceGroup --yes
```

## <a name="script-explanation"></a>脚本说明

此脚本使用以下命令创建资源组、虚拟网络、负载均衡器和所有相关资源。 表中的每条命令均链接到特定于命令的文档。

| Command | 说明 |
|---|---|
| [az group create](/cli/group#az-group-create) | 创建用于存储所有资源的资源组。 |
| [az network vnet create](/cli/network/vnet#az-network-vnet-create) | 创建 Azure 虚拟网络和子网。 |
| [az network public-ip create](/cli/network/public-ip#az-network-public-ip-create) | 使用静态 IP 地址和关联的 DNS 名称创建公共 IP 地址。 |
| [az network lb create](/cli/network/lb#az-network-lb-create) | 创建 Azure 负载均衡器。 |
| [az network lb probe create](/cli/network/lb/probe#az-network-lb-probe-create) | 创建负载均衡器探测。 负载均衡器探测用于监视负载均衡器集中的每个 VM。 如果任何 VM 无法访问，流量将不会路由到该 VM。 |
| [az network lb rule create](/cli/network/lb/rule#az-network-lb-rule-create) | 创建负载均衡器规则。 在此示例中，将为端口 80 创建一个规则。 当 HTTP 流量到达负载均衡器时，它会路由到负载均衡器集中某个 VM 的端口 80。 |
| [az network lb frontend-ip create](/cli/network/lb/frontend-ip#az-network-lb-frontend-ip-create) | 为负载均衡器创建前端 IP 地址。 |
| [az network lb address-pool create](/cli/network/lb/address-pool#az-network-lb-address-pool-create) | 创建后端地址池。 |
| [az network nic create](/cli/network/nic#az-network-nic-create) | 创建虚拟网卡并将其连接到虚拟网络和子网。 |
| [az vm availability-set create](/cli/network/lb/rule#az-network-lb-rule-create) | 创建可用性集。 可用性集通过将虚拟机分布到各个物理资源上（以便发生故障时，不会影响整个集）来确保应用程序运行时间。 |
| [az network nic ip-config create](/cli/network/nic/ip-config#az-network-nic-ip-config-create) | 创建 IP 配置。 必须为订阅启用 Microsoft.Network/AllowMultipleIpConfigurationsPerNic 功能。 对于每个 NIC，仅能使用 --make-primary 标志将一个配置指定为主要 IP 配置。 |
| [az vm create](/cli/vm/availability-set#az-vm-availability-set-create) | 创建虚拟机并将其连接到网卡、虚拟网络、子网和 NSG。 此命令还指定要使用的虚拟机映像和管理凭据。  |
| [az group delete](/cli/vm/extension#az-vm-extension-set) | 删除资源组，包括所有嵌套的资源。 |

## <a name="next-steps"></a>后续步骤

有关 Azure CLI 的详细信息，请参阅 [Azure CLI 文档](https://docs.azure.cn/zh-cn/cli)。

可在 [Azure 网络概述文档](../cli-samples.md?toc=%2fnetworking%2ftoc.json)中找到其他网络 CLI 脚本示例。
<!-- Update_Description: new articles on load balancer linux clis load balancer with multiple websits vm  -->
<!--ms.date: 06/18/2018-->