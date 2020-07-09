---
title: 使用新的 Azure 应用程序网关为新的 AKS 群集启用入口控制器加载项
description: 本教程介绍了如何使用新的应用程序网关为新的 AKS 群集启用入口控制器加载项
services: application-gateway
author: caya
ms.service: application-gateway
ms.topic: article
ms.date: 06/23/2020
ms.author: v-junlch
ms.openlocfilehash: e74111addae2f4f3a9032a8b63e3c8197c33b8a5
ms.sourcegitcommit: 3a8a7d65d0791cdb6695fe6c2222a1971a19f745
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/28/2020
ms.locfileid: "85516783"
---
# <a name="tutorial-enable-application-gateway-ingress-controller-add-on-for-a-new-aks-cluster-with-a-new-application-gateway-through-azure-cli-preview"></a>教程：通过 Azure CLI 使用新的应用程序网关为新的 AKS 群集启用应用程序网关入口控制器加载项（预览版）

你可以使用 Azure CLI 为 [Azure Kubernetes 服务 (AKS)](https://www.azure.cn/home/features/kubernetes-service/) 群集启用[应用程序网关入口控制器 (AGIC)](ingress-controller-overview.md) 加载项（当前为预览版）。 在本教程中，将创建一个启用了 AGIC 加载项的 AKS 群集，该加载项将自动创建要使用的应用程序网关。 然后将部署一个示例应用程序，它将利用 AGIC 加载项通过应用程序网关公开应用程序。 此加载项为 AKS 群集提供了一种比[之前通过 Helm 进行部署](ingress-controller-overview.md#difference-between-helm-deployment-and-aks-add-on)快得多的 AGIC 部署方法，而且还提供了完全托管体验。    

本教程介绍如何执行下列操作：

> [!div class="checklist"]
> * 创建资源组 
> * 创建启用 AGIC 加载项的新 AKS 群集 
> * 在 AKS 群集上部署将 AGIC 用于入口的示例应用程序
> * 检查是否可以通过应用程序网关访问应用程序

如果没有 Azure 订阅，可在开始前创建一个[试用帐户](https://www.azure.cn/pricing/1rmb-trial)。

如果选择在本地安装并使用 CLI，本教程要求运行 Azure CLI 2.0.4 或更高版本。 若要查找版本，请运行 `az --version`。 如果需要进行安装或升级，请参阅[安装 Azure CLI](/cli/install-azure-cli)。

使用 [az feature register](/cli/feature#az-feature-register) 命令注册 AKS-IngressApplicationGatewayAddon 功能标志，如以下示例所示；当此加载项仍处于预览版时，只需为每个订阅执行一次此操作：
```azurecli
az feature register --name AKS-IngressApplicationGatewayAddon --namespace microsoft.containerservice
```

可能需要花费几分钟时间，状态才会显示为“已注册”。 可以使用 [az feature list](/cli/feature#az-feature-register) 命令检查注册状态：
```azurecli
az feature list -o table --query "[?contains(name, 'Microsoft.ContainerService/AKS-IngressApplicationGatewayAddon')].{Name:name,State:properties.state}"
```

准备就绪后，使用 [az provider register](/cli/provider#az-provider-register) 命令刷新 Microsoft.ContainerService 资源提供程序的注册状态：
```azurecli
az provider register --namespace Microsoft.ContainerService
```

对于本教程，请确保安装/更新 aks-preview 扩展；请使用以下 Azure CLI 命令
```azurecli
az extension add --name aks-preview
az extension list
```
```azurecli
az extension update --name aks-preview
az extension list
```

## <a name="create-a-resource-group"></a>创建资源组

在 Azure 中，可将相关的资源分配到资源组。 使用 [az group create](/cli/group#az-group-create) 创建资源组。 下面的示例在 canadacentral 位置（区域）创建名为 myResourceGroup 的资源组 。 

```azurecli
az group create --name myResourceGroup --location canadacentral
```

## <a name="deploy-a-new-aks-cluster-with-agic-add-on-enabled"></a>部署启用了 AGIC 加载项的新 AKS 群集

现在，部署启用了 AGIC 加载项的新 AKS 群集。 如果你在部署启用了 AGIC 加载项的新 AKS 群集时未提供要使用的现有应用程序网关，我们将自动创建并设置新的应用程序网关，用于向 AKS 群集提供流量。  

> [!NOTE]
> 应用程序网关入口控制器 (AGIC) 加载项**仅**支持应用程序网关 v2 SKU（标准版和 WAF 版），**不**支持应用程序网关 v1 SKU。 通过 AGIC 加载项部署新的应用程序网关时，只能部署应用程序网关 Standard_v2 SKU。 若要为应用程序网关 WAF_v2 SKU 启用 AGIC 加载项，请通过门户在应用程序网关上启用 WAF，或者先创建 WAF_v2 应用程序网关，然后按照有关如何[使用现有 AKS 群集和现有应用程序网关启用 AGIC 加载项](tutorial-ingress-controller-add-on-existing.md)的说明进行操作。 

在下面的示例中，你将在所创建的资源组 myResourceGroup 中使用启用 AGIC 加载项的 [Azure CNI](/aks/concepts-network#azure-cni-advanced-networking) 部署名为 myCluster 的新 AKS 群集 。 如果在未指定现有应用程序网关的情况下部署启用 AGIC 加载项的新 AKS 群集，将自动创建 Standard_v2 SKU 应用程序网关，因此还需要指定应用程序网关的名称和子网地址空间。 应用程序网关的名称将是 myApplicationGateway，将要使用的子网地址空间是 10.2.0.0/16。 请确保你已在本教程开头添加/更新了 aks-preview 扩展。 

```azurecli
az aks create -n myCluster -g myResourceGroup --network-plugin azure --enable-managed-identity -a ingress-appgw --appgw-name myApplicationGateway --appgw-subnet-prefix "10.2.0.0/16" 
```

若要为 `az aks create` 命令配置其他参数，请访问[此处](https://docs.microsoft.com/en-us/cli/azure/aks?view=azure-cli-latest#az-aks-create)的参考信息。 

> [!NOTE]
> 创建的 AKS 群集将显示在创建的资源组 myResourceGroup 中。 但是，自动创建的应用程序网关将位于代理池所在的节点资源组中。 默认情况下，节点资源组将命名为 MC_resource-group-name_cluster-name_location，但可以修改。 

## <a name="deploy-a-sample-application-using-agic"></a>部署使用 AGIC 的示例应用程序

现在，你将向所创建的 AKS 群集部署一个示例应用程序，该应用程序将 AGIC 加载项用于入口，并将应用程序网关连接到 AKS 群集。 首先，你将通过运行 `az aks get-credentials` 命令获取所部署的 AKS 群集的凭据。 

```azurecli
az aks get-credentials -n myCluster -g myResourceGroup
```

获取你创建的群集的凭据后，运行以下命令来设置一个示例应用程序，该应用程序将 AGIC 用于群集的入口。 AGIC 会使用你部署的新示例应用程序的相应路由规则更新你之前设置的应用程序网关。  

```azurecli
kubectl apply -f https://raw.githubusercontent.com/Azure/application-gateway-kubernetes-ingress/master/docs/examples/aspnetapp.yaml 
```

## <a name="check-that-the-application-is-reachable"></a>检查应用程序是否可供访问

将应用程序网关设置为向 AKS 群集提供流量以后，接下来让我们验证是否可以访问你的应用程序。 首先获取入口的 IP 地址。 

```azurecli
kubectl get ingress
```

检查你创建的示例应用程序是否已启动并正在运行，方法是访问通过运行上述命令获取的应用程序网关的 IP 地址或通过 `curl` 进行检查。 应用程序网关可能需要花费一分钟来获取更新，因此，如果应用程序网关在门户上仍然处于“正在更新”状态，则等待其完成更新，然后再尝试访问该 IP 地址。 

## <a name="clean-up-resources"></a>清理资源

当不再需要资源组、应用程序网关以及所有相关资源时，请将其删除。

```azurecli
az group delete --name myResourceGroup
```

## <a name="next-steps"></a>后续步骤
* [详细了解如何禁用 AGIC 加载项](./ingress-controller-disable-addon.md)
* [详细了解 AGIC 支持哪些注释](./ingress-controller-annotations.md)
* [排查 AGIC 的问题](./ingress-controller-troubleshoot.md)


