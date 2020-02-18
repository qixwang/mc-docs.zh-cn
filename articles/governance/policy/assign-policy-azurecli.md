---
title: 快速入门：通过 Azure CLI 进行新策略分配
description: 本快速入门介绍如何使用 Azure CLI 创建 Azure Policy 分配以识别不合规的资源。
author: DCtheGeek
ms.author: v-tawe
origin.date: 01/11/2020
ms.date: 02/17/2020
ms.topic: quickstart
ms.openlocfilehash: f6087ee5702df2c879d90d83294d87bc5b3d85b1
ms.sourcegitcommit: 0b07f1d36ac02da055874630d6edc31cb0a15269
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2020
ms.locfileid: "77112177"
---
# <a name="quickstart-create-a-policy-assignment-to-identify-non-compliant-resources-with-azure-cli"></a>快速入门：使用 Azure CLI 创建策略分配以识别不符合的资源

若要了解 Azure 中的符合性，第一步是确定资源的状态。
本快速入门逐步讲解如何创建策略分配，以识别未使用托管磁盘的虚拟机。

此过程结束时，你可以成功识别哪些虚拟机未使用托管磁盘。 这些虚拟机不符合策略分配要求。 

Azure CLI 用于从命令行或脚本创建和管理 Azure 资源。 本指南使用 Azure CLI 创建策略分配，并识别 Azure 环境中的不合规资源。

## <a name="prerequisites"></a>必备条件

- 如果没有 Azure 订阅，请在开始前创建一个[试用帐户](https://www.azure.cn/pricing/1rmb-trial/)。

- 本快速入门需要运行 Azure CLI 版本 2.0.76 或更高版本，以便在本地安装并使用 CLI。 若要查找版本，请运行 `az --version`。 如果需要进行安装或升级，请参阅[安装 Azure CLI](/cli/install-azure-cli)。

- 使用 Azure CLI 注册 Azure Policy Insights 资源提供程序。 注册此资源提供程序可确保订阅能够使用它。 要注册资源提供程序，必须具有注册资源提供程序操作的权限。 此操作包含在“参与者”和“所有者”角色中。 运行以下命令，注册资源提供程序：

  ```azurecli
  az provider register --namespace 'Microsoft.PolicyInsights'
  ```

  有关注册和查看资源提供程序的详细信息，请参阅[资源提供程序和类型](../../azure-resource-manager/management/resource-providers-and-types.md)

- 安装 [ARMClient](https://github.com/projectkudu/ARMClient)（如果尚未安装）。 该工具可将 HTTP 请求发送到基于 Azure 资源管理器的 API。

<!-- [!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)] -->

## <a name="create-a-policy-assignment"></a>创建策略分配

本快速入门将创建一个策略分配，并分配“审核未使用托管磁盘的 VM”定义  。 此策略定义可识别不符合策略定义中设置的条件的资源。

运行以下命令创建策略分配：

```azurecli
az policy assignment create --name 'audit-vm-manageddisks' --display-name 'Audit VMs without managed disks Assignment' --scope '<scope>' --policy '<policy definition ID>'
```

上述命令使用以下信息：

- **名称** - 分配的实际名称。 对于此示例，使用 _audit-vm-manageddisks_。
- **显示名称** - 策略分配的显示名称。 本例使用了“审核未使用托管磁盘分配的虚拟机”  。
- **策略** - 策略定义 ID，用作创建分配的依据。 在本例中，它为策略定义“审核未使用托管磁盘的 VM”的 ID  。 若要获取策略定义 ID，请运行以下命令：`az policy definition list --query "[?displayName=='Audit VMs that do not use managed disks']"`
- **范围** - 范围确定在其中实施策略分配的资源或资源组。 它可以从订阅延伸至资源组。 请务必将 &lt;scope&gt; 替换为资源组的名称。

## <a name="identify-non-compliant-resources"></a>识别不合规的资源

若要查看此新分配下不合规的资源，请运行以下命令获取策略分配 ID：

```azurecli
az policy assignment list --query "[?displayName=='Audit VMs without managed disks Assignment'].id"
```

有关策略分配 ID 的详细信息，请参阅 [az policy assignment](/cli/policy/assignment)。

接下来，运行以下命令，获取输出到 JSON 文件中的不合规资源的资源 ID：

```console
armclient post "/subscriptions/<subscriptionID>/resourceGroups/<rgName>/providers/Microsoft.PolicyInsights/policyStates/latest/queryResults?api-version=2019-09-01&$filter=IsCompliant eq false and PolicyAssignmentId eq '<policyAssignmentID>'&$apply=groupby((ResourceId))" > <json file to direct the output with the resource IDs into>
```

结果应如以下示例所示：

```json
{
    "@odata.context": "https://management.chinacloudapi.cn/subscriptions/<subscriptionId>/providers/Microsoft.PolicyInsights/policyStates/$metadata#latest",
    "@odata.count": 3,
    "value": [{
            "@odata.id": null,
            "@odata.context": "https://management.chinacloudapi.cn/subscriptions/<subscriptionId>/providers/Microsoft.PolicyInsights/policyStates/$metadata#latest/$entity",
            "ResourceId": "/subscriptions/<subscriptionId>/resourcegroups/<rgname>/providers/microsoft.compute/virtualmachines/<virtualmachineId>"
        },
        {
            "@odata.id": null,
            "@odata.context": "https://management.chinacloudapi.cn/subscriptions/<subscriptionId>/providers/Microsoft.PolicyInsights/policyStates/$metadata#latest/$entity",
            "ResourceId": "/subscriptions/<subscriptionId>/resourcegroups/<rgname>/providers/microsoft.compute/virtualmachines/<virtualmachine2Id>"
        },
        {
            "@odata.id": null,
            "@odata.context": "https://management.chinacloudapi.cn/subscriptions/<subscriptionId>/providers/Microsoft.PolicyInsights/policyStates/$metadata#latest/$entity",
            "ResourceId": "/subscriptions/<subscriptionName>/resourcegroups/<rgname>/providers/microsoft.compute/virtualmachines/<virtualmachine3ID>"
        }

    ]
}
```

这些结果与 Azure 门户视图中“不合规资源”下通常所列的结果类似。 

## <a name="clean-up-resources"></a>清理资源

要删除创建的分配，请使用以下命令：

```azurecli
az policy assignment delete --name 'audit-vm-manageddisks' --scope '/subscriptions/<subscriptionID>/<resourceGroupName>'
```

## <a name="next-steps"></a>后续步骤

本快速入门已分配一个策略定义用于识别 Azure 环境中的不合规资源。

要了解有关分配策略以验证新资源是否符合要求的详细信息，请继续以下教程：

> [!div class="nextstepaction"]
> [创建和管理策略](./tutorials/create-and-manage.md)