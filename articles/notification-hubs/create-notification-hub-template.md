---
title: 使用 Azure 资源管理器模板创建 Azure 通知中心
description: 了解如何使用 Azure 资源管理器模板创建 Azure 通知中心。
services: notification-hubs
author: sethmanheim
ms.service: notification-hubs
ms.topic: quickstart
ms.custom: subject-armqs
ms.author: v-tawe
origin.date: 05/15/2020
ms.date: 07/01/2020
ms.reviewer: thsomasu
ms.lastreviewed: 05/15/2020
ms.openlocfilehash: 56a14381c99d0e7bd666d92ddb304534c1b4d0af
ms.sourcegitcommit: ac70b12de243a9949bf86b81b2576e595e55b2a6
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/07/2020
ms.locfileid: "87917323"
---
# <a name="quickstart-create-a-notification-hub-using-an-azure-resource-manager-template"></a>快速入门：使用 Azure 资源管理器模板创建通知中心

Azure 通知中心提供易于使用且横向扩展的推送引擎，允许你从任何后端（云或本地）向任何平台（iOS、Android、Windows、Kindle 等）发送通知。 有关此服务的详细信息，请参阅[什么是 Azure 通知中心](notification-hubs-push-notification-overview.md)。

[!INCLUDE [About Azure Resource Manager](../../includes/resource-manager-quickstart-introduction.md)]

本快速入门使用 Azure 资源管理器模板来创建 Azure 通知中心命名空间，并在命名空间中创建名为“MyHub”的通知中心。

如果没有 Azure 订阅，请在开始前创建一个[试用帐户](https://wd.azure.cn/pricing/1rmb-trial/)。

## <a name="prerequisites"></a>先决条件

无。

## <a name="create-a-notification-hubs-namespace-and-hub"></a>创建通知中心命名空间和中心

### <a name="review-the-template"></a>查看模板

本快速入门中使用的模板来自 [Azure 快速入门模板](https://azure.microsoft.com/resources/templates/101-notification-hub/)。

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "namespaceName": {
            "type": "string",
            "metadata": {
                "description": "The name of the Notification Hubs namespace."
            }
        },
        "location": {
            "type": "string",
            "defaultValue": "[resourceGroup().location]",
            "metadata": {
                "description": "The location in which the Notification Hubs resources should be deployed."
            }
        }
    },
    "variables": {
        "hubName": "MyHub"
    },
    "resources": [
        {
            "apiVersion": "2017-04-01",
            "name": "[parameters('namespaceName')]",
            "type": "Microsoft.NotificationHubs/namespaces",
            "location": "[parameters('location')]",
            "kind": "NotificationHub",
            "sku": {
                "name": "Free"
            },
            "resources": [
                {
                    "name": "[concat(parameters('namespaceName'), '/', variables('hubName'))]",
                    "apiVersion": "2017-04-01",
                    "type": "Microsoft.NotificationHubs/namespaces/notificationHubs",
                    "location": "[parameters('location')]",
                    "dependsOn": [
                        "[parameters('namespaceName')]"
                    ]
                }
            ]
        }
    ]
}
```

* [Microsoft.NotificationHubs/namespaces](https://docs.microsoft.com/azure/templates/microsoft.notificationhubs/2017-04-01/namespaces)
* [Microsoft.NotificationHubs/namespaces/notificationHubs](https://docs.microsoft.com/azure/templates/microsoft.notificationhubs/2017-04-01/namespaces/notificationhubs)

## <a name="deploy-the-template"></a>部署模板

选择下图登录到 Azure 并打开一个模板。 模板采用通知中心命名空间名称作为参数。 然后，该模板创建一个具有该名称的命名空间，并在该命名空间中创建名为“MyHub”的通知中心。

[![“部署到 Azure”](../media/template-deployments/deploy-to-azure.svg)](https://portal.azure.cn/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-notification-hub%2Fazuredeploy.json)

## <a name="review-deployed-resources"></a>查看已部署的资源

可以使用 Azure 门户来检查已部署的资源，也可以使用 Azure CLI 或 Azure PowerShell 脚本列出已部署的通知中心命名空间和中心：

# <a name="powershell"></a>[PowerShell](#tab/PowerShell)

```azurepowershell
Get-AzNotificationHub -Namespace "nhtestns123" -ResourceGroup "ContosoNotificationsGroup"
Get-AzNotificationHubsNamespace -Namespace "nhtestns123"
```

# <a name="cli"></a>[CLI](#tab/CLI)

```azurecli
az notification-hub show --resource-group ContosoNotificationsGroup --namespace-name nhtestns123 --name MyHub
az notification-hub namespace show --resource-group ContosoNotificationsGroup --name nhtestns123
```

---
The output looks similar to: null
':::image type="content" source="media/create-notification-hub-template/verify-deploy.png" alt-text="Verify deployment"::': null
---

## <a name="clean-up-resources"></a>清理资源

如果不再需要资源组，可以将其删除，这将删除资源组中的资源。

```azurepowershell
$resourceGroupName = Read-Host -Prompt "Enter the resource group name"
Remove-AzResourceGroup -Name $resourceGroupName
Write-Host "Press [ENTER] to continue..."
```

## <a name="next-steps"></a>后续步骤

有关引导你完成模板创建过程的分步教程，请参阅：

> [!div class="nextstepaction"]
> [教程：创建和部署你的第一个 Azure 资源管理器模板](https://docs.microsoft.com/azure/azure-resource-manager/templates/template-tutorial-create-first-template)
