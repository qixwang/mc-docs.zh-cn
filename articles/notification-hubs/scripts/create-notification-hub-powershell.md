---
title: 使用 PowerShell 创建 Azure 通知中心 | Azure
description: 了解如何使用 PowerShell 脚本创建 Azure 通知中心。
author: dimazaid
manager: femila
services: notification-hubs
editor: sethmanheim
ms.service: notification-hubs
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
origin.date: 01/14/2020
ms.date: 03/02/2020
ms.author: v-tawe
ms.openlocfilehash: 2657862bec470ceb54a53632f647c2c37ff87125
ms.sourcegitcommit: 094c057878de233180ff3b3a3e3c19bc11c81776
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/20/2020
ms.locfileid: "77501449"
---
# <a name="use-powershell-to-create-an-azure-notification-hub"></a>使用 PowerShell 创建 Azure 通知中心

此示例 PowerShell 脚本可创建示例 Azure 通知中心。 

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install-no-ssh.md)]

## <a name="prerequisites"></a>必备条件

* **Azure 订阅** - 如果没有 Azure 订阅，请在开始前创建一个[试用帐户](https://www.azure.cn/pricing/1rmb-trial/)。

## <a name="sample-script"></a>示例脚本

```powershell
# Set appropriate values for these variables
$resourceGroupName = "<Enter a name for the resource group>"
$nhubnamespace = "<Enter a name for the notification hub namespace>"
$location = "China East"

# Create a resource group.
New-AzResourceGroup -Name $resourceGroupName -Location $location

# Create a namespace for the resource group
New-AzNotificationHubsNamespace -ResourceGroup $resourceGroupName -Namespace $nhubnamespace -Location $location

# Create an input JSON file that you use with the New-AzNotificationHub command
$text = '{"name": "MyNotificationHub",  "Location": "China East",  "Properties": {  }}'
$text | Out-File "inputfile2.json"

# Create a notification hub
New-AzNotificationHub -ResourceGroup $resourceGroupName -Namespace $nhubnamespace -InputFile .\inputfile.json

```

## <a name="clean-up-deployment"></a>清理部署

运行示例脚本后，可以使用以下命令删除资源组以及与其关联的所有资源：

```powershell
Remove-AzResourceGroup -ResourceGroupName $resourceGroupName
```

## <a name="script-explanation"></a>脚本说明

此脚本使用以下命令：

| 命令 | 注释 |
|---|---|
| [New-AzResourceGroup](https://docs.microsoft.com/powershell/module/az.resources/new-azresourcegroup) | 创建用于存储所有资源的资源组。 |
| [New-AzNotificationHubsNamespace](https://docs.microsoft.com/powershell/module/az.notificationhubs/new-aznotificationhubsnamespace) | 为通知中心创建命名空间。 |
| [New-AzNotificationHub](https://docs.microsoft.com/powershell/module/az.notificationhubs/new-aznotificationhub) | 创建通知中心。 |
| [Remove-AzResourceGroup](https://docs.microsoft.com/powershell/module/az.resources/remove-azresourcegroup) | 删除资源组，包括所有嵌套的资源。 |
|||

## <a name="next-steps"></a>后续步骤

有关 Azure PowerShell 的详细信息，请参阅 [Azure PowerShell 文档](https://docs.microsoft.com/powershell/)。
