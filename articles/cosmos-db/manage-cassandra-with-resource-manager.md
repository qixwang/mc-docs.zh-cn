---
title: Azure Cosmos DB Cassandra API 的资源管理器模板
description: 使用 Azure 资源管理器模板创建和配置 Azure Cosmos DB Cassandra API。
author: rockboyfor
ms.service: cosmos-db
ms.topic: conceptual
origin.date: 11/12/2019
ms.date: 04/27/2020
ms.author: v-yeche
ms.openlocfilehash: 5cdea6416b282ea1798d79c516ac84822fbc8728
ms.sourcegitcommit: f9c242ce5df12e1cd85471adae52530c4de4c7d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/24/2020
ms.locfileid: "82134618"
---
<!--Verify successfully-->
# <a name="manage-azure-cosmos-db-cassandra-api-resources-using-azure-resource-manager-templates"></a>使用 Azure 资源管理器模板管理 Azure Cosmos DB Cassandra API 资源

本文介绍如何使用 Azure 资源管理器模板执行不同的操作来自动管理 Azure Cosmos DB 帐户、数据库和容器。 本文的示例仅适用于 SQL API 帐户。若要查找其他 API 类型帐户的示例，请参阅将 Azure 资源管理器模板与 Azure Cosmos DB 的适用于 [SQL](manage-sql-with-resource-manager.md)、[Gremlin](manage-gremlin-with-resource-manager.md)、[MongoDB](manage-mongodb-with-resource-manager.md)、[表](manage-table-with-resource-manager.md)的 API 配合使用的相关文章。

## <a name="create-azure-cosmos-account-keyspace-and-table"></a>创建 Azure Cosmos 帐户、密钥空间和表 <a name="create-resource"></a>

使用 Azure 资源管理器模板创建 Azure Cosmos DB 资源。 此模板将创建一个适用于 Cassandra API 的 Azure Cosmos 帐户，所使用的两个表在密钥空间级别共享 400 RU/秒的吞吐量。 复制模板并按如下所示进行部署，或者访问 [Azure 快速入门库](https://github.com/Azure/azure-quickstart-templates/tree/master/101-cosmosdb-cassandra/)，然后从 Azure 门户进行部署。 还可以将模板下载到本地计算机，或者创建新模板并使用 `--template-file` 参数指定本地路径。

> [!NOTE]
> 帐户名称必须为小写且不超过 44 个字符。
> 若要更新 RU/秒，请重新提交包含已更新吞吐量属性值的模板。

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "accountName": {
            "type": "string",
            "defaultValue": "[concat('cassandra-', uniqueString(resourceGroup().id))]",
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
        "keyspaceName": {
            "type": "string",
            "metadata": {
                "description": "The name for the Cassandra Keyspace"
            }
        },
        "table1Name": {
            "type": "string",
            "metadata": {
                "description": "The name for the first Cassandra table"
            }
        },
        "table2Name": {
            "type": "string",
            "metadata": {
                "description": "The name for the second Cassandra table"
            }
        },
        "throughput": {
            "type": "int",
            "defaultValue": 400,
            "minValue": 400,
            "maxValue": 1000000,
            "metadata": {
                "description": "The throughput for both Cassandra tables"
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
                "capabilities": [{ "name": "EnableCassandra" }],
                "consistencyPolicy": "[variables('consistencyPolicy')[parameters('defaultConsistencyLevel')]]",
                "locations": "[variables('locations')]",
                "databaseAccountOfferType": "Standard",
                "enableAutomaticFailover": "[parameters('automaticFailover')]",
                "enableMultipleWriteLocations": "[parameters('multipleWriteLocations')]"
            }
        },
        {
            "type": "Microsoft.DocumentDB/databaseAccounts/cassandraKeyspaces",
            "name": "[concat(variables('accountName'), '/', parameters('keyspaceName'))]",
            "apiVersion": "2019-08-01",
            "dependsOn": [ "[resourceId('Microsoft.DocumentDB/databaseAccounts/', variables('accountName'))]" ],
            "properties":{
                "resource":{
                    "id": "[parameters('keyspaceName')]"
                }
            }
        },
        {
            "type": "Microsoft.DocumentDb/databaseAccounts/cassandraKeyspaces/tables",
            "name": "[concat(variables('accountName'), '/', parameters('keyspaceName'), '/', parameters('table1Name'))]",
            "apiVersion": "2019-08-01",
            "dependsOn": [ "[resourceId('Microsoft.DocumentDB/databaseAccounts/cassandraKeyspaces', variables('accountName'), parameters('keyspaceName'))]" ],
            "properties":
            {
                "resource":{
                    "id":  "[parameters('table1Name')]",
                    "schema": {
                        "columns": [
                            { "name": "userid", "type": "uuid" },
                            { "name": "posted_month", "type": "int" },
                            { "name": "posted_time", "type": "uuid" },
                            { "name": "body", "type": "text" },
                            { "name": "posted_by", "type": "text" }
                        ],
                        "partitionKeys": [ 
                            { "name": "userid" }, 
                            { "name": "posted_month" }, 
                            { "name": "posted_time" } 
                        ]
                    },
                    "options": { "throughput": "[parameters('throughput')]" }
                }
            }
        },
        {
            "type": "Microsoft.DocumentDb/databaseAccounts/cassandraKeyspaces/tables",
            "name": "[concat(variables('accountName'), '/', parameters('keyspaceName'), '/', parameters('table2Name'))]",
            "apiVersion": "2019-08-01",
            "dependsOn": [ "[resourceId('Microsoft.DocumentDB/databaseAccounts/cassandraKeyspaces', variables('accountName'), parameters('keyspaceName'))]" ],
            "properties":
            {
                "resource":{
                    "id":  "[parameters('table2Name')]",
                    "schema": {
                        "columns": [
                            { "name": "loadid", "type": "uuid" },
                            { "name": "machine", "type": "uuid" },
                            { "name": "cpu", "type": "int" },
                            { "name": "mtime", "type": "int" },
                            { "name": "load", "type": "float" }
                        ],
                        "partitionKeys": [ 
                            { "name": "machine" }, 
                            { "name": "cpu" }, 
                            { "name": "mtime" } 
                        ],
                        "clusterKeys": [ 
                            { "name": "loadid", "orderBy": "asc" } 
                        ]
                    },
                    "options": { "throughput": "[parameters('throughput')]" }
                }
            }
        }
    ]
}

