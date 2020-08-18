---
title: Azure 资源管理器模板示例 - 事件网格 | Microsoft Docs
description: 本文提供了 GitHub 上适用于 Azure 事件网格的 Azure 资源管理器模板示例的列表。
ms.topic: sample
author: Johnnytechn
ms.author: v-johya
origin.date: 01/23/2020
ms.date: 08/10/2020
ms.openlocfilehash: 63e52eb65d80a9ce7f1b88972f98467522112a12
ms.sourcegitcommit: 9d9795f8a5b50cd5ccc19d3a2773817836446912
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/14/2020
ms.locfileid: "88228434"
---
# <a name="azure-resource-manager-templates-for-event-grid"></a>事件网格的 Azure 资源管理器模板

有关要在模板中使用的 JSON 语法和属性，请参阅 [Microsoft.EventGrid 资源类型](https://docs.microsoft.com/azure/templates/microsoft.eventgrid/allversions)。 下表包含事件网格的 Azure 资源管理器模板链接。

## <a name="event-grid-subscriptions"></a>事件网格订阅
- [使用 WebHook 终结点的自定义主题和订阅](https://github.com/Azure/azure-quickstart-templates/tree/master/101-event-grid) - 部署事件网格自定义主题。 创建使用 WebHook 终结点的自定义主题的订阅。 
- [使用事件中心终结点的自定义主题订阅](https://github.com/Azure/azure-quickstart-templates/tree/master/101-event-grid-event-hubs-handler) - 创建自定义主题的事件网格订阅。 该订阅使用事件中心作为终结点。 
- [Azure 订阅或资源组订阅](https://github.com/Azure/azure-quickstart-templates/tree/master/101-event-grid-resource-events-to-webhook) - 订阅资源组或 Azure 订阅的事件。 在部署期间指定为目标的资源组是事件的源。 该订阅使用 WebHook 作为终结点。 
- [Blob 存储帐户和订阅](https://github.com/Azure/azure-quickstart-templates/tree/master/101-event-grid-subscription-and-storage) - 部署 Azure Blob 存储帐户并订阅该存储帐户的事件。 

## <a name="next-steps"></a>后续步骤
请参阅以下示例：

- [PowerShell 示例](powershell-samples.md)
- [CLI 示例](cli-samples.md)

