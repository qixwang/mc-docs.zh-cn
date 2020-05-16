---
title: 在 Azure Kubernetes 服务 (AKS) 中自定义用户定义的路由 (UDR)
description: 了解如何在 Azure Kubernetes 服务 (AKS) 中自定义出口路由
services: container-service
ms.topic: article
origin.date: 03/16/2020
ms.date: 05/06/2020
ms.author: v-yeche
ms.openlocfilehash: ada700c6bb8cddcb3b9fc8f23f5a1956d17e8a08
ms.sourcegitcommit: 81241aa44adbcac0764e2b5eb865b96ae56da6b7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/09/2020
ms.locfileid: "83002212"
---
# <a name="customize-cluster-egress-with-a-user-defined-route-preview"></a>使用用户定义的路由自定义群集出口（预览版）

可以根据具体的方案自定义 AKS 群集的出口。 默认情况下，AKS 将预配一个可设置并用于出口的标准 SKU 负载均衡器。 但是，如果禁用了公共 IP 或者出口需要额外的跃点，则默认设置可能不能满足所有方案的要求。

本文介绍了如何自定义群集的出口路由以支持自定义网络方案，例如，禁用公共 IP 并要求群集位于网络虚拟设备 (NVA) 后面。

> [!IMPORTANT]
> AKS 预览版功能是自助服务，根据用户的选择提供。 预览版按原样提供，并且仅在可用情况下提供，不享受服务级别协议 (SLA) 和有限担保。   AKS 预览版由客户支持尽力提供部分支持。  因此，这些功能不适用于生产用途。 有关详细信息，请参阅以下支持文章：
>
> * [AKS 支持策略](support-policies.md)
> * [Azure 支持常见问题](faq.md)

## <a name="prerequisites"></a>先决条件
* Azure CLI 2.0.81 或更高版本
* Azure CLI 预览版扩展 0.4.28 或更高版本
* API `2020-01-01` 或更高版本

## <a name="install-the-latest-azure-cli-aks-preview-extension"></a>安装最新的 Azure CLI AKS 预览版扩展
若要设置群集的出站类型，需要安装 Azure CLI AKS 预览版扩展 0.4.18 或更高版本。 使用 az extension add 命令安装 Azure CLI AKS 预览版扩展，然后使用以下 az extension update 命令检查是否有任何可用更新：

```azure-cli
# Install the aks-preview extension
az extension add --name aks-preview

# Update the extension to make sure you have the latest version installed
az extension update --name aks-preview
```

## <a name="limitations"></a>限制
* 在预览版期间，只能在创建群集时定义 `outboundType`，以后无法对其进行更新。
* 在预览版期间，`outboundType` AKS 群集应使用 Azure CNI。 Kubenet 是可配置的，若要使用它，需要手动将路由表关联到 AKS 子网。
* 设置 `outboundType` 需要将 AKS 群集的 `vm-set-type` 指定为 `VirtualMachineScaleSets`，将 `load-balancer-sku` 指定为 `Standard`。
* 将 `outboundType` 设置为 `UDR` 值需要用户定义的路由以及群集的有效出站连接。
* 将 `outboundType` 设置为 `UDR` 值意味着，路由到负载均衡器的入口源 IP 可能与群集的传出出口目标地址**不匹配**。

## <a name="overview-of-outbound-types-in-aks"></a>AKS 中的出站类型概述

可以使用类型为“负载均衡器”或“用户定义的路由”的唯一 `outboundType` 来自定义 AKS 群集。

> [!IMPORTANT]
> 出站类型仅影响群集的出口流量。 有关详细信息，请参阅[设置入口控制器](ingress-basic.md)。

### <a name="outbound-type-of-loadbalancer"></a>loadBalancer 的出站类型

如果设置了 `loadBalancer`，AKS 将自动完成以下设置。 对于经过 AKS 分配的公共 IP 的传出流量，将使用负载均衡器。 出站类型 `loadBalancer` 支持 `loadBalancer` 类型的 Kubernetes 服务，此类型的服务应收到由 AKS 资源提供程序创建的负载均衡器传出的流量。

以下设置由 AKS 完成。
   * 为群集出口预配一个公共 IP 地址。
   * 将该公共 IP 地址分配给负载均衡器资源。
   * 为群集中的代理节点设置负载均衡器的后端池。

