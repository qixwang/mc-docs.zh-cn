---
author: cephalin
ms.service: app-service-web
ms.topic: include
origin.date: 11/03/2016
ms.date: 11/03/2016
ms.author: v-tawe
ms.openlocfilehash: a95d30c5dc7b19b60f6a1e339b3bbd468603adb9
ms.sourcegitcommit: c1ba5a62f30ac0a3acb337fb77431de6493e6096
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2020
ms.locfileid: "71059574"
---
### <a name="app-service-plan"></a>应用服务计划
创建用于托管 Web 应用的服务计划。 通过 **hostingPlanName** 参数提供计划的名称。 计划的位置与用于资源组的位置相同。 定价层和辅助角色大小在 **sku** 和 **workerSize** 参数中指定

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