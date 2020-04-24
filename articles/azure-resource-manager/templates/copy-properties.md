---
title: 定义属性的多个实例
description: 在资源上创建属性时，可以使用 Azure 资源管理器模板中的复制操作进行多次迭代。
ms.topic: conceptual
origin.date: 02/13/2020
ms.date: 03/09/2020
ms.author: v-yeche
ms.openlocfilehash: 364f77eb3147eb8b4299f35400903607e2698274
ms.sourcegitcommit: c1ba5a62f30ac0a3acb337fb77431de6493e6096
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2020
ms.locfileid: "78850587"
---
<!--Verified successfully-->
# <a name="property-iteration-in-azure-resource-manager-templates"></a>Azure 资源管理器模板中的属性迭代

本文介绍如何在 Azure 资源管理器模板中创建一个属性的多个实例。 通过将 **copy** 元素添加到模板中资源的 properties 节，可以在部署过程中动态设置属性的项数。 还可以避免重复模板语法。

还可以将 copy 用于 [resources](copy-resources.md)、[variables](copy-variables.md) 和 [outputs](copy-outputs.md)。

## <a name="property-iteration"></a>属性迭代

copy 元素采用以下常规格式：

```json
"copy": [
  {
    "name": "<name-of-loop>",
    "count": <number-of-iterations>,
    "input": <values-for-the-property>
  }
]
```

对于 **name**，提供要创建的资源属性的名称。 **count** 属性指定要对该属性进行的迭代次数。

**input** 属性指定要重复的属性。 你将创建一个由 **input** 属性中的值构造的元素数组。

以下示例演示了如何将 `copy` 应用于虚拟机上的 dataDisks 属性：

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "numberOfDataDisks": {
      "type": "int",
      "minValue": 0,
      "maxValue": 16,
      "defaultValue": 16,
      "metadata": {
        "description": "The number of dataDisks to create."
      }
    },
    ...
  },
  "resources": [
    {
      "type": "Microsoft.Compute/virtualMachines",
      "apiVersion": "2017-03-30",
      ...
      "properties": {
        "storageProfile": {
          ...
          "copy": [
            {
              "name": "dataDisks",
              "count": "[parameters('numberOfDataDisks')]",
              "input": {
                "diskSizeGB": 1023,
                "lun": "[copyIndex('dataDisks')]",
                "createOption": "Empty"
              }
            }
          ]
        }
      }
    }
  ]
}
```

请注意，在属性迭代中使用 `copyIndex` 时，必须提供迭代的名称。

> [!NOTE]
> 属性迭代还支持 offset 参数。 偏移量必须在迭代名称之后，例如 copyIndex('dataDisks', 1)。
>

Resource Manager 在部署期间扩展 `copy` 数组。 数组的名称将成为属性的名称。 输入值将成为对象属性。 已部署的模板将成为：

```json
{
  "name": "examplevm",
  "type": "Microsoft.Compute/virtualMachines",
  "apiVersion": "2017-03-30",
  "properties": {
    "storageProfile": {
      "dataDisks": [
        {
          "lun": 0,
          "createOption": "Empty",
          "diskSizeGB": 1023
        },
        {
          "lun": 1,
          "createOption": "Empty",
          "diskSizeGB": 1023
        },
        {
          "lun": 2,
          "createOption": "Empty",
          "diskSizeGB": 1023
        }
      ],
      ...
```

copy 元素是一个数组，因此，可以为资源指定多个属性。

```json
{
  "type": "Microsoft.Network/loadBalancers",
  "apiVersion": "2017-10-01",
  "name": "examleLB",
  "properties": {
    "copy": [
      {
        "name": "loadBalancingRules",
        "count": "[length(parameters('loadBalancingRules'))]",
        "input": {
          ...
        }
      },
      {
        "name": "probes",
        "count": "[length(parameters('loadBalancingRules'))]",
        "input": {
          ...
        }
      }
    ]
  }
}
```

可将资源和属性迭代一起使用。 按名称引用属性迭代。

```json
{
  "type": "Microsoft.Network/virtualNetworks",
  "apiVersion": "2018-04-01",
  "name": "[concat(parameters('vnetname'), copyIndex())]",
  "copy":{
    "count": 2,
    "name": "vnetloop"
  },
  "location": "[resourceGroup().location]",
  "properties": {
    "addressSpace": {
      "addressPrefixes": [
        "[parameters('addressPrefix')]"
      ]
    },
    "copy": [
      {
        "name": "subnets",
        "count": 2,
        "input": {
          "name": "[concat('subnet-', copyIndex('subnets'))]",
          "properties": {
            "addressPrefix": "[variables('subnetAddressPrefix')[copyIndex('subnets')]]"
          }
        }
      }
    ]
  }
}
```

## <a name="copy-limits"></a>复制限制

count 不能超过 800。

count 不能为负数。 如果使用 Azure PowerShell 2.6 或更高版本、Azure CLI 2.0.74 或更高版本或者 REST API 版本 **2019-05-10** 或更高版本部署模板，则可以将 count 设置为零。 更早版本的 PowerShell、CLI 和 REST API 不支持将 count 设为零。

## <a name="example-templates"></a>示例模板

以下示例显示了为一个属性创建多个值的常见方案。

|模板  |说明  |
|---------|---------|
|[部署数据磁盘数量不定的 VM](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-windows-copy-datadisks) |通过虚拟机部署多个数据磁盘。 |

## <a name="next-steps"></a>后续步骤

* 要查看教程，请参阅[教程：使用资源管理器模板创建多个资源实例](template-tutorial-create-multiple-instances.md)。
* 有关 copy 元素的其他用法，请参阅：
    * [Azure 资源管理器模板中的资源迭代](copy-resources.md)
    * [Azure 资源管理器模板中的变量迭代](copy-variables.md)
    * [Azure 资源管理器模板中的输出迭代](copy-outputs.md)
* 若要了解有关模板区段的信息，请参阅[创作 Azure 资源管理器模板](template-syntax.md)。
* 若要了解如何部署模板，请参阅[使用 Azure 资源管理器模板部署应用程序](deploy-powershell.md)。

<!-- Update_Description: new article about copy properties -->
<!--NEW.date: 03/09/2020-->