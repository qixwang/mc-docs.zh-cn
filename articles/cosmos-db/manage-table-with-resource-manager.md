---
title: Azure Cosmos DB 表 API 的资源管理器模板
description: 使用 Azure 资源管理器模板创建和配置 Azure Cosmos DB 表 API。
author: rockboyfor
ms.service: cosmos-db
ms.topic: conceptual
origin.date: 11/12/2019
ms.date: 02/10/2020
ms.author: v-yeche
ms.openlocfilehash: f06adce040460f327e2f6b1133fb4141ae435d9b
ms.sourcegitcommit: 3c98f52b6ccca469e598d327cd537caab2fde83f
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/13/2020
ms.locfileid: "79293397"
---
<!--Verify successfully-->
# <a name="manage-azure-cosmos-db-table-api-resources-using-azure-resource-manager-templates"></a>使用 Azure 资源管理器模板管理 Azure Cosmos DB 表 API 资源

本文介绍如何使用 Azure 资源管理器模板执行不同的操作来自动管理 Azure Cosmos DB 帐户、数据库和容器。 本文的示例仅适用于表 API 帐户。若要查找其他 API 类型帐户的示例，请参阅将 Azure 资源管理器模板与 Azure Cosmos DB 的适用于 [Cassandra](manage-cassandra-with-resource-manager.md)、[Gremlin](manage-gremlin-with-resource-manager.md)、[MongoDB](manage-mongodb-with-resource-manager.md)、[SQL](manage-sql-with-resource-manager.md) 的 API 配合使用的相关文章。

<a name="create-resource"></a>
## <a name="create-azure-cosmos-account-and-table"></a>创建 Azure Cosmos 帐户和表 

使用 Azure 资源管理器模板创建 Azure Cosmos DB 资源。 此模板将创建一个适用于表 API 的 Azure Cosmos 帐户，所使用的一个表的吞吐量为 400 RU/秒。 复制模板并按如下所示进行部署，或者访问 [Azure 快速入门库](https://github.com/Azure/azure-quickstart-templates/tree/master/101-cosmosdb-table/)，然后从 Azure 门户进行部署。 还可以将模板下载到本地计算机，或者创建新模板并使用 `--template-file` 参数指定本地路径。

> [!NOTE]
> 帐户名称必须为小写且不超过 44 个字符。
> 若要更新 RU/秒，请使用更新的吞吐量属性值重新提交模板。

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "accountName": {
            "type": "string",
            "defaultValue": "[concat('table-', uniqueString(resourceGroup().id))]",
            "metadata": {
                "description": "Cosmos DB account name"
            }
        },
        "location": {
            "type": "string",
            "defaultValue": "[resourceGroup().location]",
            "metadata": {
                "description": "Location for the Cosmos DB account."
            }
        },
        "primaryRegion":{
            "type":"string",
            "metadata": {
                "description": "The primary replica region for the Cosmos DB account."
            }
        },
        "secondaryRegion":{
            "type":"string",
            "metadata": {
              "description": "The secondary replica region for the Cosmos DB account."
          }
        },
        "defaultConsistencyLevel": {
            "type": "string",
            "defaultValue": "Session",
            "allowedValues": [ "Eventual", "ConsistentPrefix", "Session", "BoundedStaleness", "Strong" ],
            "metadata": {
                "description": "The default consistency level of the Cosmos DB account."
            }
        },
        "maxStalenessPrefix": {
            "type": "int",
            "defaultValue": 100000,
            "minValue": 10,
            "maxValue": 2147483647,
            "metadata": {
                "description": "Max stale requests. Required for BoundedStaleness. Valid ranges, Single Region: 10 to 1000000. Multi Region: 100000 to 1000000."
            }
        },
        "maxIntervalInSeconds": {
            "type": "int",
            "defaultValue": 300,
            "minValue": 5,
            "maxValue": 86400,
            "metadata": {
                "description": "Max lag time (seconds). Required for BoundedStaleness. Valid ranges, Single Region: 5 to 84600. Multi Region: 300 to 86400."
            }
        },  
        "multipleWriteLocations": {
            "type": "bool",
            "defaultValue": false,
            "allowedValues": [ true, false ],
            "metadata": {
                "description": "Enable multi-master to make all regions writable."
            }
        },
        "automaticFailover": {
            "type": "bool",
            "defaultValue": false,
            "allowedValues": [ true, false ],
            "metadata": {
                "description": "Enable automatic failover for regions. Ignored when Multi-Master is enabled"
            }
        },
        "tableName": {
            "type": "string",
            "metadata": {
                "description": "The name for the table"
            }
        },
        "throughput": {
            "type": "int",
            "defaultValue": 400,
            "minValue": 400,
            "maxValue": 1000000,
            "metadata": {
                "description": "The throughput for the table"
            }
        }
    },
    "variables": {
        "accountName": "[toLower(parameters('accountName'))]",
        "consistencyPolicy": {
            "Eventual": {
                "defaultConsistencyLevel": "Eventual"
            },
            "ConsistentPrefix": {
                "defaultConsistencyLevel": "ConsistentPrefix"
            },
            "Session": {
                "defaultConsistencyLevel": "Session"
            },
            "BoundedStaleness": {
                "defaultConsistencyLevel": "BoundedStaleness",
                "maxStalenessPrefix": "[parameters('maxStalenessPrefix')]",
                "maxIntervalInSeconds": "[parameters('maxIntervalInSeconds')]"
            },
            "Strong": {
                "defaultConsistencyLevel": "Strong"
            }
        },
        "locations": 
        [ 
            {
                "locationName": "[parameters('primaryRegion')]",
                "failoverPriority": 0,
                "isZoneRedundant": false
            }, 
            {
                "locationName": "[parameters('secondaryRegion')]",
                "failoverPriority": 1,
                "isZoneRedundant": false
            }
        ]
    },
    "resources": 
    [
        {
            "type": "Microsoft.DocumentDB/databaseAccounts",
            "name": "[variables('accountName')]",
            "apiVersion": "2019-08-01",
            "location": "[parameters('location')]",
            "kind": "GlobalDocumentDB",
            "properties": {
                "capabilities": [{ "name": "EnableTable" }],
                "consistencyPolicy": "[variables('consistencyPolicy')[parameters('defaultConsistencyLevel')]]",
                "locations": "[variables('locations')]",
                "databaseAccountOfferType": "Standard",
                "enableAutomaticFailover": "[parameters('automaticFailover')]",
                "enableMultipleWriteLocations": "[parameters('multipleWriteLocations')]"
            }
        },
        {
            "type": "Microsoft.DocumentDB/databaseAccounts/tables",
            "name": "[concat(variables('accountName'), '/', parameters('tableName'))]",
            "apiVersion": "2019-08-01",
            "dependsOn": [ "[resourceId('Microsoft.DocumentDB/databaseAccounts/', variables('accountName'))]" ],
            "properties":{
                "resource":{
                    "id": "[parameters('tableName')]"
                },
                "options": { "throughput": "[parameters('throughput')]" }
            }
        }
    ]
}

