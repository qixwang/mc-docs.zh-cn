---
title: 使用资源管理器模板创建和管理 Azure Cosmos DB
description: 使用 Azure 资源管理器模板创建和配置 Azure Cosmos DB for SQL (Core) API
author: rockboyfor
ms.service: cosmos-db
ms.topic: conceptual
origin.date: 11/12/2019
ms.date: 02/10/2020
ms.author: v-yeche
ms.openlocfilehash: c17d945048e187ea56b392273b21172d83d879c4
ms.sourcegitcommit: 5c4141f30975f504afc85299e70dfa2abd92bea1
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/05/2020
ms.locfileid: "77028927"
---
<!--Verify successfully-->
# <a name="manage-azure-cosmos-db-sql-core-api-resources-with-azure-resource-manager-templates"></a>使用 Azure 资源管理器模板管理 Azure Cosmos DB SQL (Core) API 资源

在本文中，你将了解如何使用 Azure 资源管理器模板来帮助自动管理 Azure Cosmos DB 帐户、数据库和容器。

本文仅展示了适用于 SQL API 帐户的 Azure 资源管理器模板示例。 你还可以查找适用于 [Cassandra](manage-cassandra-with-resource-manager.md)、[Gremlin](manage-gremlin-with-resource-manager.md)、[MongoDB](manage-mongodb-with-resource-manager.md) 和[表](manage-table-with-resource-manager.md) API 的模板示例。

<a name="create-resource"></a>

## <a name="create-an-azure-cosmos-account-database-and-container"></a>创建 Azure Cosmos 帐户、数据库和容器

下面的 Azure 资源管理器模板创建包含以下项的 Azure Cosmos 帐户：

* 两个在数据库级别共享 400 请求单位/秒 (RU/s) 吞吐量的容器。
* 一个具有专用 400 RU/s 吞吐量的容器。

