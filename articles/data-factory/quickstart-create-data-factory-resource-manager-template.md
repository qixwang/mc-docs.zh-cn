---
title: 使用 Azure 资源管理器模板（ARM 模板）创建 Azure 数据工厂
description: 使用 Azure 资源管理器模板（ARM 模板）创建示例 Azure 数据工厂管道。
services: data-factory
ms.service: data-factory
tags: azure-resource-manager
ms.workload: data-services
author: WenJason
ms.author: v-jay
ms.reviewer: maghan, jingwang
ms.topic: quickstart
ms.custom: subject-armqs
origin.date: 07/16/2020
ms.date: 08/10/2020
ms.openlocfilehash: f02584dbc839bbe2cd0c21d3ab125fafbd1a241b
ms.sourcegitcommit: 66563f2b68cce57b5816f59295b97f1647d7a3d6
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/07/2020
ms.locfileid: "87914263"
---
# <a name="quickstart-create-an-azure-data-factory-using-arm-template"></a>快速入门：使用 ARM 模板创建 Azure 数据工厂

[!INCLUDE[appliesto-adf-xxx-md](includes/appliesto-adf-xxx-md.md)]

本快速入门介绍如何使用 Azure 资源管理器模板（ARM 模板）来创建 Azure 数据工厂。 在此数据工厂中创建的管道会将数据从 Azure Blob 存储中的一个文件夹**复制**到另一个文件夹。 有关如何使用 Azure 数据工厂**转换**数据的教程，请参阅[教程：使用 Spark 转换数据](transform-data-using-spark.md)。

[!INCLUDE [About Azure Resource Manager](../../includes/resource-manager-quickstart-introduction.md)]

> [!NOTE]
> 本文不提供数据工厂服务的详细介绍。 有关 Azure 数据工厂服务的介绍，请参阅 [Azure 数据工厂简介](introduction.md)。

如果你的环境满足先决条件，并且你熟悉如何使用 ARM 模板，请选择“部署到 Azure”按钮。 Azure 门户中会打开模板。

