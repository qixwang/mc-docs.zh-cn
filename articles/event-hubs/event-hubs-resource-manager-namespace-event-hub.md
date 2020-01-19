---
title: 快速入门：创建包含使用者组的事件中心 - Azure 事件中心
description: 快速入门：使用 Azure Resource Manager 模板创建包含事件中心和使用者组的事件中心命名空间
services: event-hubs
documentationcenter: .net
author: ShubhaVijayasarathy
manager: timlt
editor: ''
ms.assetid: 28bb4591-1fd7-444f-a327-4e67e8878798
ms.service: event-hubs
ms.devlang: tbd
ms.topic: quickstart
ms.tgt_pltfrm: dotnet
ms.workload: na
origin.date: 11/05/2019
ms.date: 01/17/2020
ms.author: v-tawe
ms.openlocfilehash: 79540933bd2e11d521210fae3dbb7dd6901ec52e
ms.sourcegitcommit: 94e1c9621b8f81a7078f1412b3a73281d0a8668b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/16/2020
ms.locfileid: "76123205"
---
# <a name="quickstart-create-an-event-hub-by-using-an-azure-resource-manager-template"></a>快速入门：使用 Azure 资源管理器模板创建事件中心

Azure 事件中心是一个大数据流式处理平台和事件引入服务，每秒能够接收和处理数百万个事件。 事件中心可以处理和存储分布式软件和设备生成的事件、数据或遥测。 可以使用任何实时分析提供程序或批处理/存储适配器转换和存储发送到数据中心的数据。 有关事件中心的详细概述，请参阅[事件中心概述](event-hubs-about.md)和[事件中心功能](event-hubs-features.md)。

在此快速入门中，使用 [Azure 资源管理器模板](../azure-resource-manager/resource-group-overview.md)创建事件中心。 部署 Azure 资源管理器模板以创建包含一个事件中心的类型为[事件中心](event-hubs-what-is-event-hubs.md)的命名空间。 本文介绍如何定义要部署的资源以及如何定义执行部署时指定的参数。 可将此模板用于自己的部署，或自定义此模板以满足要求。 有关创建模板的信息，请参阅[创作 Azure 资源管理器模板][Authoring Azure Resource Manager templates]。

如果没有 Azure 订阅，请在开始前[创建一个试用帐户](https://www.azure.cn/pricing/1rmb-trial)。

## <a name="create-an-event-hub"></a>创建事件中心

本快速入门使用[现有快速入门模板](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-eventhubs-create-namespace-and-eventhub/azuredeploy.json)：

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "projectName":{
      "type": "string",
      "metadata": {
        "description": "Specifies a project name that is used to generate the Event Hub name and the Namespace name."
      }
    },
    "location": {
      "type": "string",
      "defaultValue": "[resourceGroup().location]",
      "metadata": {
        "description": "Specifies the Azure location for all resources."
      }
    },
    "eventHubSku": {
      "type": "string",
      "allowedValues": [ "Basic", "Standard" ],
      "defaultValue": "Standard",
      "metadata": {
        "description": "Specifies the messaging tier for service Bus namespace."
      }
    }
  },
  "variables": {
    "eventHubNamespaceName": "[concat(parameters('projectName'), 'ns')]",
    "eventHubName": "[parameters('projectName')]"
  },
  "resources": [
    {
      "apiVersion": "2017-04-01",
      "type": "Microsoft.EventHub/namespaces",
      "name": "[variables('eventHubNamespaceName')]",
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
    },
    {
      "apiVersion": "2017-04-01",
      "type": "Microsoft.EventHub/namespaces/eventhubs",
      "name": "[concat(variables('eventHubNamespaceName'), '/', variables('eventHubName'))]",
      "location": "[parameters('location')]",
      "dependsOn": [
        "[resourceId('Microsoft.EventHub/namespaces', variables('eventHubNamespaceName'))]"
      ],
      "properties": {
        "messageRetentionInDays": 7,
        "partitionCount": 1
      }
    }
  ]
}
```

若要查找更多模板示例，请参阅 [Azure 快速启动模板](https://azure.microsoft.com/resources/templates/?term=eventhub&pageNumber=1&sort=Popular)。

若要部署模板，请执行以下操作：

1. 从以下代码块中选择“试用”  ，然后按照说明登录 Azure Cloud shell。

   ```azurepowershell
   $projectName = Read-Host -Prompt "Enter a project name that is used for generating resource names"
   $location = Read-Host -Prompt "Enter the location (i.e. chinaeast)"
   $resourceGroupName = "${projectName}rg"
   $templateUri = "https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-eventhubs-create-namespace-and-eventhub/azuredeploy.json"

   New-AzResourceGroup -Name $resourceGroupName -Location $location
   New-AzResourceGroupDeployment -ResourceGroupName $resourceGroupName -TemplateUri $templateUri -projectName $projectName

   Write-Host "Press [ENTER] to continue ..."
   ```

   创建事件中心需要一些时间。

1. 选择“复制”以复制 PowerShell 脚本。 
1. 右键单击 shell 控制台，然后选择“粘贴”  。

## <a name="verify-the-deployment"></a>验证部署

若要验证部署，可以从 [Azure 门户](https://portal.azure.cn)打开资源组，也可以使用以下 Azure PowerShell 脚本。

```azurepowershell
$projectName = Read-Host -Prompt "Enter the same project name that you used in the last procedure"
$resourceGroupName = "${projectName}rg"
$namespaceName = "${projectName}ns"

Get-AzEventHub -ResourceGroupName $resourceGroupName -Namespace $namespaceName

Write-Host "Press [ENTER] to continue ..."
```

## <a name="clean-up-resources"></a>清理资源

不再需要 Azure 资源时，请通过删除资源组来清理部署的资源。

```azurepowershell
$projectName = Read-Host -Prompt "Enter the same project name that you used in the last procedure"
$resourceGroupName = "${projectName}rg"

Remove-AzResourceGroup -ResourceGroupName $resourceGroupName

Write-Host "Press [ENTER] to continue ..."
```

## <a name="next-steps"></a>后续步骤

在本文中，你创建了一个事件中心命名空间，并在该命名空间中创建了一个事件中心。 有关如何将事件发送到事件中心（或）从事件中心接收事件的分步说明，请参阅“发送和接收事件”教程  ：

- [.NET Core](event-hubs-dotnet-standard-getstarted-send.md)
- [.NET framework](event-hubs-dotnet-framework-getstarted-send.md)
- [Java](event-hubs-java-get-started-send.md)
- [Python](event-hubs-python-get-started-send.md)
- [Node.js](event-hubs-node-get-started-send.md)
- [Go](event-hubs-go-get-started-send.md)
- [C（仅发送）](event-hubs-c-getstarted-send.md)
- [Apache Storm（仅接收）](event-hubs-storm-getstarted-receive.md)


[3]: ./media/event-hubs-quickstart-powershell/sender1.png
[4]: ./media/event-hubs-quickstart-powershell/receiver1.png
[5]: ./media/event-hubs-quickstart-powershell/metrics.png

[Authoring Azure Resource Manager templates]: ../azure-resource-manager/resource-group-authoring-templates.md
[Azure Quickstart Templates]:  https://azure.microsoft.com/documentation/templates/?term=event+hubs
[Using Azure PowerShell with Azure Resource Manager]: ../powershell-azure-resource-manager.md
[Using the Azure CLI for Mac, Linux, and Windows with Azure Resource Management]: ../azure-resource-manager/manage-resources-cli.md
[Event hub and consumer group template]: https://github.com/Azure/azure-quickstart-templates/blob/master/201-event-hubs-create-event-hub-and-consumer-group/
