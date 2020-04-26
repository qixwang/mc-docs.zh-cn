---
title: Azure 资源管理器：创建单一数据库
description: 使用 Azure 资源管理器模板在 Azure SQL 数据库中创建单一数据库。
services: sql-database
ms.service: sql-database
ms.subservice: single-database
ms.custom: ''
ms.devlang: ''
ms.topic: quickstart
author: WenJason
ms.author: v-jay
ms.reviewer: carlrab
origin.date: 06/28/2019
ms.date: 02/17/2020
ms.openlocfilehash: 5168c453017154f089e7466270fcb28bb9889a23
ms.sourcegitcommit: a4a2521da9b29714aa6b511fc6ba48279b5777c8
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/24/2020
ms.locfileid: "82126717"
---
# <a name="quickstart-create-a-single-database-in-azure-sql-database-using-the-azure-resource-manager-template"></a>快速入门：使用 Azure 资源管理器模板在 Azure SQL 数据库中创建单一数据库

在 Azure SQL 数据库中创建数据库时，创建[单一数据库](sql-database-single-database.md)是最快速且最简单的部署选项。 本快速入门介绍如何使用 Azure 资源管理器模板创建单一数据库。

[!INCLUDE [About Azure Resource Manager](../../includes/resource-manager-quickstart-introduction.md)]