[![“部署到 Azure”](../media/template-deployments/deploy-to-azure.svg)](https://portal.azure.cn/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-data-factory-v2-blob-to-blob-copy%2Fazuredeploy.json)

## <a name="prerequisites"></a>先决条件

### <a name="azure-subscription"></a>Azure 订阅

如果没有 Azure 订阅，可在开始前创建一个[试用帐户](https://wd.azure.cn/pricing/1rmb-trial-full/?form-type=identityauth)。

### <a name="create-a-file"></a>创建文件

打开文本编辑器（如记事本），并创建包含以下内容的名为“emp.txt”的文件：

```emp.txt
John, Doe
Jane, Doe
```

将此文件保存在 C:\ADFv2QuickStartPSH 文件夹中。 （如果此文件夹不存在，则创建它。）

## <a name="review-template"></a>审阅模板

本快速入门中使用的模板来自 [Azure 快速启动模板](https://azure.microsoft.com/resources/templates/101-data-factory-v2-blob-to-blob-copy/)。

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "dataFactoryName": {
            "type": "string",
            "defaultValue": "[concat('datafactory', uniqueString(resourceGroup().id))]",
            "metadata": {
                "description": "Data Factory Name"
            }
        },
        "location": {
            "type": "string",
            "defaultValue": "[resourceGroup().location]",
            "metadata": {
                "description": "Location of the data factory."
            }
        },
        "storageAccountName": {
            "type": "string",
            "defaultValue": "[concat('storage', uniqueString(resourceGroup().id))]",
            "metadata": {
                "description": "Name of the Azure storage account that contains the input/output data."
            }
        },
        "blobContainer": {
            "type": "string",
            "defaultValue": "[concat('blob', uniqueString(resourceGroup().id))]",
            "metadata": {
                "description": "Name of the blob container in the Azure Storage account."
            }
        }
    },
    "variables": {
        "storageAccountId": "[resourceId('Microsoft.Storage/storageAccounts', parameters('storageAccountName'))]",
        "storageLinkedService": "[resourceId('Microsoft.DataFactory/factories/linkedServices', parameters('dataFactoryName'), 'armTemplateStorageLinkedService')]",
        "datasetIn": "[resourceId('Microsoft.DataFactory/factories/datasets', parameters('dataFactoryName'), 'armTemplateTestDatasetIn')]",
        "datasetOut": "[resourceId('Microsoft.DataFactory/factories/datasets', parameters('dataFactoryName'), 'armTemplateTestDatasetOut')]"
    },
    "resources": [
        {
            "type": "Microsoft.Storage/storageAccounts",
            "apiVersion": "2019-06-01",
            "name": "[parameters('storageAccountName')]",
            "location": "[parameters('location')]",
            "sku": {
                "name": "Standard_LRS"
            },
            "kind": "StorageV2",
            "properties": {},
            "resources": [
                {
                    "type": "blobServices/containers",
                    "apiVersion": "2019-06-01",
                    "name": "[concat('default/', parameters('blobContainer'))]",
                    "dependsOn": [
                        "[parameters('storageAccountName')]"
                    ]
                }
            ]
        },
        {
            "type": "Microsoft.DataFactory/factories",
            "name": "[parameters('dataFactoryName')]",
            "apiVersion": "2018-06-01",
            "location": "[parameters('location')]",
            "properties": {},
            "identity": {
                "type": "SystemAssigned"
            },
            "resources": [
                {
                    "type": "Microsoft.DataFactory/factories/linkedServices",
                    "name": "[concat(parameters('dataFactoryName'), '/ArmtemplateStorageLinkedService')]",
                    "apiVersion": "2018-06-01",
                    "location": "[parameters('location')]",
                    "dependsOn": [
                        "[parameters('dataFactoryName')]",
                        "[parameters('storageAccountName')]"
                    ],
                    "properties": {
                        "type": "AzureBlobStorage",
                        "typeProperties": {
                            "connectionString": "[concat('DefaultEndpointsProtocol=https;EndpointSuffix=core.chinacloudapi.cn;AccountName=',parameters('storageAccountName'),';AccountKey=',listKeys(variables('storageAccountId'), '2019-06-01').keys[0].value)]"
                        }
                    }
                },
                {
                    "type": "Microsoft.DataFactory/factories/datasets",
                    "name": "[concat(parameters('dataFactoryName'), '/ArmtemplateTestDatasetIn')]",
                    "apiVersion": "2018-06-01",
                    "location": "[parameters('location')]",
                    "dependsOn": [
                        "[parameters('dataFactoryName')]",
                        "[variables('storageLinkedService')]"
                    ],
                    "properties": {
                        "linkedServiceName": {
                            "referenceName": "ArmtemplateStorageLinkedService",
                            "type": "LinkedServiceReference"
                        },
                        "type": "Binary",
                        "typeProperties": {
                            "location": {
                                "type": "AzureBlobStorageLocation",
                                "container": "[parameters('blobContainer')]",
                                "folderPath": "input",
                                "fileName": "emp.txt"
                            }
                        }
                    }
                },
                {
                    "type": "Microsoft.DataFactory/factories/datasets",
                    "name": "[concat(parameters('dataFactoryName'), '/ArmtemplateTestDatasetOut')]",
                    "apiVersion": "2018-06-01",
                    "location": "[parameters('location')]",
                    "dependsOn": [
                        "[parameters('dataFactoryName')]",
                        "[variables('storageLinkedService')]"
                    ],
                    "properties": {
                        "linkedServiceName": {
                            "referenceName": "ArmtemplateStorageLinkedService",
                            "type": "LinkedServiceReference"
                        },
                        "type": "Binary",
                        "typeProperties": {
                            "location": {
                                "type": "AzureBlobStorageLocation",
                                "container": "[parameters('blobContainer')]",
                                "folderPath": "output"
                            }
                        }
                    }
                },
                {
                    "type": "Microsoft.DataFactory/factories/pipelines",
                    "name": "[concat(parameters('dataFactoryName'), '/ArmtemplateSampleCopyPipeline')]",
                    "apiVersion": "2018-06-01",
                    "location": "[parameters('location')]",
                    "dependsOn": [
                        "[parameters('dataFactoryName')]",
                        "[variables('datasetIn')]",
                        "[variables('datasetOut')]"
                    ],
                    "properties": {
                        "activities": [
                            {
                            "name": "MyCopyActivity",
                            "type": "Copy",
                            "policy": {
                                "timeout": "7.00:00:00",
                                "retry": 0,
                                "retryIntervalInSeconds": 30,
                                "secureOutput": false,
                                "secureInput": false
                            },
                            "typeProperties": {
                                "source": {
                                    "type": "BinarySource",
                                    "storeSettings": {
                                        "type": "AzureBlobStorageReadSettings",
                                        "recursive": true
                                    }
                                },
                                "sink": {
                                    "type": "BinarySink",
                                    "storeSettings": {
                                        "type": "AzureBlobStorageWriteSettings"
                                    }
                                },
                                "enableStaging": false
                            },
                            "inputs": [{
                                "referenceName": "ArmtemplateTestDatasetIn",
                                "type": "DatasetReference",
                                "parameters": {

                                    }
                                }
                            ],
                            "outputs": [{
                                "referenceName": "ArmtemplateTestDatasetOut",
                                "type": "DatasetReference",
                                "parameters": {}
                                    }
                                ]
                            }
                        ]
                    }
                }
            ]
        }
    ]
}
```

该模板中定义了 Azure 资源：

- [Microsoft.Storage/storageAccounts](https://docs.microsoft.com/azure/templates/Microsoft.Storage/storageAccounts)：定义存储帐户。
- [Microsoft.DataFactory/factories](https://docs.microsoft.com/azure/templates/microsoft.datafactory/factories)：创建 Azure 数据工厂。
- [Microsoft.DataFactory/factories/linkedServices](https://docs.microsoft.com/azure/templates/microsoft.datafactory/factories/linkedservices)：创建 Azure 数据工厂链接服务。
- [Microsoft.DataFactory/factories/datasets](https://docs.microsoft.com/azure/templates/microsoft.datafactory/factories/datasets)：创建 Azure 数据工厂数据集。
- [Microsoft.DataFactory/factories/pipelines](https://docs.microsoft.com/azure/templates/microsoft.datafactory/factories/pipelines)：创建 Azure 数据工厂管道。

可以在[快速入门模板库](https://azure.microsoft.com/resources/templates/?resourceType=Microsoft.Datafactory&pageNumber=1&sort=Popular)中找到更多 Azure 数据工厂模板示例。

## <a name="deploy-the-template"></a>部署模板

1. 选择下图登录到 Azure 并打开一个模板。 此模板创建 Azure 数据工厂帐户、存储帐户和 blob 容器。

    [![部署到 Azure](../media/template-deployments/deploy-to-azure.svg)](https://portal.azure.cn/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-data-factory-v2-blob-to-blob-copy%2Fazuredeploy.json)

2. 选择或输入以下值。

    :::image type="content" source="media/quickstart-create-data-factory-resource-manager-template/data-factory-deploy-arm-template.png" alt-text="部署 ADF ARM 模板":::

    除非另有指定，否则请使用默认值创建 Azure 数据工厂资源：

    - 订阅：选择 Azure 订阅。
    - 资源组：选择“新建”，输入资源组的唯一名称，然后选择“确定”。
    - **区域**：选择一个位置。  例如“美国东部”。
    - 数据工厂名称：使用默认值。
    - 位置：使用默认值。
    - **存储帐户名称**：使用默认值。
    - Blob 容器：使用默认值。

## <a name="review-deployed-resources"></a>查看已部署的资源

1. 选择“转到资源组”。

    :::image type="content" source="media/quickstart-create-data-factory-resource-manager-template/data-factory-go-to-resource-group.png" alt-text="资源组":::

2.  验证是否已创建 Azure 数据工厂。
    1. Azure 数据工厂名称的格式为 - datafactory\<uniqueid\>。

    :::image type="content" source="media/quickstart-create-data-factory-resource-manager-template/data-factory-sample.png" alt-text="数据工厂示例":::

2. 验证是否已创建存储帐户。
    1. 存储帐户名称的格式为 - storage\<uniqueid\>。

    :::image type="content" source="media/quickstart-create-data-factory-resource-manager-template/data-factory-arm-template-storage-account.png" alt-text="存储帐户":::

3. 选择创建的存储帐户，并选择“容器”。
    1. 在“容器”页上，选择创建的 blob 容器。
        1. blob 容器名称的格式为 - blob\<uniqueid\>。

    :::image type="content" source="media/quickstart-create-data-factory-resource-manager-template/data-factory-arm-template-blob-container.png" alt-text="blob 容器":::

### <a name="upload-a-file"></a>上传文件

1. 在“容器”页上，选择“上传”。

2. 在右侧窗口，选择“文件”框，然后浏览到先前创建的“emp.txt”文件并进行选择。

3. 展开“高级”标题。

4. 在“上传到文件夹”框中，输入“输入”。

5. 选择“上传”按钮。 应该会在列表中看到 **emp.txt** 文件和上传状态。

6. 选择“关闭”图标 (X) 以关闭“上传 Blob”页面  。

    :::image type="content" source="media/quickstart-create-data-factory-resource-manager-template/data-factory-arm-template-upload-blob-file.png" alt-text="将文件上传到输入文件夹":::

使容器页保持打开状态，因为你可以使用它在快速入门结束时验证输出。

### <a name="start-trigger"></a>启动触发器

1. 导航到“数据工厂”页，选择创建的数据工厂。 

2. 选择“创建者和监视器”磁贴。 

    :::image type="content" source="media/quickstart-create-data-factory-resource-manager-template/data-factory-author-monitor-tile.png" alt-text="创建者和监视器":::

2. 选择“创作”选项卡 :::image type="icon" source="media/quickstart-create-data-factory-resource-manager-template/data-factory-author.png" border="false":::。

3. 选择创建的管道 - ArmtemplateSampleCopyPipeline。

    :::image type="content" source="media/quickstart-create-data-factory-resource-manager-template/data-factory-arm-template-pipelines.png" alt-text="ARM 模板管道":::

4. 选择“添加触发器” > “立即触发器”。

    触发器:::image type="content" source="media/quickstart-create-data-factory-resource-manager-template/data-factory-trigger-now.png" alt-text="":::

5. 在“管道运行”下的右窗格中，选择“确定”。

### <a name="monitor-the-pipeline"></a>监视管道

1. 选择“监视”选项卡 :::image type="icon" source="media/quickstart-create-data-factory-resource-manager-template/data-factory-monitor.png" border="false":::。

2. 此时会看到与管道运行相关联的活动运行。 在本快速入门中，管道只有一个活动，其类型为：“复制”。 因此会看到该活动运行。

    :::image type="content" source="media/quickstart-create-data-factory-resource-manager-template/data-factory-arm-template-successful-run.png" alt-text="成功运行":::

### <a name="verify-the-output-file"></a>验证输出文件

该管道自动在 blob 容器中创建一个输出文件夹。 然后将 emp.txt 文件从 input 文件夹复制到 output 文件夹。 

1. 在 Azure 门户的“容器”页中选择“刷新”，以查看输出文件夹。 

2. 在文件夹列表中，选择“output”。

3. 确认 **emp.txt** 已复制到 output 文件夹。 

    :::image type="content" source="media/quickstart-create-data-factory-resource-manager-template/data-factory-arm-template-output.png" alt-text="输出":::

## <a name="clean-up-resources"></a>清理资源

可以通过两种方式清理在快速入门中创建的资源。 可以[删除 Azure 资源组](../azure-resource-manager/management/delete-resource-group.md)，其中包括资源组中的所有资源。 若要使其他资源保持原封不动，请仅删除在此教程中创建的数据工厂。

删除资源组时会删除所有资源，包括其中的数据工厂。 运行以下命令可以删除整个资源组： 

```azurepowershell
Remove-AzResourceGroup -ResourceGroupName $resourcegroupname
```

如果只需删除数据工厂，而不是整个资源组，请运行以下命令： 

```azurepowershell
Remove-AzDataFactoryV2 -Name $dataFactoryName -ResourceGroupName $resourceGroupName
```

## <a name="next-steps"></a>后续步骤

在本快速入门中，你使用 ARM 模板创建了 Azure 数据工厂工作区，并验证了部署。 若要详细了解 Azure 数据工厂和 Azure 资源管理器，请继续阅读以下文章。

- [Azure 数据工厂文档](index.yml)
- 了解有关 [Azure 资源管理器](../azure-resource-manager/management/overview.md)的详细信息
- 获取其他 [Azure 数据工厂 ARM 模板](https://azure.microsoft.com/resources/templates/?resourceType=Microsoft.Datafactory&pageNumber=1&sort=Popular)