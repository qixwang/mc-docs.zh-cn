---
title: 超出部署配额
description: 介绍如何解决这样一个错误：在资源组历史记录中有 800 多个部署。
ms.topic: troubleshooting
origin.date: 06/25/2020
ms.date: 07/13/2020
ms.testscope: yes
ms.testdate: 07/13/2020
ms.author: v-yeche
ms.openlocfilehash: 75e62964e5fab09d1f4969930bdc9aad48dc844e
ms.sourcegitcommit: 2bd0be625b21c1422c65f20658fe9f9277f4fd7c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/17/2020
ms.locfileid: "86440979"
---
<!--Verified successfully on 2020/07/13 by harris-->
# <a name="resolve-error-when-deployment-count-exceeds-800"></a>解决部署计数超出 800 的错误

每个资源组在其部署历史记录中最多只能有 800 个部署。 本文介绍在部署因超出允许的 800 个部署的限制而失败时可能会出现的错误。 若要解决此错误，请从资源组历史记录中删除部署。 从历史记录中删除部署不会影响已部署的任何资源。

> [!NOTE]
> 当你接近限制时，Azure 资源管理器会很快开始自动从历史记录中删除部署。 如果取消选择了自动删除，可能仍会看到此错误。 有关详细信息，请参阅[从部署历史记录中自动删除](deployment-history-deletions.md)。

## <a name="symptom"></a>症状

在部署过程中，你收到错误，该错误指出当前部署会超出 800 个部署的配额。

## <a name="solution"></a>解决方案

### <a name="azure-cli"></a>Azure CLI

使用 [az deployment group delete](https://docs.azure.cn/cli/group/deployment?view=azure-cli-latest#az-deployment-group-delete) 命令从历史记录中删除部署。

```azurecli
az deployment group delete --resource-group exampleGroup --name deploymentName
```

若要删除五天以前的所有部署，请使用：

```azurecli
startdate=$(date +%F -d "-5days")
deployments=$(az deployment group list --resource-group exampleGroup --query "[?properties.timestamp<'$startdate'].name" --output tsv)

for deployment in $deployments
do
  az deployment group delete --resource-group exampleGroup --name $deployment
done
```

可使用以下命令获取部署历史记录中的当前计数：

```azurecli
az deployment group list --resource-group exampleGroup --query "length(@)"
```

### <a name="azure-powershell"></a>Azure PowerShell

使用 [Remove-AzResourceGroupDeployment](https://docs.microsoft.com/powershell/module/az.resources/remove-azresourcegroupdeployment) 命令从历史记录中删除部署。

```powershell
Remove-AzResourceGroupDeployment -ResourceGroupName exampleGroup -Name deploymentName
```

若要删除五天以前的所有部署，请使用：

```powershell
$deployments = Get-AzResourceGroupDeployment -ResourceGroupName exampleGroup | Where-Object Timestamp -lt ((Get-Date).AddDays(-5))

foreach ($deployment in $deployments) {
  Remove-AzResourceGroupDeployment -ResourceGroupName exampleGroup -Name $deployment.DeploymentName
}
```

可使用以下命令获取部署历史记录中的当前计数：

```powershell
(Get-AzResourceGroupDeployment -ResourceGroupName exampleGroup).Count
```

## <a name="third-party-solutions"></a>第三方解决方案

以下外部解决方案解决特定场景的问题：

* [Azure 逻辑应用和 PowerShell 解决方案](https://devkimchi.com/2018/05/30/managing-excessive-arm-deployment-histories-with-logic-apps/)
* [AzDevOps 扩展](https://github.com/christianwaha/AzureDevOpsExtensionCleanRG)

<!-- Update_Description: update meta properties, wording update, update link -->