如果没有 Azure 订阅，可[创建一个 1 元人民币试用帐户](https://www.azure.cn/zh-cn/pricing/1rmb-trial-full/?form-type=identityauth)。

## <a name="prerequisites"></a>先决条件

无

## <a name="create-a-single-database"></a>创建单一数据库

单一数据库有一组通过两个[购买模型](sql-database-purchase-models.md)中的一个定义的计算、内存和存储资源。 创建单一数据库时，也定义一个 [SQL 数据库服务器](sql-database-servers.md)来管理它并将它放置在指定区域的 [Azure 资源组](../azure-resource-manager/management/overview.md)中。

### <a name="review-the-template"></a>查看模板

本快速入门中使用的模板来自 [Azure 快速入门模板](https://azure.microsoft.com/resources/templates/101-sql-logical-server/)。

```JSON
{
 "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentTemplate.json#",
 "contentVersion": "1.0.0.0",
 "parameters": {
   "serverName": {
     "type": "string",
     "defaultValue":"[uniqueString('sql', resourceGroup().id)]",
     "metadata": {
       "description": "The name of the SQL logical server."
     }
   },
   "location": {
     "type": "string",
     "defaultValue": "[resourceGroup().location]",
     "metadata": {
       "description": "Location for all resources."
     }
   },
   "administratorLogin": {
     "type": "string",
     "metadata": {
       "description": "The administrator username of the SQL logical server."
     }
   },
   "administratorLoginPassword": {
     "type": "securestring",
     "metadata": {
       "description": "The administrator password of the SQL logical server."
     }
   },
   "enableADS": {
     "defaultValue": true,
     "type": "bool",
     "metadata": {
       "description": "Enable Advanced Data Security, the user deploying the template must have an administrator or owner permissions."
     }
   },
   "allowAzureIPs": {
     "defaultValue": true,
     "type": "bool",
     "metadata": {
       "description": "Allow Azure services to access server."
     }
   },
   "connectionType": {
     "defaultValue": "Default",
     "allowedValues": [ "Default", "Redirect", "Proxy" ],
     "type": "string",
     "metadata": {
       "description": "SQL logical server connection type."
     }
   }
 },
 "variables": {
    "serverResourceGroupName": "[resourceGroup().name]",
    "subscriptionId": "[subscription().subscriptionId]",
    "uniqueStorage": "[uniqueString(variables('subscriptionId'), variables('serverResourceGroupName'), parameters('location'))]",
    "storageName": "[tolower(concat('sqlva', variables('uniqueStorage')))]",
    "uniqueRoleGuid": "[guid(resourceId('Microsoft.Storage/storageAccounts', variables('storageName')), variables('storageBlobContributor'), resourceId('Microsoft.Sql/servers', parameters('serverName')))]",
    "StorageBlobContributor": "[subscriptionResourceId('Microsoft.Authorization/roleDefinitions', 'ba92f5b4-2d11-453d-a403-e96b0029c9fe')]"
  },
 "resources": [
   {
     "type": "Microsoft.Sql/servers",
     "apiVersion": "2018-06-01-preview",
     "name": "[parameters('serverName')]",
     "location": "[parameters('location')]",
     "identity": "[if(parameters('enableADS'), json('{\"type\":\"SystemAssigned\"}'), json('null'))]",
     "properties": {
       "administratorLogin": "[parameters('administratorLogin')]",
       "administratorLoginPassword": "[parameters('administratorLoginPassword')]",
       "version": "12.0"
     },
     "resources": [
       {
         "condition": "[parameters('allowAzureIPs')]",
         "type": "firewallRules",
         "apiVersion": "2018-06-01-preview",
         "name": "AllowAllWindowsAzureIps",
         "location": "[parameters('location')]",
         "dependsOn": [
           "[resourceId('Microsoft.Sql/servers', parameters('serverName'))]"
         ],
         "properties": {
            "endIpAddress": "0.0.0.0",
           "startIpAddress": "0.0.0.0"
         }
       },
       {
         "condition": "[parameters('enableADS')]",
         "type": "securityAlertPolicies",
         "apiVersion": "2017-03-01-preview",
         "name": "Default",
         "dependsOn": [
           "[resourceId('Microsoft.Sql/servers', parameters('serverName'))]"
         ],
         "properties": {
           "state": "Enabled",
           "emailAccountAdmins": true
         }
       },
       {
         "condition": "[parameters('enableADS')]",
         "type": "vulnerabilityAssessments",
         "apiVersion": "2018-06-01-preview",
         "name": "Default",
         "dependsOn": [
           "[resourceId('Microsoft.Sql/servers', parameters('serverName'))]",
           "[resourceId('Microsoft.Sql/servers/securityAlertPolicies', parameters('serverName'), 'Default')]",
           "[resourceId('Microsoft.Storage/storageAccounts', variables('storageName'))]"
         ],
         "properties": {
           "storageContainerPath": "[if(parameters('enableADS'), concat(reference(resourceId('Microsoft.Storage/storageAccounts', variables('storageName'))).primaryEndpoints.blob, 'vulnerability-assessment'), json('null'))]",
           "recurringScans": {
             "isEnabled": true,
             "emailSubscriptionAdmins": true
           }
         }
       },
       {
         "type": "connectionPolicies",
         "apiVersion": "2014-04-01",
         "name": "Default",
         "dependsOn": [
           "[resourceId('Microsoft.Sql/servers', parameters('serverName'))]"
         ],
         "properties": {
           "connectionType": "[parameters('connectionType')]"
         }
       }
     ]
   },
   {
     "condition": "[parameters('enableADS')]",
     "type": "Microsoft.Storage/storageAccounts",
     "apiVersion": "2019-06-01",
     "name": "[variables('storageName')]",
     "location": "[parameters('location')]",
     "sku": {
       "name": "Standard_LRS"
     },
     "kind": "StorageV2",
     "resources": [
       {
         "condition": "[parameters('enableADS')]",
         "type": "Microsoft.Storage/storageAccounts/providers/roleAssignments",
         "apiVersion": "2018-09-01-preview",
         "name": "[concat(variables('storageName'), '/Microsoft.Authorization/', variables('uniqueRoleGuid') )]",
         "dependsOn": [
           "[resourceId('Microsoft.Sql/servers', parameters('serverName'))]",
           "[resourceId('Microsoft.Storage/storageAccounts', variables('storageName'))]"
         ],
         "properties": {
           "roleDefinitionId": "[variables('StorageBlobContributor')]",
           "principalId": "[reference(resourceId('Microsoft.Sql/servers', parameters('serverName')), '2018-06-01-preview', 'Full').identity.principalId]",
           "scope": "[resourceId('Microsoft.Storage/storageAccounts', variables('storageName'))]",
           "principalType": "ServicePrincipal"
         }
       }
     ]
   }
 ]
}
```

该模板中定义了以下资源：

- [**Microsoft.Sql/servers**](https://docs.microsoft.com/azure/templates/microsoft.sql/servers)
- [**Microsoft.Sql/servers/firewallRules**](https://docs.microsoft.com/azure/templates/microsoft.sql/servers/firewallrules)
- [**Microsoft.Sql/servers/securityAlertPolicies**](https://docs.microsoft.com/azure/templates/microsoft.sql/servers/securityalertpolicies)
- [**Microsoft.Sql/servers/vulnerabilityAssessments**](https://docs.microsoft.com/azure/templates/microsoft.sql/servers/vulnerabilityassessments)
- [**Microsoft.Sql/servers/connectionPolicies**](https://docs.microsoft.com/azure/templates/microsoft.sql/servers/connectionpolicies)
- [**Microsoft.Storage/storageAccounts**](https://docs.microsoft.com/azure/templates/microsoft.storage/storageaccounts)
- [**Microsoft.Storage/storageAccounts/providers/roleAssignments**](https://docs.microsoft.com/azure/templates/microsoft.authorization/roleassignments)

可以在 [Azure 快速入门模板](https://azure.microsoft.com/resources/templates/?resourceType=Microsoft.Sql&pageNumber=1&sort=Popular)中找到更多 Azure SQL 数据库模板示例。

### <a name="deploy-the-template"></a>部署模板

从以下 PowerShell 代码块中选择“试用”  以打开 Azure Cloud Shell。

# <a name="powershell"></a>[PowerShell](#tab/azure-powershell)

```azurepowershell
$projectName = Read-Host -Prompt "Enter a project name that is used for generating resource names"
$location = Read-Host -Prompt "Enter an Azure location (i.e. chinaeast2)"
$adminUser = Read-Host -Prompt "Enter the SQL server administrator username"
$adminPassword = Read-Host -Prompt "Enter the SQl server administrator password" -AsSecureString

$resourceGroupName = "${projectName}rg"

New-AzResourceGroup -Name $resourceGroupName -Location $location
New-AzResourceGroupDeployment -ResourceGroupName $resourceGroupName -TemplateUri "https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-sql-logical-server/azuredeploy.json" -administratorLogin $adminUser -administratorLoginPassword $adminPassword

Read-Host -Prompt "Press [ENTER] to continue ..."
```

# <a name="azure-cli"></a>[Azure CLI](#tab/azure-cli)

```azurecli
$projectName = Read-Host -Prompt "Enter a project name that is used for generating resource names"
$location = Read-Host -Prompt "Enter an Azure location (i.e. chinaeast2)"
$adminUser = Read-Host -Prompt "Enter the SQL server administrator username"
$adminPassword = Read-Host -Prompt "Enter the SQl server administrator password" -AsSecureString

$resourceGroupName = "${projectName}rg"

az group create --location $location --name $resourceGroupName

az group deployment create -g $resourceGroupName --template-uri "D:\GitHub\azure-docs-json-samples\SQLServerAndDatabase\azuredeploy.json" `
    --parameters 'projectName=' + $projectName \
                 'administratorLogin=' + $adminUser \
                 'administratorLoginPassword=' + $adminPassword

Read-Host -Prompt "Press [ENTER] to continue ..."
```

* * *

## <a name="validate-the-deployment"></a>验证部署

若要查询数据库，请参阅[查询数据库](./sql-database-single-database-get-started.md#query-the-database)。

## <a name="clean-up-resources"></a>清理资源

如果希望转到[后续步骤](#next-steps)，请保留此资源组、数据库服务器和单一数据库。 后续步骤展示了如何使用各种方法连接和查询数据库。

若要删除资源组，请执行以下操作：

# <a name="powershell"></a>[PowerShell](#tab/azure-powershell)

```azurepowershell
$resourceGroupName = Read-Host -Prompt "Enter the Resource Group name"
Remove-AzResourceGroup -Name $resourceGroupName
```

# <a name="azure-cli"></a>[Azure CLI](#tab/azure-cli)

```azurecli
echo "Enter the Resource Group name:" &&
read resourceGroupName &&
az group delete --name $resourceGroupName
```

* * *

## <a name="next-steps"></a>后续步骤

- 创建服务器级防火墙规则，以便通过本地或远程工具连接到单一数据库。 有关详细信息，请参阅[创建服务器级防火墙规则](sql-database-server-level-firewall-rule.md)。
- 创建服务器级防火墙规则后，使用多种不同的工具和语言[连接和查询](sql-database-connect-query.md)数据库。
  - [使用 SQL Server Management Studio 连接和查询](sql-database-connect-query-ssms.md)
  - [使用 Azure Data Studio 连接和查询](https://docs.microsoft.com/sql/azure-data-studio/quickstart-sql-database?toc=/sql-database/toc.json)
- 若要使用 Azure CLI 创建单一数据库，请参阅 [Azure CLI 示例](sql-database-cli-samples.md)。
- 若要使用 Azure PowerShell 创建单一数据库，请参阅 [Azure PowerShell 示例](sql-database-powershell-samples.md)。
