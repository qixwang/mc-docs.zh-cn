---
title: 教程 - 创建 Azure ML 工作区 - 资源管理器模板
description: 在本教程中，你将使用 Azure 资源管理器模板快速部署用于机器学习的 Azure 工作区
services: machine-learning
author: lobrien
ms.author: laobri
ms.custom: subject-armqs
ms.date: 05/26/2020
ms.service: machine-learning
ms.subservice: core
ms.topic: tutorial
ms.openlocfilehash: 01d70c0c9f42b493d3d9e04b840d3db571423cec
ms.sourcegitcommit: 9d9795f8a5b50cd5ccc19d3a2773817836446912
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/14/2020
ms.locfileid: "88228489"
---
# <a name="tutorial-deploy-an-azure-machine-learning-workspace-using-a-resource-manager-template"></a>教程：使用资源管理器模板部署 Azure 机器学习工作区
[!INCLUDE [applies-to-skus](../../includes/aml-applies-to-basic-enterprise-sku.md)]

本教程演示如何使用 Azure 资源管理器模板创建 Azure 机器学习工作区。 Azure 机器学习工作区可将你的所有机器学习资产从基线数据集组织到已部署的模型中。 工作区是一个单一位置，可供你与同事合作创建、运行和查看试验，管理训练和推理计算资源，以及对部署的模型进行监视和版本控制。

[!INCLUDE [About Azure Resource Manager](../../includes/resource-manager-quickstart-introduction.md)]

如果你的环境满足先决条件，并且你熟悉如何使用 ARM 模板，请选择“部署到 Azure”按钮。 Azure 门户中会打开模板。

