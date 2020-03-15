---
title: 定义输出值的多个实例
description: 从部署中返回值时，可以在 Azure 资源管理器模板中使用复制操作进行多次迭代。
ms.topic: conceptual
origin.date: 02/25/2020
ms.date: 03/09/2020
ms.author: v-yeche
ms.openlocfilehash: 10eb34eafaca9d632aacb1b5d5c5b42671607dda
ms.sourcegitcommit: b7fe28ec2de92b5befe61985f76c8d0216f23430
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/06/2020
ms.locfileid: "78850588"
---
<!--Verified successfully-->
# <a name="output-iteration-in-azure-resource-manager-templates"></a>Azure 资源管理器模板中的输出迭代

本文展示了如何为 Azure 资源管理器模板中的输出创建多个值。 通过将 **copy** 元素添加到模板的 outputs 节，可以在部署过程中动态返回许多项。

还可以将 copy 用于[资源](copy-resources.md)、[资源中的属性](copy-properties.md)，以及[变量](copy-variables.md)。

## <a name="outputs-iteration"></a>输出迭代

copy 元素采用以下常规格式：

```json
"copy": [
  {
    "count": <number-of-iterations>,
    "input": <values-for-the-variable>
  }
]
```

**count** 属性指定要对该输出值进行的迭代次数。

**input** 属性指定要重复的属性。 你将创建一个由 **input** 属性中的值构造的元素数组。 它可以是单个属性（例如字符串），也可以是具有多个属性的对象。

以下示例创建可变数量的存储帐户，并返回每个存储帐户的终结点：

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "storageCount": {
            "type": "int",
            "defaultValue": 2
        }
    },
    "variables": {
        "baseName": "[concat('storage', uniqueString(resourceGroup().id))]"
    },
    "resources": [
        {
            "type": "Microsoft.Storage/storageAccounts",
            "apiVersion": "2019-04-01",
            "name": "[concat(copyIndex(), variables('baseName'))]",
            "location": "[resourceGroup().location]",
            "sku": {
                "name": "Standard_LRS"
            },
            "kind": "Storage",
            "properties": {},
            "copy": {
                "name": "storagecopy",
                "count": "[parameters('storageCount')]"
            }
        }
    ],
    "outputs": {
        "storageEndpoints": {
            "type": "array",
            "copy": {
                "count": "[parameters('storageCount')]",
                "input": "[reference(concat(copyIndex(), variables('baseName'))).primaryEndpoints.blob]"
            }
        }
    }
}
```

前面的模板返回包含以下值的数组：

```json
[
    "https://0storagecfrbqnnmpeudi.blob.core.chinacloudapi.cn/",
    "https://1storagecfrbqnnmpeudi.blob.core.chinacloudapi.cn/"
]
```

下一示例返回新存储帐户的三个属性。

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "storageCount": {
            "type": "int",
            "defaultValue": 2
        }
    },
    "variables": {
        "baseName": "[concat('storage', uniqueString(resourceGroup().id))]"
    },
    "resources": [
        {
            "type": "Microsoft.Storage/storageAccounts",
            "apiVersion": "2019-04-01",
            "name": "[concat(copyIndex(), variables('baseName'))]",
            "location": "[resourceGroup().location]",
            "sku": {
                "name": "Standard_LRS"
            },
            "kind": "Storage",
            "properties": {},
            "copy": {
                "name": "storagecopy",
                "count": "[parameters('storageCount')]"
            }
        }
    ],
    "outputs": {
        "storageInfo": {
            "type": "array",
            "copy": {
                "count": "[parameters('storageCount')]",
                "input": {
                    "id": "[reference(concat(copyIndex(), variables('baseName')), '2019-04-01', 'Full').resourceId]",
                    "blobEndpoint": "[reference(concat(copyIndex(), variables('baseName'))).primaryEndpoints.blob]",
                    "status": "[reference(concat(copyIndex(), variables('baseName'))).statusOfPrimary]"
                }
            }
        }
    }
}
```

前面的示例返回包含以下值的数组：

```json
[
    {
        "id": "Microsoft.Storage/storageAccounts/0storagecfrbqnnmpeudi",
        "blobEndpoint": "https://0storagecfrbqnnmpeudi.blob.core.chinacloudapi.cn/",
        "status": "available"
    },
    {
        "id": "Microsoft.Storage/storageAccounts/1storagecfrbqnnmpeudi",
        "blobEndpoint": "https://1storagecfrbqnnmpeudi.blob.core.chinacloudapi.cn/",
        "status": "available"
    }
]
```

## <a name="next-steps"></a>后续步骤

* 要查看教程，请参阅[教程：使用资源管理器模板创建多个资源实例](template-tutorial-create-multiple-instances.md)。
* 有关 copy 元素的其他用法，请参阅：
    * [Azure 资源管理器模板中的资源迭代](copy-resources.md)
    * [Azure 资源管理器模板中的属性迭代](copy-properties.md)
    * [Azure 资源管理器模板中的变量迭代](copy-variables.md)
* 若要了解有关模板区段的信息，请参阅[创作 Azure Resource Manager 模板](template-syntax.md)。
* 若要了解如何部署模板，请参阅 [使用 Azure Resource Manager 模板部署应用程序](deploy-powershell.md)。

<!-- Update_Description: new article about copy outputs -->
<!--NEW.date: 03/09/2020-->