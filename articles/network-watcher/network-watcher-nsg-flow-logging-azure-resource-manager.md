---
title: 网络观察程序 - 使用 Azure 资源管理器模板创建 NSG 流日志
description: 使用 Azure 资源管理器模板和 PowerShell 轻松设置 NSG 流日志。
services: network-watcher
documentationcenter: na
author: damendo
manager: twooley
editor: ''
tags: azure-resource-manager
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 04/30/2020
ms.author: v-tawe
origin.date: 01/26/2020
ms.openlocfilehash: bbe7345b1bdb232843a002b3231c81e800a97aec
ms.sourcegitcommit: b81ea2ab9eafa986986fa3eb1e784cfe9bbf9ec1
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/13/2020
ms.locfileid: "83367872"
---
# <a name="configure-nsg-flow-logs-from-an-azure-resource-manager-template"></a>通过 Azure 资源管理器模板配置 NSG 流日志

> [!div class="op_single_selector"]
> - [Azure 门户](network-watcher-nsg-flow-logging-portal.md)
> - [PowerShell](network-watcher-nsg-flow-logging-powershell.md)
> - [CLI](network-watcher-nsg-flow-logging-cli.md)
> - [REST API](network-watcher-nsg-flow-logging-rest.md)
> - [Azure Resource Manager](network-watcher-nsg-flow-logging-azure-resource-manager.md)


