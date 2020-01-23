---
title: 管理工作区中的角色
titleSuffix: Azure Machine Learning
description: 了解如何使用基于角色的访问控制 (RBAC) 访问 Azure 机器学习工作区。
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.reviewer: jmartens
ms.author: larryfr
author: Blackmist
ms.date: 11/06/2019
ms.custom: seodec18
ms.openlocfilehash: 56c7d3790e34905015b45b33048dcd522c22b77e
ms.sourcegitcommit: 623d64ef33e80d5f84b6dcf6d1ef4120fe4b8c08
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/02/2020
ms.locfileid: "75598861"
---
# <a name="manage-access-to-an-azure-machine-learning-workspace"></a>管理对 Azure 机器学习工作区的访问权限
[!INCLUDE [aml-applies-to-enterprise-sku](../../includes/aml-applies-to-enterprise-sku.md)]

本文介绍了如何管理对 Azure 机器学习工作区的访问权限。 [基于角色的访问控制 (RBAC)](/role-based-access-control/overview) 用于管理对 Azure 资源的访问权限。 Azure Active Directory 中的用户可获得特定角色，这些角色授予了对资源的访问权限。 Azure 提供内置角色和创建自定义角色的功能。

## <a name="default-roles"></a>默认角色

Azure 机器学习工作区是一种 Azure 资源。 与其他 Azure 资源一样，当创建新的 Azure 机器学习工作区时，它附带三个默认角色。 可以将用户添加到工作区，并将他们分配给这些内置角色之一。

| 角色 | 访问级别 |
| --- | --- |
| **读者** | 工作区中的只读操作。 读者可以在工作区中列出和查看资产，但不能创建或更新这些资产。 |
| **参与者** | 在工作区中查看、创建、编辑或删除（如果适用）资产。 例如，参与者可以创建试验、创建或附加计算群集、提交运行和部署 Web 服务。 |
| **所有者** | 对工作区的完全访问权限，包括能够在工作区中查看、创建、编辑或删除（如果适用）资产。 此外，还可以更改角色分配。 |

> [!IMPORTANT]
> 在 Azure 中，角色访问的作用域可以限定为多个级别。 例如，对工作区具有所有者访问权限的人可能没有对包含工作区的资源组的所有者访问权限。 有关详细信息信息，请参阅 [RBAC 工作原理](/role-based-access-control/overview#how-rbac-works)。

有关特定内置角色的详细信息，请参阅 [Azure 的内置角色](/role-based-access-control/built-in-roles)。

## <a name="manage-workspace-access"></a>管理工作区访问权限

如果你是工作区的所有者，则可以为工作区添加和删除角色。 还可以将角色分配给用户。 使用以下链接了解如何管理访问权限：
- [Azure 门户 UI](/role-based-access-control/role-assignments-portal)
- [PowerShell](/role-based-access-control/role-assignments-powershell)
- [Azure CLI](/role-based-access-control/role-assignments-cli)
- [REST API](/role-based-access-control/role-assignments-rest)
- [Azure Resource Manager 模板](/role-based-access-control/role-assignments-template)

如果已安装 [Azure 机器学习 CLI](reference-azure-machine-learning-cli.md)，则还可以使用 CLI 命令为用户分配角色。

```azurecli 
az ml workspace share -w <workspace_name> -g <resource_group_name> --role <role_name> --user <user_corp_email_address>
```

`user` 字段是 Azure Active Directory 实例中现有用户的电子邮件地址，该实例中包含工作区父订阅。 下面是此命令的用法示例：

```azurecli 
az ml workspace share -w my_workspace -g my_resource_group --role Contributor --user jdoe@contoson.com
```

## <a name="create-custom-role"></a>创建自定义角色

如果内置角色不够，可以创建自定义角色。 自定义角色可能具有该工作区中的读取、写入、删除和计算资源权限。 可以使角色在特定工作区级别、特定资源组级别或特定订阅级别可用。

> [!NOTE]
> 必须是该级别资源的所有者，才能在该资源中创建自定义角色。

要创建自定义角色，请首先构造角色定义 JSON 文件，指定角色的权限和作用域。 以下示例定义了名为“数据科学家”的自定义角色，其作用域为特定工作区级别：

`data_scientist_role.json`：
```json
{
    "Name": "Data Scientist",
    "IsCustom": true,
    "Description": "Can run experiment but can't create or delete compute.",
    "Actions": ["*"],
    "NotActions": [
        "Microsoft.MachineLearningServices/workspaces/*/delete",
        "Microsoft.MachineLearningServices/workspaces/computes/*/write",
        "Microsoft.MachineLearningServices/workspaces/computes/*/delete", 
        "Microsoft.Authorization/*/write"
    ],
    "AssignableScopes": [
        "/subscriptions/<subscription_id>/resourceGroups/<resource_group_name>/providers/Microsoft.MachineLearningServices/workspaces/<workspace_name>"
    ]
}
```

可以更改 `AssignableScopes` 字段，以在订阅级别、资源组级别或特定工作区级别设置此自定义角色的作用域。

此自定义角色可以在工作区中执行除以下操作之外的所有操作：

- 创建或更新计算资源。
- 删除计算资源。
- 添加、删除或更改角色分配。
- 删除工作区。

要部署此自定义角色，请使用以下 Azure CLI 命令：

```azurecli 
az role definition create --role-definition data_scientist_role.json
```

部署后，此角色在指定工作区中可用。 现在，可以在 Azure 门户中添加和分配此角色。 或者，可以使用 `az ml workspace share` CLI 命令将此角色分配给用户：

```azurecli
az ml workspace share -w my_workspace -g my_resource_group --role "Data Scientist" --user jdoe@contoson.com
```

有关自定义角色的详细信息，请参阅 [Azure 资源的自定义角色](/role-based-access-control/custom-roles)。

有关可用于自定义角色的操作的详细信息，请参阅[资源提供程序操作](/role-based-access-control/resource-provider-operations#microsoftmachinelearningservices)。

## <a name="next-steps"></a>后续步骤

- [企业安全性概述](concept-enterprise-security.md)
- [在虚拟网络中安全运行试验和推理/评分](how-to-enable-virtual-network.md)
- [教程：训练模型](tutorial-train-models-with-aml.md)
- [资源提供程序操作](/role-based-access-control/resource-provider-operations#microsoftmachinelearningservices)