下面是默认情况下在 AKS 群集中部署的网络拓扑，该拓扑使用类型为 `loadBalancer` 的 `outboundType`。

![outboundtype-lb](media/egress-outboundtype/outboundtype-lb.png)

### <a name="outbound-type-of-userdefinedrouting"></a>userDefinedRouting 的出站类型

> [!NOTE]
> 使用出站类型是一种高级网络方案，需要正确配置网络。

如果设置了 `userDefinedRouting`，则 AKS 不会自动配置出口路径。 以下设置预期由**用户**完成。

必须将群集部署到具有已配置的子网的现有虚拟网络。 具有出站连接的子网中必须存在有效的用户定义路由 (UDR)。

AKS 资源提供程序将部署一个标准负载均衡器 (SLB)。 不会为负载均衡器配置任何规则，且[在实施规则之前，负载均衡器不会产生费用](https://www.azure.cn/pricing/details/load-balancer/)。 AKS **不会**自动为 SLB 前端预配公共 IP 地址。 AKS **不会**自动配置负载均衡器后端池。

## <a name="deploy-a-cluster-with-outbound-type-of-udr-and-azure-firewall"></a>部署出站类型为 UDR 且具有 Azure 防火墙的群集

若要演示如何应用出站类型为用户定义的路由的群集，可以在通过 Azure 防火墙对等互连的虚拟网络中配置一个群集。

![锁定的拓扑](media/egress-outboundtype/outboundtype-udr.png)

* 强制入口流量流经防火墙筛选器
   * 一个隔离的子网包含用于路由到代理节点的内部负载均衡器
   * 代理节点隔离在专用子网中
* 出站请求从代理节点启动并使用用户定义的路由发送到 Azure 防火墙内部 IP
   * 来自 AKS 代理节点的请求遵循 AKS 群集所部署到的子网中已放置的 UDR。
   * Azure 防火墙通过公共 IP 前端将流量传出虚拟网络
   * 对 AKS 控制平面的访问由已启用防火墙前端 IP 地址的 NSG 提供保护
   * 对公共 Internet 或其他 Azure 服务的访问流量会流入和流出防火墙前端 IP 地址

### <a name="set-configuration-via-environment-variables"></a>通过环境变量设置配置

定义创建资源时要使用的一组环境变量。

```bash
PREFIX="contosofin"
RG="${PREFIX}-rg"
LOC="chinaeast2"
NAME="${PREFIX}outboundudr"
AKS_NAME="${PREFIX}aks"
VNET_NAME="${PREFIX}vnet"
AKSSUBNET_NAME="${PREFIX}akssubnet"
SVCSUBNET_NAME="${PREFIX}svcsubnet"
# DO NOT CHANGE FWSUBNET_NAME - This is currently a requirement for Azure Firewall.
FWSUBNET_NAME="AzureFirewallSubnet"
FWNAME="${PREFIX}fw"
FWPUBLICIP_NAME="${PREFIX}fwpublicip"
FWIPCONFIG_NAME="${PREFIX}fwconfig"
FWROUTE_TABLE_NAME="${PREFIX}fwrt"
FWROUTE_NAME="${PREFIX}fwrn"
FWROUTE_NAME_INTERNET="${PREFIX}fwinternet"
DEVSUBNET_NAME="${PREFIX}dev"
```

接下来，设置订阅 ID。

```azure-cli
# Get ARM Access Token and Subscription ID - This will be used for AuthN later.

ACCESS_TOKEN=$(az account get-access-token -o tsv --query 'accessToken')

# NOTE: Update Subscription Name
# Set Default Azure Subscription to be Used via Subscription ID

az account set -s <SUBSCRIPTION_ID_GOES_HERE>

# NOTE: Update Subscription Name for setting SUBID

SUBID=$(az account show -s '<SUBSCRIPTION_NAME_GOES_HERE>' -o tsv --query 'id')
```

## <a name="create-a-virtual-network-with-multiple-subnets"></a>创建包含多个子网的虚拟网络

预配包含三个单独子网的虚拟网络，其中一个子网用于群集，一个子网用于防火墙，一个子网用于服务入口。

![空网络拓扑](media/egress-outboundtype/empty-network.png)

创建一个资源组来存放所有资源。

```azure-cli
# Create Resource Group

az group create --name $RG --location $LOC
```

创建两个虚拟网络来托管 AKS 群集和 Azure 防火墙。 每个虚拟网络都具有自己的子网。 让我们从 AKS 网络开始。

```
# Dedicated virtual network with AKS subnet

az network vnet create \
    --resource-group $RG \
    --name $VNET_NAME \
    --address-prefixes 100.64.0.0/16 \
    --subnet-name $AKSSUBNET_NAME \
    --subnet-prefix 100.64.1.0/24

# Dedicated subnet for K8s services

az network vnet subnet create \
    --resource-group $RG \
    --vnet-name $VNET_NAME \
    --name $SVCSUBNET_NAME \
    --address-prefix 100.64.2.0/24

# Dedicated subnet for Azure Firewall (Firewall name cannot be changed)

az network vnet subnet create \
    --resource-group $RG \
    --vnet-name $VNET_NAME \
    --name $FWSUBNET_NAME \
    --address-prefix 100.64.3.0/24
```

## <a name="create-and-setup-an-azure-firewall-with-a-udr"></a>创建并设置具有 UDR 的 Azure 防火墙

必须配置 Azure 防火墙入站和出站规则。 防火墙的主要用途是使组织能够针对传入和传出 AKS 群集的流量设置精细的规则。

![防火墙和 UDR](media/egress-outboundtype/firewall-udr.png)

创建将用作 Azure 防火墙前端地址的标准 SKU 公共 IP 资源。

```azure-cli
az network public-ip create -g $RG -n $FWPUBLICIP_NAME -l $LOC --sku "Standard"
```

注册预览版 CLI 扩展以创建 Azure 防火墙。
```azure-cli
# Install Azure Firewall preview CLI extension

az extension add --name azure-firewall

# Deploy Azure Firewall

az network firewall create -g $RG -n $FWNAME -l $LOC
```

现在，可将前面创建的 IP 地址分配到防火墙前端。
> [!NOTE]
> 设置 Azure 防火墙的公共 IP 地址可能需要几分钟时间。
> 
> 如果以下命令反复出现错误，请删除现有的防火墙和公共 IP，同时通过门户预配公共 IP 和 Azure 防火墙。

```azure-cli
# Configure Firewall IP Config

az network firewall ip-config create -g $RG -f $FWNAME -n $FWIPCONFIG_NAME --public-ip-address $FWPUBLICIP_NAME --vnet-name $VNET_NAME
```

当前面的命令成功时，保存防火墙前端 IP 地址以便稍后进行配置。

```bash
# Capture Firewall IP Address for Later Use

FWPUBLIC_IP=$(az network public-ip show -g $RG -n $FWPUBLICIP_NAME --query "ipAddress" -o tsv)
FWPRIVATE_IP=$(az network firewall show -g $RG -n $FWNAME --query "ipConfigurations[0].privateIpAddress" -o tsv)
```

### <a name="create-a-udr-with-a-hop-to-azure-firewall"></a>创建包含 Azure 防火墙跃点的 UDR

Azure 自动在 Azure 子网、虚拟网络与本地网络之间路由流量。 若要更改 Azure 的任何默认路由，可以创建一个路由表。

创建一个要与给定子网关联的空路由表。 该路由表将下一跃点定义为前面创建的 Azure 防火墙。 每个子网可以有一个与之关联的路由表，也可以没有。

```azure-cli
# Create UDR and add a route for Azure Firewall

az network route-table create -g $RG --name $FWROUTE_TABLE_NAME
az network route-table route create -g $RG --name $FWROUTE_NAME --route-table-name $FWROUTE_TABLE_NAME --address-prefix 0.0.0.0/0 --next-hop-type VirtualAppliance --next-hop-ip-address $FWPRIVATE_IP --subscription $SUBID
az network route-table route create -g $RG --name $FWROUTE_NAME_INTERNET --route-table-name $FWROUTE_TABLE_NAME --address-prefix $FWPUBLIC_IP/32 --next-hop-type Internet
```

请参阅[虚拟网络路由表文档](../virtual-network/virtual-networks-udr-overview.md#user-defined)，了解如何替代 Azure 的默认系统路由或者在子网的路由表中添加更多路由。

## <a name="adding-network-firewall-rules"></a>添加网络防火墙规则

> [!WARNING]
> 以下示例展示了如何添加防火墙规则。 应用程序防火墙规则必须启用[所需的出口终结点](egress.md)中定义的所有出口终结点，AKS 群集才能正常运行。 如果未启用这些终结点，则群集无法正常运行。

下面是网络和应用程序规则的示例。 我们添加了一个允许任何协议、源地址、目标地址和目标端口的网络规则。 我们还为 AKS 所需的**某些**终结点添加了应用程序规则。

在生产方案中，只应启用对应用程序所需终结点以及 [AKS 所需出口](egress.md)中定义的终结点的访问。

```
# Add Network FW Rules

az network firewall network-rule create -g $RG -f $FWNAME --collection-name 'aksfwnr' -n 'netrules' --protocols 'Any' --source-addresses '*' --destination-addresses '*' --destination-ports '*' --action allow --priority 100

# Add Application FW Rules
# IMPORTANT: Add AKS required egress endpoints

az network firewall application-rule create -g $RG -f $FWNAME \
    --collection-name 'AKS_Global_Required' \
    --action allow \
    --priority 100 \
    -n 'required' \
    --source-addresses '*' \
    --protocols 'http=80' 'https=443' \
    --target-fqdns \
        'aksrepos.azurecr.cn' \
        '*blob.core.chinacloudapi.cn' \
        'mcr.microsoft.com' \
        '*cdn.mscr.io' \
        '*.data.mcr.microsoft.com' \
        'management.chinacloudapi.cn' \
        'login.chinacloudapi.cn' \
        'ntp.ubuntu.com' \
        'packages.microsoft.com' \
        'acs-mirror.azureedge.net'
```

请参阅 [Azure 防火墙文档](/firewall/overview)来详细了解 Azure 防火墙服务。

## <a name="associate-the-route-table-to-aks"></a>将路由表关联到 AKS

若要将群集与防火墙相关联，群集子网的专用子网必须引用前面创建的路由表。 可以通过向包含群集和防火墙的虚拟网络发出更新群集子网路由表的命令来执行关联。

```azure-cli
# Associate route table with next hop to Firewall to the AKS subnet

az network vnet subnet update -g $RG --vnet-name $VNET_NAME --name $AKSSUBNET_NAME --route-table $FWROUTE_TABLE_NAME
```

## <a name="deploy-aks-with-outbound-type-of-udr-to-the-existing-network"></a>将出站类型为 UDR 的 AKS 部署到现有网络

现在，可将 AKS 群集部署到现有的虚拟网络设置中。 若要将群集出站类型设置为用户定义的路由，必须向 AKS 提供一个现有子网。

![aks-deploy](media/egress-outboundtype/outboundtype-udr.png)

### <a name="create-a-service-principal-with-access-to-provision-inside-the-existing-virtual-network"></a>创建有权在现有虚拟网络中进行预配的服务主体

AKS 使用服务主体来创建群集资源。 创建时传递的服务主体用于创建底层 AKS 资源，例如 AKS 使用的 VM、存储和负载均衡器。 如果授予的权限太少，将无法预配 AKS 群集。

```azure-cli
# Create SP and Assign Permission to Virtual Network

az ad sp create-for-rbac -n "${PREFIX}sp" --skip-assignment
```

现在，请将下面的 `APPID` 和 `PASSWORD` 替换为前一命令输出自动生成的服务主体 appid 和服务主体密码。 我们将引用 VNET 资源 ID 来向服务主体授予权限，使 AKS 能够将资源部署到其中。

```azure-cli
APPID="<SERVICE_PRINCIPAL_APPID_GOES_HERE>"
PASSWORD="<SERVICEPRINCIPAL_PASSWORD_GOES_HERE>"
VNETID=$(az network vnet show -g $RG --name $VNET_NAME --query id -o tsv)

# Assign SP Permission to VNET

az role assignment create --assignee $APPID --scope $VNETID --role Contributor

# View Role Assignment
az role assignment list --assignee $APPID --all -o table
```

### <a name="deploy-aks"></a>部署 AKS

最后，可将 AKS 群集部署到专用于群集的现有子网中。 要部署到的目标子网是使用环境变量 (`$SUBNETID`) 定义的。 在前面的步骤中，我们未定义 `$SUBNETID` 变量。 若要设置子网 ID 的值，可使用以下命令：

```azurecli
SUBNETID="/subscriptions/$SUBID/resourceGroups/$RG/providers/Microsoft.Network/virtualNetworks/$VNET_NAME/subnets/$AKSSUBNET_NAME"
```

我们将出站类型定义为遵循子网中存在的 UDR，使 AKS 能够跳过负载均衡器（现在可以严格要求它是内部负载均衡器）设置和 IP 预配。

可以添加 [API 服务器已授权 IP 范围](api-server-authorized-ip-ranges.md) AKS 功能，以便限制 API 服务器仅访问防火墙的公共终结点。 已授权 IP 范围功能在示意图中表示为 NSG，必须通过此 NSG 才能访问控制平面。 启用已授权 IP 范围功能来限制 API 服务器访问权限时，开发人员工具必须使用防火墙虚拟网络中的 Jumpbox，或者必须将所有开发人员终结点添加到已授权 IP 范围。

> [!TIP]
> 可向群集部署添加更多功能，例如“专用群集”。 使用已授权 IP 范围时，Jumpbox 需位于群集网络内部才能访问 API 服务器。

```azure-cli
az aks create -g $RG -n $AKS_NAME -l $LOC \
  --node-count 3 \
  --network-plugin azure --generate-ssh-keys \
  --service-cidr 192.168.0.0/16 \
  --dns-service-ip 192.168.0.10 \
  --docker-bridge-address 172.22.0.1/29 \
  --vnet-subnet-id $SUBNETID \
  --service-principal $APPID \
  --client-secret $PASSWORD \
  --load-balancer-sku standard \
  --outbound-type userDefinedRouting \
  --api-server-authorized-ip-ranges $FWPUBLIC_IP
  ```

### <a name="enable-developer-access-to-the-api-server"></a>使开发人员能够访问 API 服务器

由于为群集设置了已授权 IP 范围，因此必须将开发人员工具 IP 地址添加到 AKS 群集的已批准 IP 范围列表，才能访问 API 服务器。 另一种做法是在防火墙虚拟网络中的单独子网内，使用所需的工具配置 Jumpbox。

使用以下命令将另一个 IP 地址添加到已批准范围

```bash
# Retrieve your IP address
CURRENT_IP=$(dig @resolver1.opendns.com ANY myip.opendns.com +short)

# Add to AKS approved list
az aks update -g $RG -n $AKS_NAME --api-server-authorized-ip-ranges $CURRENT_IP/32

```

 使用 [az aks get-credentials][az-aks-get-credentials] 命令将 `kubectl` 配置为连接到新建的 Kubernetes 群集。 

 ```azure-cli
 az aks get-credentials -g $RG -n $AKS_NAME
 ```

### <a name="setup-the-internal-load-balancer"></a>设置内部负载均衡器

AKS 已在群集中部署了一个负载均衡器，可将该负载均衡器设置为[内部负载均衡器](internal-lb.md)。

若要创建内部负载均衡器，请创建名为 internal-lb.yaml 的服务清单并在其中包含 LoadBalancer 服务类型和 azure-load-balancer-internal 注释，如以下示例所示：

```yaml
apiVersion: v1
kind: Service
metadata:
  name: internal-app
  annotations:
    service.beta.kubernetes.io/azure-load-balancer-internal: "true"
    service.beta.kubernetes.io/azure-load-balancer-internal-subnet: "contosofinsvcsubnet"
spec:
  type: LoadBalancer
  ports:
  - port: 80
  selector:
    app: internal-app
```

使用 kubectl apply 部署内部负载均衡器，并指定 YAML 清单的名称：

```bash
kubectl apply -f internal-lb.yaml
```

## <a name="deploy-a-kubernetes-service"></a>部署 Kubernetes 服务

由于群集出站类型设置为 UDR，在创建群集时，AKS 不会自动完成将代理节点关联为负载均衡器后端池的操作。 但是，在部署 Kubernetes 服务时，Kubernetes Azure 云提供商会处理后端池关联。

通过将以下 yaml 复制为名为 `example.yaml` 的文件来部署 Azure 投票应用程序。

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: azure-vote-back
spec:
  replicas: 1
  selector:
    matchLabels:
      app: azure-vote-back
  template:
    metadata:
      labels:
        app: azure-vote-back
    spec:
      nodeSelector:
        "beta.kubernetes.io/os": linux
      containers:
      - name: azure-vote-back
        image: redis
        resources:
          requests:
            cpu: 100m
            memory: 128Mi
          limits:
            cpu: 250m
            memory: 256Mi
        ports:
        - containerPort: 6379
          name: redis
---
apiVersion: v1
kind: Service
metadata:
  name: azure-vote-back
spec:
  ports:
  - port: 6379
  selector:
    app: azure-vote-back
---
apiVersion: apps/v1
kind: Deployment
metadata:
  name: azure-vote-front
spec:
  replicas: 1
  selector:
    matchLabels:
      app: azure-vote-front
  template:
    metadata:
      labels:
        app: azure-vote-front
    spec:
      nodeSelector:
        "beta.kubernetes.io/os": linux
      containers:
      - name: azure-vote-front
        image: dockerhub.azk8s.cn/microsoft/azure-vote-front:v1
        resources:
          requests:
            cpu: 100m
            memory: 128Mi
          limits:
            cpu: 250m
            memory: 256Mi
        ports:
        - containerPort: 80
        env:
        - name: REDIS
          value: "azure-vote-back"
---
apiVersion: v1
kind: Service
metadata:
  name: azure-vote-front
  annotations:
    service.beta.kubernetes.io/azure-load-balancer-internal: "true"
    service.beta.kubernetes.io/azure-load-balancer-internal-subnet: "contosofinsvcsubnet"
spec:
  type: LoadBalancer
  ports:
  - port: 80
  selector:
    app: azure-vote-front
```

运行以下命令来部署服务：

```bash
kubectl apply -f example.yaml
```

## <a name="add-a-dnat-rule-to-azure-firewall"></a>将 DNAT 规则添加到 Azure 防火墙

若要配置入站连接，必须将一个 DNAT 规则写入到 Azure 防火墙。 为了测试与群集的连接，我们为防火墙前端公共 IP 地址定义了一个规则，以便路由到内部服务公开的内部 IP。

可以自定义目标地址，因为它是防火墙上要访问的端口。 转换的地址必须是内部负载均衡器的 IP 地址。 转换的端口必须是 Kubernetes 服务的已公开端口。

你需要指定分配给 Kubernetes 服务所创建的负载均衡器的内部 IP 地址。 运行以下命令来检索该地址：

```bash
kubectl get services
```

所需的 IP 地址将在“EXTERNAL-IP”列中列出，如下所示。

```bash
NAME               TYPE           CLUSTER-IP       EXTERNAL-IP   PORT(S)        AGE
azure-vote-back    ClusterIP      192.168.92.209   <none>        6379/TCP       23m
azure-vote-front   LoadBalancer   192.168.19.183   100.64.2.5    80:32106/TCP   23m
kubernetes         ClusterIP      192.168.0.1      <none>        443/TCP        4d3h
```

```azure-cli
az network firewall nat-rule create --collection-name exampleset --destination-addresses $FWPUBLIC_IP --destination-ports 80 --firewall-name $FWNAME --name inboundrule --protocols Any --resource-group $RG --source-addresses '*' --translated-port 80 --action Dnat --priority 100 --translated-address <INSERT IP OF K8s SERVICE>
```

## <a name="clean-up-resources"></a>清理资源

> [!NOTE]
> 删除 Kubernetes 内部服务时，如果内部负载均衡器不再由任何服务使用，则 Azure 云提供商会删除内部负载均衡器。 下次部署服务时，如果找不到使用所请求配置的负载均衡器，则会部署一个负载均衡器。

若要清理 Azure 资源，请删除 AKS 资源组。

```azure-cli
az group delete -g $RG
```

## <a name="validate-connectivity"></a>验证连接

在浏览器中导航到 Azure 防火墙前端 IP 地址来验证连接。

应会看到 Azure 投票应用的图像。

## <a name="next-steps"></a>后续步骤

参阅 [Azure 网络 UDR 概述](/virtual-network/virtual-networks-udr-overview)。

参阅[如何创建、更改或删除路由表](/virtual-network/manage-route-table)。

<!-- LINKS - internal -->

[az-aks-get-credentials]: https://docs.microsoft.com/cli/azure/aks?view=azure-cli-latest#az-aks-get-credentials

<!-- Update_Description: new article about egress outboundtype -->
<!--NEW.date: 05/06/2020-->