```

## <a name="deploy-with-the-azure-cli"></a>使用 Azure CLI 进行部署

使用以下脚本通过 Azure Local CLI 部署资源管理器模板。

[!INCLUDE [azure-cli-2-azurechinacloud-environment-parameter](../../includes/azure-cli-2-azurechinacloud-environment-parameter.md)]

<!--Not Available on **Copy** the script and select **Try it** to open the Azure Cloud shell. To paste the script, right-click the shell, and then select **Paste**:-->

<!--MOONCAKE: parameter correct on keyspaceName-->
<!--MOONCAKE: parameter correct on --name $accountName-->

```azurecli

read -p 'Enter the Resource Group name: ' resourceGroupName
read -p 'Enter the location (i.e. chinanorth2): ' location
read -p 'Enter the account name: ' accountName
read -p 'Enter the primary region (i.e. chinanorth2): ' primaryRegion
read -p 'Enter the secondary region (i.e. chinaeast2): ' secondaryRegion
read -p 'Enter the keyset name: ' keyspaceName
read -p 'Enter the first table name: ' table1Name
read -p 'Enter the second table name: ' table2Name

az group create --name $resourceGroupName --location $location
az group deployment create --resource-group $resourceGroupName \
   --template-uri https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/101-cosmosdb-cassandra/azuredeploy.json \
   --parameters accountName=$accountName primaryRegion=$primaryRegion secondaryRegion=$secondaryRegion keyspaceName=$keyspaceName \
   table1Name=$table1Name table2Name=$table2Name

az cosmosdb show --resource-group $resourceGroupName --name $accountName --output tsv
```

<!--MOONCAKE: parameter correct on keyspaceName-->
<!--MOONCAKE: parameter correct on --name $accountName-->

`az cosmosdb show` 命令显示预配后的新建 Azure Cosmos 帐户。

<!--Not Available on  If you choose to use a locally installed version of Azure CLI instead of using CloudShell, see [Azure Command-Line Interface (CLI)](https://docs.azure.cn/cli/?view=azure-cli-latest) article.-->

## <a name="next-steps"></a>后续步骤

下面是一些其他资源：

- [Azure 资源管理器文档](/azure-resource-manager/)

    <!--Not Available on [Azure Cosmos DB resource provider schema](https://docs.microsoft.com/azure/templates/microsoft.documentdb/allversions)-->
    
- [Azure Cosmos DB 快速入门模板](https://github.com/Azure/azure-quickstart-templates/?resourceType=Microsoft.DocumentDB&pageNumber=1&sort=Popular)
- [排查常见的 Azure 资源管理器部署错误](../azure-resource-manager/templates/common-deployment-errors.md)

<!-- Update_Description: update meta properties, wording update, update link -->