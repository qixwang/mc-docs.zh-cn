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
ms.openlocfilehash: 49530af547582d88320ae97ed45a3635e5ef4f44
ms.sourcegitcommit: d7b86a424b72849fe8ed32893dd05e4696e4fe85
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/12/2020
ms.locfileid: "77155708"
---
# <a name="quickstart-create-a-single-database-in-azure-sql-database-using-the-azure-resource-manager-template"></a>快速入门：使用 Azure 资源管理器模板在 Azure SQL 数据库中创建单一数据库

在 Azure SQL 数据库中创建数据库时，创建[单一数据库](sql-database-single-database.md)是最快速且最简单的部署选项。 本快速入门介绍如何使用 Azure 资源管理器模板创建单一数据库。 有关详细信息，请参阅 [Azure 资源管理器文档](/azure-resource-manager/)。

如果没有 Azure 订阅，可[创建一个 1 元人民币试用帐户](https://www.azure.cn/zh-cn/pricing/1rmb-trial-full/?form-type=identityauth)。

## <a name="create-a-single-database"></a>创建单一数据库

单一数据库有一组通过两个[购买模型](sql-database-purchase-models.md)中的一个定义的计算、内存和存储资源。 创建单一数据库时，也定义一个 [SQL 数据库服务器](sql-database-servers.md)来管理它并将它放置在指定区域的 [Azure 资源组](../azure-resource-manager/management/overview.md)中。

以下 JSON 文件是本文中使用的模板。 该模板存储在 [GitHub](https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/SQLServerAndDatabase/azuredeploy.json) 中。 可以在 [Azure 快速入门模板](https://azure.microsoft.com/resources/templates/?resourceType=Microsoft.Sql&pageNumber=1&sort=Popular)中找到更多 Azure SQL 数据库模板示例。

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "projectName": {
      "type": "string",
      "metadata": {
        "description": "Specify a project name that is used to generate resource names."
      }
    },
    "location": {
      "type": "string",
      "defaultValue": "[resourceGroup().location]",
      "metadata": {
        "description": "Specify the Azure location for all services."
      }
    },
    "adminUser": {
      "type": "string",
      "metadata": {
        "description": "Specify the username for the database server administrator."
      }
    },
    "adminPassword": {
      "type": "securestring",
      "metadata": {
        "description": "Specify the password for the database server administrator."
      }
    }
  },
  "variables": {
    "databaseServerName": "[concat(toLower(parameters('projectName')),'server')]",
    "databaseName": "[concat(parameters('projectName'),'db')]"
  },
  "resources": [
    {
      "type": "Microsoft.Sql/servers",
      "name": "[variables('databaseServerName')]",
      "location": "[parameters('location')]",
      "apiVersion": "2015-05-01-preview",
      "properties": {
        "administratorLogin": "[parameters('adminUser')]",
        "administratorLoginPassword": "[parameters('adminPassword')]",
        "version": "12.0"
      },
      "resources": [
        {
          "type": "Microsoft.Sql/servers/databases",
          "name": "[concat(string(variables('databaseServerName')), '/', string(variables('databaseName')))]",
          "location": "[parameters('location')]",
          "apiVersion": "2017-10-01-preview",
          "dependsOn": [
            "[resourceID('Microsoft.Sql/servers/', variables('databaseServerName'))]"
          ]
        },
        {
          "type": "firewallrules",
          "name": "AllowAllAzureIps",
          "location": "[parameters('location')]",
          "apiVersion": "2015-05-01-preview",
          "dependsOn": [
            "[resourceID('Microsoft.Sql/servers/', variables('databaseServerName'))]"
          ],
          "properties": {
            "startIpAddress": "0.0.0.0",
            "endIpAddress": "0.0.0.0"
          }
        }
      ]
    }
  ]
}
```

# <a name="powershelltabazure-powershell"></a>[PowerShell](#tab/azure-powershell)

```azurepowershell
$projectName = Read-Host -Prompt "Enter a project name that is used for generating resource names"
$location = Read-Host -Prompt "Enter an Azure location (i.e. chinaeast2)"
$adminUser = Read-Host -Prompt "Enter the SQL server administrator username"
$adminPassword = Read-Host -Prompt "Enter the SQl server administrator password" -AsSecureString

$resourceGroupName = "${projectName}rg"

New-AzResourceGroup -Name $resourceGroupName -Location $location
New-AzResourceGroupDeployment -ResourceGroupName $resourceGroupName -TemplateFile "D:\GitHub\azure-docs-json-samples\SQLServerAndDatabase\azuredeploy.json" -projectName $projectName -adminUser $adminUser -adminPassword $adminPassword

Read-Host -Prompt "Press [ENTER] to continue ..."
```

# <a name="azure-clitabazure-cli"></a>[Azure CLI](#tab/azure-cli)

```azurecli
$projectName = Read-Host -Prompt "Enter a project name that is used for generating resource names"
$location = Read-Host -Prompt "Enter an Azure location (i.e. chinaeast2)"
$adminUser = Read-Host -Prompt "Enter the SQL server administrator username"
$adminPassword = Read-Host -Prompt "Enter the SQl server administrator password" -AsSecureString

$resourceGroupName = "${projectName}rg"

az group create --location $location --name $resourceGroupName

az group deployment create -g $resourceGroupName --template-uri "D:\GitHub\azure-docs-json-samples\SQLServerAndDatabase\azuredeploy.json" `
    --parameters 'projectName=' + $projectName \
                 'adminUser=' + $adminUser \
                 'adminPassword=' + $adminPassword

Read-Host -Prompt "Press [ENTER] to continue ..."
```

* * *

## <a name="query-the-database"></a>查询数据库

若要查询数据库，请参阅[查询数据库](./sql-database-single-database-get-started.md#query-the-database)。

## <a name="clean-up-resources"></a>清理资源

如果希望转到[后续步骤](#next-steps)，请保留此资源组、数据库服务器和单一数据库。 后续步骤展示了如何使用各种方法连接和查询数据库。

若要删除资源组，请执行以下操作：

# <a name="powershelltabazure-powershell"></a>[PowerShell](#tab/azure-powershell)

```azurepowershell
$resourceGroupName = Read-Host -Prompt "Enter the Resource Group name"
Remove-AzResourceGroup -Name $resourceGroupName
```

# <a name="azure-clitabazure-cli"></a>[Azure CLI](#tab/azure-cli)

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