```

### <a name="deploy-via-powershell"></a>通过 PowerShell 部署

使用以下脚本通过 PowerShell 部署资源管理器模板。

<!--Not Available on  **Copy** the script and select **Try it** to open the Azure Cloud shell. To paste the script, right-click the shell, and then select **Paste**:-->

```powershell
Connect-AzAccount -Environment AzureChinaCloud

$resourceGroupName = Read-Host -Prompt "Enter the Resource Group name"
$accountName = Read-Host -Prompt "Enter the account name"
$location = Read-Host -Prompt "Enter the location (i.e. chinanorth2)"
$primaryRegion = Read-Host -Prompt "Enter the primary region (i.e. chinanorth2)"
$secondaryRegion = Read-Host -Prompt "Enter the secondary region (i.e. chinaeast2)"
$tableName = Read-Host -Prompt "Enter the table name"

New-AzResourceGroup -Name $resourceGroupName -Location $location
New-AzResourceGroupDeployment `
    -ResourceGroupName $resourceGroupName `
    -TemplateUri "https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-cosmosdb-table/azuredeploy.json" `
    -primaryRegion $primaryRegion `
    -secondaryRegion $secondaryRegion `
    -tableName $tableName

(Get-AzResource -ResourceType "Microsoft.DocumentDb/databaseAccounts" -ApiVersion "2015-04-08" -ResourceGroupName $resourceGroupName).name
```

<!--MOONCAKE: parameter correct on -ResourceType-->

如果选择使用本地安装的 PowerShell 版本，则需[安装](https://docs.microsoft.com/powershell/azure/install-az-ps) Azure PowerShell 模块。 运行 `Get-Module -ListAvailable Az` 即可查找版本。

<!--MOONCAKE: Not available on instead of using CloudShell-->

### <a name="deploy-via-the-azure-cli"></a>通过 Azure CLI 部署

使用以下脚本通过 Azure CLI 部署资源管理器模板。

[!INCLUDE [azure-cli-2-azurechinacloud-environment-parameter](../../includes/azure-cli-2-azurechinacloud-environment-parameter.md)]

<!--Not Available on **Copy** the script and select **Try it** to open the Azure Cloud shell. To paste the script, right-click the shell, and then select **Paste**:-->

<!--MOONCAKE: parameter correct on --name $accountName-->

```azurecli
read -p 'Enter the Resource Group name: ' resourceGroupName
read -p 'Enter the location (i.e. chinanorth2): ' location
read -p 'Enter the account name: ' accountName
read -p 'Enter the primary region (i.e. chinanorth2): ' primaryRegion
read -p 'Enter the secondary region (i.e. chinaeast2): ' secondaryRegion
read -p 'Enter the table name: ' tableName

az group create --name $resourceGroupName --location $location
az group deployment create --resource-group $resourceGroupName \
   --template-uri https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/101-cosmosdb-table/azuredeploy.json \
   --parameters accountName=$accountName primaryRegion=$primaryRegion secondaryRegion=$secondaryRegion tableName=$tableName

az cosmosdb show --resource-group $resourceGroupName --name $accountName --output tsv
```
<!--MOONCAKE: parameter correct on --name $accountName-->

`az cosmosdb show` 命令显示预配后的新建 Azure Cosmos 帐户。 如果选择使用本地安装的 Azure CLI 版本，请参阅 [Azure CLI](https://docs.azure.cn/cli/?view=azure-cli-latest) 一文。

<!--Not Available on instead of using CloudShell -->

## <a name="next-steps"></a>后续步骤

下面是一些其他资源：

- [Azure 资源管理器文档](/azure-resource-manager/)
    
    <!--Not Available on  - [Azure Cosmos DB resource provider schema](https://docs.microsoft.com/azure/templates/microsoft.documentdb/allversions)-->
    
- [Azure Cosmos DB 快速入门模板](https://github.com/Azure/azure-quickstart-templates/?resourceType=Microsoft.DocumentDB&pageNumber=1&sort=Popular)
- [排查常见的 Azure 资源管理器部署错误](../azure-resource-manager/templates/common-deployment-errors.md)

<!-- Update_Description: update meta properties, wording update, update link -->