若要创建 Azure Cosmos DB 资源，请复制下面的示例模板，并按照说明通过 [PowerShell](#deploy-via-powershell) 或 [Azure CLI](#deploy-via-azure-cli) 进行部署。

* 还可以访问 [Azure 快速入门库](https://github.com/Azure/azure-quickstart-templates/tree/master/101-cosmosdb-sql/)，然后从 Azure 门户部署模板。
* 还可以将模板下载到本地计算机，或者创建新模板并使用 `--template-file` 参数指定本地路径。

> [!IMPORTANT]
>
> * 在 Azure Cosmos 帐户中添加或删除位置时，不能同时修改其他属性。 这些操作必须单独执行。
> * 帐户名称限制为 44 个字符，全部小写。
> * 若要更改吞吐量值，请使用更新后的 RU/s 重新提交模板。

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "accountName": {
            "type": "string",
            "defaultValue": "[concat('sql-', uniqueString(resourceGroup().id))]",
            "metadata": {
                "description": "Cosmos DB account name, max length 44 characters"
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
            "minValue": 10,
            "defaultValue": 100000,
            "maxValue": 2147483647,
            "metadata": {
                "description": "Max stale requests. Required for BoundedStaleness. Valid ranges, Single Region: 10 to 1000000. Multi Region: 100000 to 1000000."
            }
        },
        "maxIntervalInSeconds": {
            "type": "int",
            "minValue": 5,
            "defaultValue": 300,
            "maxValue": 86400,
            "metadata": {
                "description": "Max lag time (minutes). Required for BoundedStaleness. Valid ranges, Single Region: 5 to 84600. Multi Region: 300 to 86400."
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
        "databaseName": {
            "type": "string",
            "metadata": {
                "description": "The name for the SQL database"
            }
        },
        "sharedThroughput": {
            "type": "int",
            "defaultValue": 400,
            "minValue": 400,
            "maxValue": 1000000,
            "metadata": {
                "description": "The throughput for the database to be shared"
            }           
        },
        "sharedContainer1Name": {
            "type": "string",
            "defaultValue": "sharedContainer1",
            "metadata": {
                "description": "The name for the first container with shared throughput"
            }
        },
        "sharedContainer2Name": {
            "type": "string",
            "defaultValue": "sharedContainer2",
            "metadata": {
                "description": "The name for the second container with shared throughput"
            }
        },
        "dedicatedContainer1Name": {
            "type": "string",
            "defaultValue": "dedicatedContainer1",
            "metadata": {
                "description": "The name for the container with dedicated throughput"
            }
        },
        "dedicatedThroughput": {
            "type": "int",
            "defaultValue": 400,
            "minValue": 400,
            "maxValue": 1000000,
            "metadata": {
                "description": "The throughput for the container with dedicated throughput"
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
            "kind": "GlobalDocumentDB",
            "location": "[parameters('location')]",
            "properties": {
                "consistencyPolicy": "[variables('consistencyPolicy')[parameters('defaultConsistencyLevel')]]",
                "locations": "[variables('locations')]",
                "databaseAccountOfferType": "Standard",
                "enableAutomaticFailover": "[parameters('automaticFailover')]",
                "enableMultipleWriteLocations": "[parameters('multipleWriteLocations')]"
            }
        },
        {
            "type": "Microsoft.DocumentDB/databaseAccounts/sqlDatabases",
            "name": "[concat(variables('accountName'), '/', parameters('databaseName'))]",
            "apiVersion": "2019-08-01",
            "dependsOn": [ "[resourceId('Microsoft.DocumentDB/databaseAccounts', variables('accountName'))]" ],
            "properties":{
                "resource":{
                    "id": "[parameters('databaseName')]"
                },
                "options": { "throughput": "[parameters('sharedThroughput')]" }
            }
        },
        {
            "type": "Microsoft.DocumentDB/databaseAccounts/sqlDatabases/containers",
            "name": "[concat(variables('accountName'), '/', parameters('databaseName'), '/', parameters('sharedContainer1Name'))]",
            "apiVersion": "2019-08-01",
            "dependsOn": [ "[resourceId('Microsoft.DocumentDB/databaseAccounts/sqlDatabases', variables('accountName'), parameters('databaseName'))]" ],
            "properties":
            {
                "resource":{
                    "id":  "[parameters('sharedContainer1Name')]",
                    "partitionKey": {
                        "paths": [
                        "/myPartitionKey"
                        ],
                        "kind": "Hash"
                    },
                    "indexingPolicy": {
                        "indexingMode": "consistent",
                        "includedPaths": [{
                                "path": "/*"
                            }
                        ],
                        "excludedPaths": [{
                                "path": "/myPathToNotIndex/*"
                            }
                        ]
                    }
                }
            }
        },
        {
            "type": "Microsoft.DocumentDB/databaseAccounts/sqlDatabases/containers",
            "name": "[concat(variables('accountName'), '/', parameters('databaseName'), '/', parameters('sharedContainer2Name'))]",
            "apiVersion": "2019-08-01",
            "dependsOn": [ "[resourceId('Microsoft.DocumentDB/databaseAccounts/sqlDatabases', variables('accountName'), parameters('databaseName'))]" ],
            "properties":
            {
                "resource":{
                    "id":  "[parameters('sharedContainer2Name')]",
                    "partitionKey": {
                        "paths": [
                        "/myPartitionKey"
                        ],
                        "kind": "Hash"
                    },
                    "indexingPolicy": {
                        "indexingMode": "consistent",
                        "includedPaths": [{
                                "path": "/*"
                            }
                        ],
                        "excludedPaths": [{
                                "path": "/myPathToNotIndex/*"
                            }
                        ]
                    }
                }
            }
        },
        {
            "type": "Microsoft.DocumentDB/databaseAccounts/sqlDatabases/containers",
            "name": "[concat(variables('accountName'), '/', parameters('databaseName'), '/', parameters('dedicatedContainer1Name'))]",
            "apiVersion": "2019-08-01",
            "dependsOn": [ "[resourceId('Microsoft.DocumentDB/databaseAccounts/sqlDatabases', variables('accountName'), parameters('databaseName'))]" ],
            "properties":
            {
                "resource":{
                    "id":  "[parameters('dedicatedContainer1Name')]",
                    "partitionKey": {
                        "paths": [
                        "/myPartitionKey"
                        ],
                        "kind": "Hash"
                    },
                    "indexingPolicy": {
                        "indexingMode": "consistent",
                        "includedPaths": [{
                                "path": "/*"
                            }
                        ],
                        "excludedPaths": [{
                                "path": "/myPathToNotIndex/*"
                            }
                        ],
                        "compositeIndexes":[  
                        [
                            {
                                "path":"/name",
                                "order":"ascending"
                            },
                            {
                                "path":"/age",
                                "order":"descending"
                            }
                        ]
                    ],
                    "spatialIndexes": [
                            {
                                "path": "/path/to/geojson/property/?",
                                "types": [
                                    "Point",
                                    "Polygon",
                                    "MultiPolygon",
                                    "LineString"
                                ]
                            }
                        ]
                    },
                    "defaultTtl": 86400,
                    "uniqueKeyPolicy": {
                        "uniqueKeys": [
                        {
                            "paths": [
                            "/phoneNumber"
                            ]
                        }
                        ]
                    }
                },
                "options": { "throughput": "[parameters('dedicatedThroughput')]" }
            }
        }
    ]
}

```

> [!NOTE]
> 若要使用大的分区键创建容器，请修改前面的模板以在 `partitionKey` 对象内包括 `"version":2` 属性。

### <a name="deploy-via-powershell"></a>通过 PowerShell 部署

若要使用 PowerShell 部署 Azure 资源管理器模板，请执行以下操作：

<!--Not Available on  Select **Try it** to open Azure Cloud Shell.-->

```powershell

$resourceGroupName = Read-Host -Prompt "Enter the Resource Group name"
$accountName = Read-Host -Prompt "Enter the account name"
$location = Read-Host -Prompt "Enter the location (i.e. chinanorth2)"
$primaryRegion = Read-Host -Prompt "Enter the primary region (i.e. chinanorth2)"
$secondaryRegion = Read-Host -Prompt "Enter the secondary region (i.e. chinaeast2)"
$databaseName = Read-Host -Prompt "Enter the database name"
$sharedThroughput = Read-Host -Prompt "Enter the shared database throughput (i.e. 400)"
$sharedContainer1Name = Read-Host -Prompt "Enter the first shared container name"
$sharedContainer2Name = Read-Host -Prompt "Enter the second shared container name"
$dedicatedContainer1Name = Read-Host -Prompt "Enter the dedicated container name"
$dedicatedThroughput = Read-Host -Prompt "Enter the dedicated container throughput (i.e. 400)"

New-AzResourceGroup -Name $resourceGroupName -Location $location
New-AzResourceGroupDeployment `
    -ResourceGroupName $resourceGroupName `
    -TemplateUri "https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-cosmosdb-sql/azuredeploy.json" `
    -accountName $accountName `
    -location $location `
    -primaryRegion $primaryRegion `
    -secondaryRegion $secondaryRegion `
    -databaseName $databaseName `
    -sharedThroughput $ $sharedThroughput `
    -sharedContainer1Name $sharedContainer1Name `
    -sharedContainer2Name $sharedContainer2Name `
    -dedicatedContainer1Name $dedicatedContainer1Name `
    -dedicatedThroughput $dedicatedThroughput

 (Get-AzResource -ResourceType "Microsoft.DocumentDb/databaseAccounts" -ApiVersion "2019-08-01" -ResourceGroupName $resourceGroupName).name
```

<!--CORRECT ON POWERSHELL CONTAINS ONE DASH(-) IN CMDLET-->

你可以选择使用本地安装的 PowerShell 版本来部署模板。 你需要[安装 Azure PowerShell 模块](https://docs.microsoft.com/powershell/azure/install-az-ps)。 请运行 `Get-Module -ListAvailable Az` 来查找所需的版本。

<!--Not Available on Azure Cloud Shell-->

### <a name="deploy-via-azure-cli"></a>通过 Azure CLI 部署

若要使用 Azure CLI 部署 Azure 资源管理器模板，请执行以下操作：

<!--Not Avaialble on Cloud Shell of select **Try it** to open Azure local Shell. To paste the script, right-click the shell, and then select **Paste**:-->

```azurecli
read -p 'Enter the Resource Group name: ' resourceGroupName
read -p 'Enter the location (i.e. chinanorth2): ' location
read -p 'Enter the account name: ' accountName
read -p 'Enter the primary region (i.e. chinanorth2): ' primaryRegion
read -p 'Enter the secondary region (i.e. chinaeast2): ' secondaryRegion
read -p 'Enter the database name: ' databaseName
read -p 'Enter the shared database throughput: sharedThroughput
read -p 'Enter the first shared container name: ' sharedContainer1Name
read -p 'Enter the second shared container name: ' sharedContainer2Name
read -p 'Enter the dedicated container name: ' dedicatedContainer1Name
read -p 'Enter the dedicated container throughput: dedicatedThroughput

az group create --name $resourceGroupName --location $location
az group deployment create --resource-group $resourceGroupName \
   --template-uri https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/101-cosmosdb-sql/azuredeploy.json \
   --parameters accountName=$accountName \
   primaryRegion=$primaryRegion \
   secondaryRegion=$secondaryRegion \
   databaseName=$databaseName \
   sharedThroughput=$sharedThroughput \
   sharedContainer1Name=$sharedContainer1Name \
   sharedContainer2Name=$sharedContainer2Name \
   dedicatedContainer1Name=$dedicatedContainer1Name \
   dedicatedThroughput=$dedicatedThroughput

az cosmosdb show --resource-group $resourceGroupName --name accountName --output tsv
```

`az cosmosdb show` 命令显示预配后的新建 Azure Cosmos 帐户。 你可以选择使用本地安装的 Azure CLI 版本来部署模板。 有关详细信息，请参阅 [Azure 命令行接口 (CLI)](https://docs.azure.cn/cli/?view=azure-cli-latest) 一文。

<!--Not Available on instead of using CloudShell-->

<a name="create-sproc"></a>
## <a name="create-an-azure-cosmos-db-container-with-server-side-functionality"></a>使用服务器端功能创建 Azure Cosmos DB 容器

可以使用 Azure 资源管理器模板创建包含存储过程、触发器和用户定义的函数的 Azure Cosmos DB 容器。

复制下面的示例模板，并按照说明通过 [PowerShell](#deploy-with-powershell) 或 [Azure CLI](#deploy-with-azure-cli) 进行部署。

* 还可以访问 [Azure 快速入门库](https://github.com/Azure/azure-quickstart-templates/tree/master/101-cosmosdb-sql-container-sprocs/)，然后从 Azure 门户部署模板。
* 还可以将模板下载到本地计算机，或者创建新模板并使用 `--template-file` 参数指定本地路径。

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "accountName": {
            "type": "string",
            "defaultValue": "[concat('cosmos-', uniqueString(resourceGroup().id))]",
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
            "minValue": 10,
            "defaultValue": 100000,
            "maxValue": 2147483647,
            "metadata": {
                "description": "Max stale requests. Required for BoundedStaleness. Valid ranges, Single Region: 10 to 1000000. Multi Region: 100000 to 1000000."
            }
        },
        "maxIntervalInSeconds": {
            "type": "int",
            "minValue": 5,
            "defaultValue": 300,
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
        "databaseName": {
            "type": "string",
            "metadata": {
                "description": "The name for the Core (SQL) database"
            }
        },
        "containerName": {
            "type": "string",
            "defaultValue": "container1",
            "metadata": {
                "description": "The name for the Core (SQL) API container"
            }
        },
        "throughput": {
            "type": "int",
            "defaultValue": 400,
            "minValue": 400,
            "maxValue": 1000000,
            "metadata": {
                "description": "The throughput for the container"
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
                "consistencyPolicy": "[variables('consistencyPolicy')[parameters('defaultConsistencyLevel')]]",
                "locations": "[variables('locations')]",
                "databaseAccountOfferType": "Standard",
                "enableAutomaticFailover": "[parameters('automaticFailover')]",
                "enableMultipleWriteLocations": "[parameters('multipleWriteLocations')]"
            }
        },
        {
            "type": "Microsoft.DocumentDB/databaseAccounts/sqlDatabases",
            "name": "[concat(variables('accountName'), '/', parameters('databaseName'))]",
            "apiVersion": "2019-08-01",
            "dependsOn": [ "[resourceId('Microsoft.DocumentDB/databaseAccounts', variables('accountName'))]" ],
            "properties":{
                "resource":{
                    "id": "[parameters('databaseName')]"
                }
            }
        },
        {
            "type": "Microsoft.DocumentDB/databaseAccounts/sqlDatabases/containers",
            "name": "[concat(variables('accountName'), '/', parameters('databaseName'), '/', parameters('containerName'))]",
            "apiVersion": "2019-08-01",
            "dependsOn": [ "[resourceId('Microsoft.DocumentDB/databaseAccounts/sqlDatabases', variables('accountName'), parameters('databaseName'))]" ],
            "properties":
            {
                "resource":{
                    "id":  "[parameters('containerName')]",
                    "partitionKey": {
                        "paths": ["/myPartitionKey"],
                        "kind": "Hash"
                    },
                    "indexingPolicy": {
                        "indexingMode": "consistent",
                        "includedPaths": [{ "path": "/*" } ],
                        "excludedPaths": [{ "path": "/myPathToNotIndex/*" } ]
                    }
                },
                "options": { "throughput": "[parameters('throughput')]" }
            },
            "resources":
            [
                {
                    "type": "Microsoft.DocumentDB/databaseAccounts/sqlDatabases/containers/storedProcedures",
                    "name": "[concat(variables('accountName'), '/', parameters('databaseName'), '/', parameters('containerName'), '/myStoredProcedure')]",
                    "apiVersion": "2019-08-01",
                    "dependsOn": [ "[resourceId('Microsoft.DocumentDB/databaseAccounts/sqlDatabases/containers', variables('accountName'), parameters('databaseName'), parameters('containerName'))]" ],
                    "properties": {
                        "resource": {
                        "id": "myStoredProcedure",
                        "body": "function () { var context = getContext(); var response = context.getResponse(); response.setBody('Hello, World'); }"
                        }
                    }
                },
                {
                    "type": "Microsoft.DocumentDB/databaseAccounts/sqlDatabases/containers/triggers",
                    "name": "[concat(variables('accountName'), '/', parameters('databaseName'), '/', parameters('containerName'), '/myPreTrigger')]",
                    "apiVersion": "2019-08-01",
                    "dependsOn": [ "[resourceId('Microsoft.DocumentDB/databaseAccounts/sqlDatabases/containers', variables('accountName'), parameters('databaseName'), parameters('containerName'))]" ],
                    "properties": {
                        "resource": {
                        "id": "myPreTrigger",
                        "triggerType": "Pre",
                        "triggerOperation": "Create",
                        "body": "function validateToDoItemTimestamp(){var context=getContext();var request=context.getRequest();var itemToCreate=request.getBody();if(!('timestamp'in itemToCreate)){var ts=new Date();itemToCreate['timestamp']=ts.getTime();}request.setBody(itemToCreate);}"
                        }
                    }
                },
                {
                    "type": "Microsoft.DocumentDB/databaseAccounts/sqlDatabases/containers/userDefinedFunctions",
                    "name": "[concat(variables('accountName'), '/', parameters('databaseName'), '/', parameters('containerName'), '/myUserDefinedFunction')]",
                    "apiVersion": "2019-08-01",
                    "dependsOn": [ "[resourceId('Microsoft.DocumentDB/databaseAccounts/sqlDatabases/containers', variables('accountName'), parameters('databaseName'), parameters('containerName'))]" ],
                    "properties": {
                        "resource": {
                        "id": "myUserDefinedFunction",
                        "body": "function tax(income){if(income==undefined)throw'no input';if(income<1000)return income*0.1;else if(income<10000)return income*0.2;else return income*0.4;}"
                        }
                    }
                }
            ]
        }
    ]
}

```

### <a name="deploy-with-powershell"></a>使用 PowerShell 进行部署

若要使用 PowerShell 部署 Azure 资源管理器模板，请执行以下操作：

<!--Not Available on  **Copy** the script and select **Try it** to open the Azure Cloud shell. To paste the script, right-click the shell, and then select **Paste**:-->

```powershell
Connect-AzAccount -Environment AzureChinaCloud

$resourceGroupName = Read-Host -Prompt "Enter the Resource Group name"
$accountName = Read-Host -Prompt "Enter the account name"
$location = Read-Host -Prompt "Enter the location (i.e. chinanorth2)"
$primaryRegion = Read-Host -Prompt "Enter the primary region (i.e. chinanorth2)"
$secondaryRegion = Read-Host -Prompt "Enter the secondary region (i.e. chinaeast2)"
$databaseName = Read-Host -Prompt "Enter the database name"
$containerName = Read-Host -Prompt "Enter the container name"

New-AzResourceGroup -Name $resourceGroupName -Location $location
New-AzResourceGroupDeployment `
    -ResourceGroupName $resourceGroupName `
    -TemplateUri "https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-cosmosdb-sql-container-sprocs/azuredeploy.json" `
    -accountName $accountName `
    -location $location `
    -primaryRegion $primaryRegion `
    -secondaryRegion $secondaryRegion `
    -databaseName $databaseName `
    -containerName $containerName

 (Get-AzResource -ResourceType "Microsoft.DocumentDb/databaseAccounts" -ApiVersion "2019-08-01" -ResourceGroupName $resourceGroupName).name
```

你可以选择使用本地安装的 PowerShell 版本来部署模板。 你需要[安装 Azure PowerShell 模块](https://docs.microsoft.com/powershell/azure/install-az-ps)。 请运行 `Get-Module -ListAvailable Az` 来查找所需的版本。

<!--MOONCAKE: Not available on instead of using CloudShell-->

### <a name="deploy-with-azure-cli"></a>使用 Azure CLI 进行部署

若要使用 Azure CLI 部署 Azure 资源管理器模板，请执行以下操作：


<!--Not Available on **Copy** the script and select **Try it** to open the Azure Cloud shell. To paste the script, right-click the shell, and then select **Paste**:-->

[!INCLUDE [azure-cli-2-azurechinacloud-environment-parameter](../../includes/azure-cli-2-azurechinacloud-environment-parameter.md)]

<!--MOONCAKE: parameter correct on --name $accountName-->

```azurecli
read -p 'Enter the Resource Group name: ' resourceGroupName
read -p 'Enter the location (i.e. chinanorth2): ' location
read -p 'Enter the account name: ' accountName
read -p 'Enter the primary region (i.e. chinanorth2): ' primaryRegion
read -p 'Enter the secondary region (i.e. chinaeast2): ' secondaryRegion
read -p 'Enter the database name: ' databaseName
read -p 'Enter the container name: ' containerName

az group create --name $resourceGroupName --location $location
az group deployment create --resource-group $resourceGroupName \
   --template-uri https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/101-cosmosdb-sql-container-sprocs/azuredeploy.json \
   --parameters accountName=$accountName primaryRegion=$primaryRegion secondaryRegion=$secondaryRegion databaseName=$databaseName \
   containerName=$containerName

az cosmosdb show --resource-group $resourceGroupName --name $accountName --output tsv
```

<!--MOONCAKE: parameter correct on --name $accountName-->

## <a name="next-steps"></a>后续步骤

下面是一些其他资源：

* [Azure 资源管理器文档](/azure-resource-manager/)
    
    <!--Not Available on  - [Azure Cosmos DB resource provider schema](https://docs.microsoft.com/azure/templates/microsoft.documentdb/allversions)-->
    
* [Azure Cosmos DB 快速入门模板](https://github.com/Azure/azure-quickstart-templates/?resourceType=Microsoft.Documentdb&pageNumber=1&sort=Popular)
* [排查常见的 Azure 资源管理器部署错误](../azure-resource-manager/templates/common-deployment-errors.md)

<!-- Update_Description: update meta properties, wording update, update link -->