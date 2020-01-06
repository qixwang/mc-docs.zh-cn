---
author: lugoldbemicrosoft
ms.service: data-explorer
ms.topic: include
origin.date: 10/23/2019
ms.date: 01/13/2020
ms.author: v-tawe
ms.openlocfilehash: 42d748565fb49d1fb1581ad938bc378e81e3422a
ms.sourcegitcommit: 6fb55092f9e99cf7b27324c61f5fab7f579c37dc
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/03/2020
ms.locfileid: "75630819"
---
## <a name="azure-resource-manager-template"></a>Azure Resource Manager 模板

在本文中，你将使用 Azure 资源管理器模板来创建资源组、存储帐户和容器、事件中心以及 Azure 数据资源管理器群集和数据库。 将以下内容保存在名为 `template.json` 的文件中。 使用此文件来运行代码示例。

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "eventHubNamespaceName": {
            "type": "string",
            "metadata": {
                "description": "Specifies a the event hub Namespace name."
            }
        },
        "eventHubName": {
            "type": "string",
            "metadata": {
                "description": "Specifies a event hub name."
            }
        },
        "storageAccountType": {
            "type": "string",
            "defaultValue": "Standard_LRS",
            "allowedValues": ["Standard_LRS", "Standard_GRS", "Standard_ZRS", "Premium_LRS"],
            "metadata": {
                "description": "Storage Account type"
            }
        },
        "storageAccountName": {
            "type": "string",
            "defaultValue": "[concat('storage', uniqueString(resourceGroup().id))]",
            "metadata": {
                "description": "Name of the storage account to create"
            }
        },
        "containerName": {
            "type": "string",
            "defaultValue": "[concat('storagecontainer', uniqueString(resourceGroup().id))]",
            "metadata": {
                "description": "Name of the container in storage account to create"
            }
        },
        "eventHubSku": {
            "type": "string",
            "allowedValues": ["Basic", "Standard"],
            "defaultValue": "Standard",
            "metadata": {
                "description": "Specifies the messaging tier for service Bus namespace."
            }
        },
        "kustoClusterName": {
            "type": "string",
            "defaultValue": "[concat('kusto', uniqueString(resourceGroup().id))]",
            "metadata": {
                "description": "Name of the cluster to create"
            }
        },
        "kustoDatabaseName": {
            "type": "string",
            "defaultValue": "kustodb",
            "metadata": {
                "description": "Name of the database to create"
            }
        },
        "location": {
            "type": "string",
            "defaultValue": "[resourceGroup().location]",
            "metadata": {
                "description": "Location for all resources."
            }
        }
    },
    "variables": {
    },
    "resources": [{
            "apiVersion": "2017-04-01",
            "type": "Microsoft.EventHub/namespaces",
            "name": "[parameters('eventHubNamespaceName')]",
            "location": "[parameters('location')]",
            "sku": {
                "name": "[parameters('eventHubSku')]",
                "tier": "[parameters('eventHubSku')]",
                "capacity": 1
            },
            "properties": {
                "isAutoInflateEnabled": false,
                "maximumThroughputUnits": 0
            }
        }, {
            "apiVersion": "2017-04-01",
            "type": "Microsoft.EventHub/namespaces/eventhubs",
            "name": "[concat(parameters('eventHubNamespaceName'), '/', parameters('eventHubName'))]",
            "location": "[parameters('location')]",
            "dependsOn": ["[resourceId('Microsoft.EventHub/namespaces', parameters('eventHubNamespaceName'))]"],
            "properties": {
                "messageRetentionInDays": 7,
                "partitionCount": 1
            }
        }, {
            "type": "Microsoft.Storage/storageAccounts",
            "name": "[parameters('storageAccountName')]",
            "location": "[parameters('location')]",
            "apiVersion": "2018-07-01",
            "sku": {
                "name": "[parameters('storageAccountType')]"
            },
            "kind": "StorageV2",
            "resources": [
                {
                    "name": "[concat('default/', parameters('containerName'))]",
                    "type": "blobServices/containers",
                    "apiVersion": "2018-07-01",
                    "dependsOn": [
                        "[parameters('storageAccountName')]"
                    ],
                    "properties": {
                        "publicAccess": "None"
                    }
                }
            ],
            "properties": {}
        }, {
            "name": "[parameters('kustoClusterName')]",
            "type": "Microsoft.Kusto/clusters",
            "sku": {
                "name": "Standard_D13_v2",
                "tier": "Standard",
                "capacity": 2
            },
            "apiVersion": "2019-09-07",
            "location": "[parameters('location')]",
            "tags": {
                "Created By": "GitHub quickstart template"
            }
        }, {
            "name": "[concat(parameters('kustoClusterName'), '/', parameters('kustoDatabaseName'))]",
            "type": "Microsoft.Kusto/clusters/databases",
            "apiVersion": "2019-09-07",
            "location": "[parameters('location')]",
            "dependsOn": ["[resourceId('Microsoft.Kusto/clusters', parameters('kustoClusterName'))]"],
            "properties": {
                "softDeletePeriodInDays": 365,
                "hotCachePeriodInDays": 31
            }
        }
    ]
}

```
