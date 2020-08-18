---
author: cephalin
ms.service: app-service-web
ms.topic: include
origin.date: 11/03/2016
ms.date: 08/13/2020
ms.author: v-tawe
ms.openlocfilehash: 6dcf5d7aa40d99f789d39631b92e2b2106e71b0d
ms.sourcegitcommit: 9d9795f8a5b50cd5ccc19d3a2773817836446912
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/14/2020
ms.locfileid: "88246471"
---
### <a name="app-service-plan"></a>应用服务计划
创建用于托管 Web 应用的服务计划。 通过 **hostingPlanName** 参数提供计划的名称。 计划的位置与用于资源组的位置相同。 定价层和辅助角色大小在 **sku** 和 **workerSize** 参数中指定

```config
{
  "apiVersion": "2015-08-01",
  "name": "[parameters('hostingPlanName')]",
  "type": "Microsoft.Web/serverfarms",
  "location": "[resourceGroup().location]",
  "sku": {
    "name": "[parameters('sku')]",
    "capacity": "[parameters('workerSize')]"
  },
  "properties": {
    "name": "[parameters('hostingPlanName')]"
  }
},
```