[![部署到 Azure](../media/template-deployments/deploy-to-azure.svg)](https://portal.azure.cn/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-machine-learning-create%2Fazuredeploy.json)

## <a name="prerequisites"></a>先决条件

* Azure 订阅。 如果没有 Azure 订阅，请在开始前创建一个[试用帐户](https://www.azure.cn/pricing/1rmb-trial)

* 若要从本地环境**** 使用本文档中的 CLI 命令，需要使用 [Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest)

## <a name="review-the-template"></a>查看模板

本快速入门中使用的模板来自 [Azure 快速启动模板](https://azure.microsoft.com/resources/templates/101-machine-learning-create/)。

```JSON
{
  "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
      "workspaceName": {
        "type": "string",
        "metadata": {
          "description": "Specifies the name of the Azure Machine Learning workspace."
        }
      },
      "location": {
        "type": "string",
        "allowedValues": [
          "australiaeast",
          "brazilsouth",
          "canadacentral",
          "centralus",
          "eastasia",
          "eastus",
          "eastus2",
          "francecentral",
          "japaneast",
          "koreacentral",
          "northcentralus",
          "northeurope",
          "southeastasia",
          "southcentralus",
          "uksouth",
          "westcentralus",
          "westus",
          "westus2",
          "westeurope"
        ],
        "metadata": {
          "description": "Specifies the location for all resources."
        }
      },
      "sku": {
        "type": "string",
        "defaultValue": "basic",
        "allowedValues": [
          "basic",
          "enterprise"
        ],
        "metadata": {
          "description": "Specifies the sku, also referred as 'edition' of the Azure Machine Learning workspace."
        }
      },
      "storageAccountNewOrExisting": {
        "type": "string",
        "defaultValue": "new",
        "allowedValues": [
          "new",
          "existing"
        ]
      },
      "storageAccountName": {
        "type": "string",
        "defaultValue": "[concat('sa',uniqueString(resourceGroup().id))]"
      },
      "storageAccountType": {
        "type": "string",
        "defaultValue": "Standard_LRS",
        "allowedValues": [
          "Standard_LRS",
          "Standard_GRS",
          "Standard_RAGRS",
          "Standard_ZRS",
          "Premium_LRS",
          "Premium_ZRS",
          "Standard_GZRS",
          "Standard_RAGZRS"
        ]
      },
      "storageAccountResourceGroupName": {
        "type": "string",
        "defaultValue": "[resourceGroup().name]"
      },
      "keyVaultNewOrExisting": {
        "type": "string",
        "defaultValue": "new",
        "allowedValues": [
          "new",
          "existing"
        ],
        "metadata": {
          "description": "Determines whether or not a key vault should be provisioned."
        }
      },
      "keyVaultName": {
        "type": "string",
        "defaultValue": "[concat('kv',uniqueString(resourceGroup().id))]"
      },
      "keyVaultResourceGroupName": {
        "type": "string",
        "defaultValue": "[resourceGroup().name]"
      },
      "applicationInsightsNewOrExisting": {
        "type": "string",
        "defaultValue": "new",
        "allowedValues": [
          "new",
          "existing"
        ]
      },
      "applicationInsightsName": {
        "type": "string",
        "defaultValue": "[concat('ai',uniqueString(resourceGroup().id))]"
      },
      "applicationInsightsResourceGroupName": {
        "type": "string",
        "defaultValue": "[resourceGroup().name]"
      },
      "containerRegistry": {
        "type": "string",
        "defaultValue": "",
        "metadata": {
          "description": "The container registry resource id if you want to create a link to the workspace."
        }
      },
       "adbWorkspace": {
        "type": "string",
        "defaultValue": "",
        "metadata": {
          "description": "Azure Databrick workspace resource id to be linked to the workspace"
        }
      },
      "confidential_data":{
        "type": "string",
        "defaultValue": "false",
          "allowedValues": [
            "false",
            "true"
          ],
          "metadata": {
            "description": "Specifies that the Azure Machine Learning workspace holds highly confidential data."
          }
      },
      "encryption_status":{
        "type": "string",
        "defaultValue": "Disabled",
          "allowedValues": [
            "Enabled",
            "Disabled"
          ],
          "metadata": {
            "description": "Specifies if the Azure Machine Learning workspace should be encrypted with customer managed key."
          }
      },
      "cmk_keyvault":{
        "type": "string",
        "defaultValue": "",
        "metadata": {
          "description": "Specifies the customer managed keyVault arm id."
        }
      },
      "resource_cmk_uri":{
        "type": "string",
        "defaultValue": "",
        "metadata": {
          "description": "Specifies if the customer managed keyvault key uri."
        }
      }
  },
  "variables": {
      "tenantId": "[subscription().tenantId]",
      "storageAccount": "[resourceId(parameters('storageAccountResourceGroupName'), 'Microsoft.Storage/storageAccounts', parameters('storageAccountName'))]",
      "keyVault": "[resourceId(parameters('keyVaultResourceGroupName'), 'Microsoft.KeyVault/vaults', parameters('keyVaultName'))]",
      "applicationInsights": "[resourceId(parameters('applicationInsightsResourceGroupName'), 'Microsoft.Insights/components', parameters('applicationInsightsName'))]"
  },
  "resources": [
      {
          "condition": "[equals(parameters('storageAccountNewOrExisting'), 'new')]",
          "type": "Microsoft.Storage/storageAccounts",
          "apiVersion": "2019-04-01",
          "name": "[parameters('storageAccountName')]",
          "location": "[parameters('location')]",
          "sku": {
              "name": "[parameters('storageAccountType')]"
          },
          "kind": "StorageV2",
          "properties": {
              "encryption": {
                  "services": {
                      "blob": {
                          "enabled": true
                      },
                      "file": {
                          "enabled": true
                      }
                  },
                  "keySource": "Microsoft.Storage"
              },
              "supportsHttpsTrafficOnly": true
          }
      },
      {
          "condition": "[equals(parameters('keyVaultNewOrExisting'), 'new')]",
          "type": "Microsoft.KeyVault/vaults",
          "apiVersion": "2019-09-01",
          "name": "[parameters('keyVaultName')]",
          "location": "[parameters('location')]",
          "properties": {
              "tenantId": "[variables('tenantId')]",
              "sku": {
                  "name": "standard",
                  "family": "A"
              },
              "accessPolicies": []
          }
      },
      {
          "condition": "[equals(parameters('applicationInsightsNewOrExisting'), 'new')]",
          "type": "Microsoft.Insights/components",
          "apiVersion": "2018-05-01-preview",
          "name": "[parameters('applicationInsightsName')]",
          "location": "[if(or(equals(parameters('location'),'eastus2'), equals(parameters('location'),'westcentralus')),'southcentralus',parameters('location'))]",
          "kind": "web",
          "properties": {
              "Application_Type": "web"
          }
      },
      {
          "type": "Microsoft.MachineLearningServices/workspaces",
          "apiVersion": "2020-03-01",
          "name": "[parameters('workspaceName')]",
          "location": "[parameters('location')]",
          "dependsOn": [
              "[variables('storageAccount')]",
              "[variables('keyVault')]",
              "[variables('applicationInsights')]"
          ],
          "identity": {
              "type": "systemAssigned"
          },
          "sku": {
              "tier": "[parameters('sku')]",
              "name": "[parameters('sku')]"
          },
          "properties": {
              "friendlyName": "[parameters('workspaceName')]",
              "storageAccount": "[variables('storageAccount')]",
              "keyVault": "[variables('keyVault')]",
              "applicationInsights": "[variables('applicationInsights')]",
              "containerRegistry": "[if(empty(parameters('containerRegistry')), json('null'), parameters('containerRegistry'))]",
              "adbWorkspace": "[if(empty(parameters('adbWorkspace')), json('null'), parameters('adbWorkspace'))]",
              "encryption": {
                  "status": "[parameters('encryption_status')]",
                  "keyVaultProperties": {
                      "keyVaultArmId": "[parameters('cmk_keyvault')]",
                      "keyIdentifier": "[parameters('resource_cmk_uri')]"
                  }
              },
              "hbiWorkspace": "[parameters('confidential_data')]"
          }
      }
  ]
}
```

该模板中定义了以下资源：

* [Microsoft.MachineLearningServices/workspaces](/azure/templates/microsoft.machinelearningservices/workspaces)：创建 Azure ML 工作区。 在此模板中，位置和名称是用户可以传入或以交互方式输入的参数。

## <a name="deploy-the-template"></a>部署模板

若要通过 Azure CLI 使用该模板，请登录并选择你的订阅（请参阅[使用 Azure CLI 登录](https://docs.microsoft.com/cli/azure/authenticate-azure-cli?view=azure-cli-latest)）。 然后运行：

```azurecli
read -p "Enter a project name that is used for generating resource names:" projectName &&
read -p "Enter the location (i.e. centralus):" location &&
templateUri="https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-machine-learning-create/azuredeploy.json" &&
resourceGroupName="${projectName}rg" &&
workspaceName="${projectName}ws" &&
az group create --name $resourceGroupName --location "$location" &&
az deployment group create --resource-group $resourceGroupName --template-uri $templateUri --parameters workspaceName=$workspaceName location=$location && 
echo "Press [ENTER] to continue ..." &&
read
```

运行上述命令时，请输入：

1. 项目名称，它将作为创建的资源组和 Azure ML 工作区名称的基础
1. 要在其中进行部署的 Azure 位置

## <a name="review-deployed-resources"></a>查看已部署的资源

若要查看 Azure ML 工作区，请执行以下操作：

1. 转到 https://portal.azure.cn 
1. 登录 
1. 选择刚创建的工作区

你将看到 Azure 机器学习主页： 

:::image type="content" source="media/tutorial-resource-manager-workspace/workspace-home.png" alt-text="Azure ML 工作区的屏幕截图":::

若要查看与部署相关的所有资源，请单击左上方带有工作区名称（在屏幕截图中为 `my_templated_ws`）的链接。 该链接会转到 Azure 门户中的资源组。 资源组的名称为 `{projectName}rg`，工作区的名称为 `{projectName}ws`。

## <a name="clean-up-resources"></a>清理资源

如果不想使用此工作区，请删除它。 由于此工作区与其他资源（例如存储帐户）关联，因此可能需要删除创建的整个资源组。 可以使用门户删除资源组，方法是单击“删除”按钮并进行确认。 也可以从 CLI 中使用以下命令删除资源组： 

```azurecli
echo "Enter the Resource Group name:" &&
read resourceGroupName &&
az group delete --name $resourceGroupName &&
echo "Press [ENTER] to continue ..."
```

## <a name="next-steps"></a>后续步骤

在本教程中，你通过 Azure 资源管理器模板创建了 Azure 机器学习工作区。 若要探索 Azure 机器学习，请继续学习以下教程。 

> [!div class="nextstepaction"]
> [教程：开始使用 Python SDK 创建第一个 ML 试验](tutorial-1st-experiment-sdk-setup.md)