[Azure 资源管理器](https://docs.azure.cn/azure-resource-manager/)是 Azure 的本机方法且功能强大，可以将基础结构作为代码进行管理。

本文介绍如何使用 Azure 资源管理器模板和 Azure PowerShell 以编程方式启用 [NSG 流日志](https://docs.azure.cn/network-watcher/network-watcher-nsg-flow-logging-overview)。 我们首先概述 NSG 流日志对象的属性，然后介绍几个示例模板。 然后，使用本地 PowerShell 实例部署模板。


## <a name="nsg-flow-logs-object"></a>NSG 流日志对象

下面显示了具有所有参数的 NSG 流日志对象。


```json
{
  "name": "string",
  "type": "Microsoft.Network/networkWatchers/flowLogs",
  "location": "string",
  "apiVersion": "2019-09-01",
  "properties": {
    "targetResourceId": "string",
    "storageId": "string",
    "enabled": "boolean",
    "flowAnalyticsConfiguration": {
      "networkWatcherFlowAnalyticsConfiguration": {
         "enabled": "boolean",
         "workspaceResourceId": "string",
          "trafficAnalyticsInterval": "integer"
        },
        "retentionPolicy": {
           "days": "integer",
           "enabled": "boolean"
         },
        "format": {
           "type": "string",
           "version": "integer"
         }
      }
    }
  }
```
若要创建 Microsoft.Network/networkWatchers/flowLogs 资源，请将上面的 JSON 添加到模板的 resources 节。


## <a name="creating-your-template"></a>创建模板

如果是首次使用 Azure 资源管理器模板，可以通过以下链接了解有关这些模板的详细信息。

* [使用 Resource Manager 模板和 Azure PowerShell 部署资源](https://docs.azure.cn/azure-resource-manager/templates/deploy-powershell#deploy-local-template)
* [教程：创建和部署你的第一个 Azure 资源管理器模板](https://docs.azure.cn/azure-resource-manager/templates/template-tutorial-create-first-template?tabs=azure-powershell)


下面是用于设置 NSG 流日志的完整模板的两个示例。

**示例 1**：上述版本中最简单的版本，传递的参数最少。 下面的模板启用目标 NSG 上的 NSG 流日志并将其存储在给定存储帐户中。

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "apiProfile": "2019-09-01",
  "resources": [
 {
    "name": "NetworkWatcher_centraluseuap/Microsoft.NetworkDalanDemoPerimeterNSG",
    "type": "Microsoft.Network/networkWatchers/FlowLogs/",
    "location": "centraluseuap",
    "apiVersion": "2019-09-01",
    "properties": {
      "targetResourceId": "/subscriptions/56abfbd6-ec72-4ce9-831f-bc2b6f2c5505/resourceGroups/DalanDemo/providers/Microsoft.Network/networkSecurityGroups/PerimeterNSG",
      "storageId": "/subscriptions/56abfbd6-ec72-4ce9-831f-bc2b6f2c5505/resourceGroups/MyCanaryFlowLog/providers/Microsoft.Storage/storageAccounts/storagev2ira",
      "enabled": true,
      "flowAnalyticsConfiguration": {},
      "retentionPolicy": {},
      "format": {}
    }

  }
  ]
}
```

> [!NOTE]
> * 资源名称的格式为“父资源>/子资源”。 此处的父资源是区域网络观察程序实例（格式：NetworkWatcher_<RegionName>。 示例：NetworkWatcher_centraluseuap）
> * targetResourceId 是目标 NSG 的资源 ID
> * storageId 是目标存储帐户的资源 ID

**示例 2**：以下模板启用保留期为 5 天的 NSG 流日志（版本 2）。 启用处理间隔为 10 分钟的流量分析。

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "apiProfile": "2019-09-01",
  "resources": [
 {
    "name": "NetworkWatcher_centraluseuap/Microsoft.NetworkDalanDemoPerimeterNSG",
    "type": "Microsoft.Network/networkWatchers/FlowLogs/",
    "location": "centraluseuap",
    "apiVersion": "2019-09-01",
    "properties": {
      "targetResourceId": "/subscriptions/56abfbd6-ec72-4ce9-831f-bc2b6f2c5505/resourceGroups/DalanDemo/providers/Microsoft.Network/networkSecurityGroups/PerimeterNSG",
      "storageId": "/subscriptions/56abfbd6-ec72-4ce9-831f-bc2b6f2c5505/resourceGroups/MyCanaryFlowLog/providers/Microsoft.Storage/storageAccounts/storagev2ira",
      "enabled": true,
      "flowAnalyticsConfiguration": {
        "networkWatcherFlowAnalyticsConfiguration": {
            "enabled": true,
            "workspaceResourceId": "/subscriptions/56abfbd6-ec72-4ce9-831f-bc2b6f2c5505/resourceGroups/defaultresourcegroup-wcus/providers/Microsoft.OperationalInsights/workspaces/1c4f42e5-3a02-4146-ac9b-3051d8501db0",
            "trafficAnalyticsInterval": 10
                }
      },
      "retentionPolicy": {
        "days": 5,
        "enabled": true
      },
      "format": {
        "type": "JSON",
        "version": 2            
      }
    }

  }
  ]
}
```

## <a name="deploying-your-azure-resource-manager-template"></a>部署 Azure 资源管理器模板

本教程假设你有一个现有的资源组和一个可以在其上启用流日志记录的 NSG。
你可以在本地将上述任何示例模板另存为 `azuredeploy.json`。 更新属性值，使其指向订阅中的有效资源。

若要部署模板，请在 PowerShell 中运行以下命令。
```azurepowershell
New-AzResourceGroupDeployment -Name EnableFlowLog -ResourceGroupName NetworkWatcherRG `
    -TemplateFile "C:\MyTemplates\azuredeploy.json"
```


## <a name="verifying-your-deployment"></a>验证部署

可以通过多种方法来检查部署是否成功。 PowerShell 控制台应显示“ProvisioningState”为“Succeeded”。 此外，还可以访问 [NSG 流日志门户页](https://portal.azure.cn/#blade/Microsoft_Azure_Network/NetworkWatcherMenuBlade/flowLogs)来确认所做的更改。 如果部署出现问题，请参阅[排查使用 Azure 资源管理器时的常见 Azure 部署错误](https://docs.azure.cn/azure-resource-manager/templates/common-deployment-errors)。


## <a name="next-steps"></a>后续步骤

了解如何使用以下工具直观显示 NSG 流数据：
* [Microsoft Power BI](network-watcher-visualize-nsg-flow-logs-power-bi.md)
* [开源工具](network-watcher-visualize-nsg-flow-logs-open-source-tools.md)
* [Azure 流量分析](https://docs.azure.cn/network-watcher/traffic-analytics)
