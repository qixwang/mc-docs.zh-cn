---
title: 为 Azure Kubernetes 服务 (AKS) 群集重置凭据
description: 了解如何为 Azure Kubernetes 服务 (AKS) 中的群集更新或重置服务主体凭据
services: container-service
ms.topic: article
origin.date: 05/31/2019
ms.date: 03/09/2020
ms.author: v-yeche
ms.openlocfilehash: 704b668e18015ffc9d015808ab975b932701dc43
ms.sourcegitcommit: 3c98f52b6ccca469e598d327cd537caab2fde83f
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/13/2020
ms.locfileid: "79290842"
---
# <a name="update-or-rotate-the-credentials-for-a-service-principal-in-azure-kubernetes-service-aks"></a>为 Azure Kubernetes 服务 (AKS) 中的服务主体更新或轮换凭据

默认情况下，使用服务主体创建的 AKS 群集具有为期一年的有效期。 在有效期即将结束时，你可以重置凭据来将服务主体延长额外的一段时间。 作为已定义安全策略的一部分，还可能要更新或轮换凭据。 本文详细介绍如何为 AKS 群集更新这些凭据。

## <a name="before-you-begin"></a>准备阶段

需要安装并配置 Azure CLI 2.0.65 或更高版本。 运行  `az --version` 即可查找版本。 如果需要进行安装或升级，请参阅 [安装 Azure CLI][install-azure-cli]。

## <a name="choose-to-update-or-create-a-service-principal"></a>选择要更新或创建服务主体

要为 AKS 群集更新凭据时，可以选择：

* 为群集使用的现有服务主体更新凭据，或
* 创建服务主体并更新群集以使用这些新凭据。

### <a name="update-existing-service-principal-expiration"></a>更新现有服务主体到期时间

若要为现有服务主体更新凭据，请使用 [az aks show][az-aks-show] 命令获取群集的服务主体 ID。 以下示例获取 myResourceGroup 资源组中名为 myAKSCluster 的群集的 ID   。 服务主体 ID 设置为名为“SP_ID”  变量以供在其他命令中使用。

```azurecli
SP_ID=$(az aks show --resource-group myResourceGroup --name myAKSCluster \
    --query servicePrincipalProfile.clientId -o tsv)
```

借助包含服务主体 ID 的变量集，现在使用 [az ad sp credential reset][az-ad-sp-credential-reset] 重置凭据。 下面的示例可让 Azure 平台为服务主体生成新安全密码。 此新安全密码也存储为变量。

```azurecli
SP_SECRET=$(az ad sp credential reset --name $SP_ID --query password -o tsv)
```

现在继续浏览[使用新凭据更新 AKS 群集](#update-aks-cluster-with-new-credentials)。 若要在 AKS 群集上反映服务主体更改，必须执行此步骤。

### <a name="create-a-new-service-principal"></a>创建新服务主体

如果在上一部分中选择更新现有服务主体凭据，请跳过此步骤。 继续浏览[使用新凭据更新 AKS 群集](#update-aks-cluster-with-new-credentials)。

若要创建服务主体，然后更新 AKS 群集以使用这些新凭据，请使用 [az ad sp create-for-rbac][az-ad-sp-create] 命令。 在以下示例中，`--skip-assignment` 参数阻止系统分配更多的默认分配。

```azurecli
az ad sp create-for-rbac --skip-assignment
```

输出类似于以下示例。 记下你自己的 `appId`和 `password`。 下一步会使用这些值。

```json
{
  "appId": "7d837646-b1f3-443d-874c-fd83c7c739c5",
  "name": "7d837646-b1f3-443d-874c-fd83c7c739c",
  "password": "a5ce83c9-9186-426d-9183-614597c7f2f7",
  "tenant": "a4342dc8-cd0e-4742-a467-3129c469d0e5"
}
```

现在使用自己的 [az ad sp create-for-rbac][az-ad-sp-create] 命令的输出为服务主体 ID 和客户端密码定义变量，如下面的示例所示。 SP_ID  是 appId  ，SP_SECRET  是 password  ：

```azurecli
SP_ID=7d837646-b1f3-443d-874c-fd83c7c739c5
SP_SECRET=a5ce83c9-9186-426d-9183-614597c7f2f7
```

现在继续浏览[使用新凭据更新 AKS 群集](#update-aks-cluster-with-new-credentials)。 若要在 AKS 群集上反映服务主体更改，必须执行此步骤。

## <a name="update-aks-cluster-with-new-credentials"></a>使用新凭据更新 AKS 群集

无论是选择为现有服务主体更新凭据还是创建服务主体，现在都通过 [az aks update-credentials][az-aks-update-credentials] 命令，使用新凭据更新 AKS 群集。 会使用 --service-principal  和 --client-secret  的变量：

```azurecli
az aks update-credentials \
    --resource-group myResourceGroup \
    --name myAKSCluster \
    --reset-service-principal \
    --service-principal $SP_ID \
    --client-secret $SP_SECRET
```

在 AKS 中更新服务主体凭据需要一段时间。

## <a name="next-steps"></a>后续步骤

在本文中，更新了 AKS 群集本身的服务主体。 有关如何为群集中的工作负荷管理标识的详细信息，请参阅 [AKS 中的身份验证和授权的最佳做法][best-practices-identity]。

<!-- LINKS - internal -->

[install-azure-cli]: https://docs.azure.cn/cli/install-azure-cli?view=azure-cli-latest
[az-aks-show]: https://docs.microsoft.com/cli/azure/aks?view=azure-cli-latest#az-aks-show
[az-aks-update-credentials]: https://docs.microsoft.com/cli/azure/aks?view=azure-cli-latest#az-aks-update-credentials
[best-practices-identity]: operator-best-practices-identity.md
[az-ad-sp-create]: https://docs.azure.cn/cli/ad/sp?view=azure-cli-latest#az-ad-sp-create-for-rbac
[az-ad-sp-credential-reset]: https://docs.azure.cn/cli/ad/sp/credential?view=azure-cli-latest#az-ad-sp-credential-reset

<!-- Update_Description: update meta properties, wording update, update link -->