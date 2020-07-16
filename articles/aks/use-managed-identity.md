---
title: 在 Azure Kubernetes 服务中使用托管标识
description: 了解如何在 Azure Kubernetes 服务 (AKS) 中使用托管标识
services: container-service
author: rockboyfor
ms.topic: article
ms.date: 07/13/2020
ms.testscope: yes
ms.testdate: 07/13/2020
ms.author: v-yeche
ms.openlocfilehash: a4bb98fdeae10619ae9297b8be53cc9fcbc514fd
ms.sourcegitcommit: 6c9e5b3292ade56d812e7e214eeb66aeb9b8776e
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/10/2020
ms.locfileid: "86218854"
---
<!--Verified successfully-->
# <a name="use-managed-identities-in-azure-kubernetes-service"></a>在 Azure Kubernetes 服务中使用托管标识

目前，Azure Kubernetes 服务 (AKS) 群集（特指 Kubernetes 云提供商）需要使用标识才能在 Azure 中创建其他资源，例如负载均衡器和托管磁盘。 此标识可以是托管标识或服务主体。 如果使用[服务主体](kubernetes-service-principal.md)，你必须提供一个服务主体，或由 AKS 代表你创建一个。 如果使用托管标识，AKS 会自动为你创建托管标识。 使用服务主体的群集最终会达到这样一种状态，即，必须续订服务主体才能让群集保持正常运行。 管理服务主体会增加复杂性，这也是托管标识使用起来更简单的原因。 服务主体和托管标识适用相同的权限要求。

托管标识本质上是服务主体的包装器，这使其更易于管理。 根据 Azure Active Directory 的默认设置，MI 的凭据轮换每 46 天自动发生一次。 AKS 使用系统分配和用户分配的托管标识类型。 这些标识目前是不可变的。 若要了解详细信息，请阅读 [Azure 资源托管标识](https://docs.microsoft.com/azure/active-directory/managed-identities-azure-resources/overview)。

## <a name="before-you-begin"></a>准备阶段

必须安装了以下资源：

- Azure CLI，版本 2.2.0 或更高版本

## <a name="limitations"></a>限制

* 目前不支持自带托管标识。
* 具有托管标识的 AKS 群集只能在群集创建过程中启用。
* 无法将现有 AKS 群集更新或升级为启用托管标识。
* 在群集升级操作期间，托管标识暂时不可用。

## <a name="summary-of-managed-identities"></a>托管标识摘要

AKS 对内置服务和加载项使用多个托管标识。

| 标识                       | 名称    | 使用案例 | 默认权限 | 自带标识
|----------------------------|-----------|----------|
| 控制面板 | 不可见 | 由 AKS 用于管理网络资源，例如为入口、公共 IP 等创建负载均衡器| 节点资源组的参与者角色 | 目前不支持
| Kubelet | AKS Cluster Name-agentpool | 向 Azure 容器注册表 (ACR) 进行身份验证 | 节点资源组的读取者角色 | 目前不支持
| 加载项 | AzureNPM | 无需标识 | 不可用 | 否
| 加载项 | AzureCNI 网络监视 | 无需标识 | 不可用 | 否
| 加载项 | azurepolicy（网关守卫） | 无需标识 | 不可用 | 否
| 加载项 | azurepolicy | 无需标识 | 不可用 | 否
| 加载项 | Calico | 无需标识 | 不可用 | 否
| 加载项 | 仪表板 | 无需标识 | 不可用 | 否
| 加载项 | HTTPApplicationRouting | 管理所需的网络资源 | 节点资源组的读取者角色，DNS 区域的参与者角色 | 否
| 加载项 | 入口应用程序网关 | 管理所需的网络资源| 节点资源组的参与者角色 | 否
| 加载项 | omsagent | 用于将 AKS 指标发送到 Azure Monitor | “监视指标发布者”角色 | 否
| 加载项 | Virtual-Node (ACIConnector) | 管理 Azure 容器实例 (ACI) 所需的网络资源 | 节点资源组的参与者角色 | 否


## <a name="create-an-aks-cluster-with-managed-identities"></a>创建具有托管标识的 AKS 群集

现在，可以使用以下 CLI 命令创建具有托管标识的 AKS 群集。

首先，创建 Azure 资源组：

```azurecli
# Create an Azure resource group
az group create --name myResourceGroup --location chinanorth2
```

然后，创建 AKS 群集：

```azurecli
az aks create -g myResourceGroup -n myManagedCluster --enable-managed-identity
```

使用托管标识成功创建群集的命令中包含以下服务主体配置文件信息：

```json
"servicePrincipalProfile": {
    "clientId": "msi"
  }
```

使用以下命令查询控制平面托管标识的 objectid：

```azurecli
az aks show -g myResourceGroup -n MyManagedCluster --query "identity"
```

结果应如下所示：

```json
{
  "principalId": "<object_id>",   
  "tenantId": "<tenant_id>",      
  "type": "SystemAssigned"                                 
}
```

> [!NOTE]
> 若要创建并使用自己的 VNet、静态 IP 地址或附加的 Azure 磁盘（资源位于工作器节点资源组外部），请使用群集系统分配的托管标识的 PrincipalID 来执行角色分配。 有关角色分配的详细信息，请参阅[委托对其他 Azure 资源的访问权限](kubernetes-service-principal.md#delegate-access-to-other-azure-resources)。
>
> 向 Azure 云提供商使用的群集托管标识授予的权限可能需要 60 分钟才能填充完毕。

最后，获取用于访问群集的凭据：

```azurecli
az aks get-credentials --resource-group myResourceGroup --name MyManagedCluster
```

系统将在几分钟内创建群集。 然后，你可以将应用程序工作负荷部署到新群集中，并与之交互，就像与基于服务主体的 AKS 群集交互一样。

<!--Not Available on ## Next steps-->
<!--Not Available on [Azure Resource Manager (ARM) templates ][aks-arm-template]-->

<!-- LINKS - external -->

<!--Not Available on [aks-arm-template]: https://docs.microsoft.com/azure/templates/microsoft.containerservice/managedclusters-->


<!-- Update_Description: new article about use managed identity -->
<!--NEW.date: 07/13/2020-->