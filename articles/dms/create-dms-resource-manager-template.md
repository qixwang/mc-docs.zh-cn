---
title: 创建 DMS 实例（Azure 资源管理器模板）
description: 了解如何使用 Azure 资源管理器模板创建数据库迁移服务。
author: WenJason
ms.topic: quickstart
ms.custom: subject-armqs
ms.author: v-jay
origin.date: 06/29/2020
ms.date: 07/20/2020
ms.service: dms
ms.openlocfilehash: 8a50083254d14e287fb7c6f0d95b5d77bee0ae5e
ms.sourcegitcommit: 403db9004b6e9390f7fd1afddd9e164e5d9cce6a
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/17/2020
ms.locfileid: "86440591"
---
# <a name="create-instance-of-azure-database-migration-service-azure-resource-manager-template"></a>创建 Azure 数据库迁移服务实例（Azure 资源管理器模板）

使用此 Azure 资源管理器模板部署 Azure 数据库迁移服务实例。 

[!INCLUDE [About Azure Resource Manager](../../includes/resource-manager-quickstart-introduction.md)]

## <a name="prerequisites"></a>先决条件

Azure 数据库迁移服务 ARM 模板需要以下各项： 

- 最新版本的 [Azure CLI](/cli/install-azure-cli?view=azure-cli-latest) 和/或 [PowerShell](https://docs.microsoft.com/powershell/scripting/install/installing-powershell?view=powershell-7)。 
- Azure 订阅。 如果没有订阅，请在开始之前创建一个[试用帐户](https://wd.azure.cn/pricing/1rmb-trial-full)。

## <a name="review-the-template"></a>查看模板

本快速入门中使用的模板来自 [Azure 快速启动模板](https://azure.microsoft.com/resources/templates/101-azure-database-migration-simple-deploy/)。

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "serviceName": {
            "type": "string",
            "metadata": {
                "description": "Name of the new migration service."
            }
        },
        "location": {
            "type": "string",
            "defaultValue": "[resourceGroup().location]",
            "metadata": {
                "description": "Location where the resources will be deployed."
            }
        },
        "vnetName": {
            "type": "string",
            "metadata": {
                "description": "Name of the new virtual network."
            }
        },
        "subnetName": {
            "type": "string",
            "metadata": {
                "description": "Name of the new subnet associated with the virtual network."
            }
        }
    },
    "variables": {
    },
    "resources": [
        {
            "type": "Microsoft.Network/virtualNetworks",
            "apiVersion": "2020-04-01",
            "name": "[parameters('vnetName')]",
            "location": "[parameters('location')]",
            "properties": {
                "addressSpace": {
                    "addressPrefixes": [
                        "10.0.0.0/16"
                    ]
                }
            }
        },
        {
            "type": "Microsoft.Network/virtualNetworks/subnets",
            "apiVersion": "2020-04-01",
            "name": "[concat(parameters('vnetName'), '/', parameters('subnetName'))]",
            "dependsOn": [
                "[parameters('vnetName')]"
            ],
            "properties": {
                "addressPrefix": "10.0.0.0/24"
            }
        },
        {
            "type": "Microsoft.DataMigration/services",
            "apiVersion": "2018-07-15-preview",
            "name": "[parameters('serviceName')]",
            "location": "[parameters('location')]",
            "sku": {
                "tier": "Standard",
                "size": "1 vCores",
                "name": "Standard_1vCores"
            },
            "dependsOn": [
                "[resourceId('Microsoft.Network/virtualNetworks/subnets', parameters('vnetName'), parameters('subnetName'))]"
            ],
            "properties": {
                "virtualSubnetId": "[resourceId('Microsoft.Network/virtualNetworks/subnets', parameters('vnetName'), parameters('subnetName'))]"
            }
        }
    ],
    "outputs": {
    }
}
```

该模板中定义了三个 Azure 资源： 

- [Microsoft.Network/virtualNetworks](https://docs.microsoft.com/azure/templates/microsoft.network/virtualnetworks)：创建虚拟网络。 
- [Microsoft.Network/virtualNetworks/subnets](https://docs.microsoft.com/azure/templates/microsoft.network/virtualnetworks/subnets)：创建子网。 
- [Microsoft.DataMigration/services](https://docs.microsoft.com/azure/templates/microsoft.datamigration/services)：部署 Azure 数据库迁移服务实例。 

可以在[快速入门模板库](https://azure.microsoft.com/resources/templates/?resourceType=Microsoft.Datamigration)中找到更多 Azure 数据库迁移服务模板。


## <a name="deploy-the-template"></a>部署模板

1. 选择下图登录到 Azure 并打开一个模板。 使用该模板创建 Azure 数据库迁移服务实例。 

   [![“部署到 Azure”](../media/template-deployments/deploy-to-azure.svg)](https://portal.azure.cn/#create/Microsoft.Template/uri/https%3a%2f%2fraw.githubusercontent.com%2fAzure%2fazure-quickstart-templates%2fmaster%2f101-azure-database-migration-simple-deploy%2fazuredeploy.json)

2. 选择或输入以下值。

    * **订阅**：选择 Azure 订阅。
    * **资源组**：从下拉列表中选择现有资源组，或者选择“新建”来创建新的资源组。 
    * **区域**：将在其中部署资源的位置。
    * **服务名称**：新迁移服务的名称。
    * **位置**：资源组的位置，保留为默认值 `[resourceGroup().location]`。
    * **Vnet 名称**：新虚拟网络的名称。
    * **子网名称**：与虚拟网络关联的新子网的名称。



3. 选择“查看 + 创建”。 成功部署 Azure 数据库迁移服务实例后，你会收到通知。 


使用 Azure 门户部署模板。 除了 Azure 门户，还可以使用 Azure PowerShell、Azure CLI 和 REST API。 若要了解其他部署方法，请参阅[部署模板](../azure-resource-manager/templates/deploy-powershell.md)。

## <a name="review-deployed-resources"></a>查看已部署的资源

可以使用 Azure CLI 查看已部署的资源。 


```azurecli
echo "Enter the resource group where your SQL Server VM exists:" &&
read resourcegroupName &&
az resource list --resource-group $resourcegroupName 
```


## <a name="clean-up-resources"></a>清理资源

不再需要时，可使用 Azure CLI 或 Azure PowerShell 删除资源组：

# <a name="cli"></a>[CLI](#tab/CLI)

```azurecli
echo "Enter the Resource Group name:" &&
read resourceGroupName &&
az group delete --name $resourceGroupName &&
echo "Press [ENTER] to continue ..."
```

# <a name="powershell"></a>[PowerShell](#tab/PowerShell)

```azurepowershell
$resourceGroupName = Read-Host -Prompt "Enter the Resource Group name"
Remove-AzResourceGroup -Name $resourceGroupName
Write-Host "Press [ENTER] to continue..."
```

---

## <a name="next-steps"></a>后续步骤

有关引导你完成模板创建过程的分步教程，请参阅：

> [!div class="nextstepaction"]
> [教程：创建和部署你的第一个 Azure 资源管理器模板](https://docs.microsoft.com/azure/azure-resource-manager/templates/template-tutorial-create-first-template)

有关部署 Azure 数据库迁移服务的其他方式，请参阅： 
- [Azure 门户](quickstart-create-data-migration-service-portal.md)

若要了解详细信息，请参阅 [Azure 数据库迁移服务概述](dms-overview.md)