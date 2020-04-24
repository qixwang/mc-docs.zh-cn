---
title: CreateUiDefinition 项目
description: 说明如何为 Azure 托管应用程序创建 createUiDefinition 项目。 该文件名为 createUiDefinition.json。
ms.topic: conceptual
author: rockboyfor
origin.date: 07/11/2019
ms.date: 01/20/2020
ms.author: v-yeche
ms.openlocfilehash: 23871cc6296a457e943b3213db8023932731a533
ms.sourcegitcommit: c1ba5a62f30ac0a3acb337fb77431de6493e6096
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2020
ms.locfileid: "78209154"
---
# <a name="reference-user-interface-elements-artifact"></a>参考：用户界面元素项目

本文是 Azure 托管应用程序中 createUiDefinition.json  项目的参考。 有关创作用户界面元素的详细信息，请参阅[创建用户界面元素](create-uidefinition-elements.md)。

## <a name="user-interface-elements"></a>用户界面元素

以下 JSON 演示了 Azure 托管应用程序的 createUiDefinition.json  文件的示例：

```json
{
  "$schema": "https://schema.management.azure.com/schemas/0.1.2-preview/CreateUIDefinition.MultiVm.json#",
  "handler": "Microsoft.Azure.CreateUIDef",
  "version": "0.1.2-preview",
  "parameters": {
    "basics": [
      {}
    ],
    "steps": [
      {
        "name": "applicationSettings",
        "label": "Application Settings",
        "subLabel": {
          "preValidation": "Configure your application settings",
          "postValidation": "Done"
        },
        "bladeTitle": "Application Settings",
        "elements": [
          {
            "name": "funcname",
            "type": "Microsoft.Common.TextBox",
            "label": "Name of the function to be created",
            "toolTip": "Name of the function to be created",
            "visible": true,
            "constraints": {
              "required": true
            }
          },
          {
            "name": "storagename",
            "type": "Microsoft.Common.TextBox",
            "label": "Name of the storage to be created",
            "toolTip": "Name of the storage to be created",
            "visible": true,
            "constraints": {
              "required": true
            }
          },
          {
            "name": "zipFileBlobUri",
            "type": "Microsoft.Common.TextBox",
            "defaultValue": "https://github.com/Azure/azure-quickstart-templates/tree/master/101-custom-rp-with-function/artifacts/functionzip/functionpackage.zip",
            "label": "The Uri to the uploaded function zip file",
            "toolTip": "The Uri to the uploaded function zip file",
            "visible": true
          }
        ]
      }
    ],
    "outputs": {
      "funcname": "[steps('applicationSettings').funcname]",
      "storageName": "[steps('applicationSettings').storagename]",
      "zipFileBlobUri": "[steps('applicationSettings').zipFileBlobUri]"
    }
  }
}
```

## <a name="next-steps"></a>后续步骤

<!--Not Available on - [Tutorial: Create managed application with custom actions and resources](tutorial-create-managed-app-with-custom-provider.md)-->

- [参考：部署模板项目](reference-main-template-artifact.md)
- [参考：视图定义项目](reference-view-definition-artifact.md)

<!-- Update_Description: new article about reference createuidefinition artifact -->
<!--NEW.date: 01/20/2020-->