---
title: Azure PowerShell 脚本示例 - 添加用户
description: Azure PowerShell 脚本示例 - 添加用户
services: api-management
documentationcenter: ''
author: vladvino
manager: cfowler
editor: ''
ms.service: api-management
ms.workload: mobile
ms.devlang: na
ms.topic: sample
origin.date: 11/16/2017
ms.date: 03/11/2019
ms.author: apimpm
ms.custom: mvc
ms.openlocfilehash: 59b2b5e4b183028f83c14b325aab754137e44fac
ms.sourcegitcommit: 2b4507745b98b45f1ce3f3d30f397521148ef35a
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/02/2020
ms.locfileid: "78213716"
---
# <a name="add-a-user"></a>添加用户

此示例脚本在 API 管理中创建用户并获取订阅密钥。

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

[!INCLUDE [cloud-shell-powershell.md](../../../includes/cloud-shell-powershell.md)]

如果选择在本地安装并使用 PowerShell，则本教程需要 Azure PowerShell 模块 1.0 或更高版本。 运行 `Get-Module -ListAvailable Az` 即可查找版本。 如果需要进行升级，请参阅 [Install Azure PowerShell module](https://docs.microsoft.com//powershell/azure/install-Az-ps)（安装 Azure PowerShell 模块）。 如果在本地运行 PowerShell，则还需运行 `Connect-AzAccount` 来创建与 Azure 的连接。

## <a name="sample-script"></a>示例脚本

```powershell
##########################################################
#  Script to create a user in api management and get a subscription key
###########################################################

$random = (New-Guid).ToString().Substring(0,8)

#Azure specific details
$subscriptionId = "my-azure-subscription-id"

# Api Management service specific details
$apimServiceName = "apim-$random"
$resourceGroupName = "apim-rg-$random"
$location = "China East"
$organisation = "Contoso"
$adminEmail = "admin@contoso.com"

# User specific details
$userEmail = "user@contoso.com"
$userFirstName = "userFirstName"
$userLastName = "userLastName"
$userPassword = "userPassword"
$userNote = "fellow trying out my apim instance"
$userState = "Active"

# Subscription Name details
$subscriptionName = "subscriptionName"
$subscriptionState = "Active"

# Set the context to the subscription Id where the cluster will be created
Select-AzureRmSubscription -SubscriptionId $subscriptionId

# Create a resource group.
New-AzureRmResourceGroup -Name $resourceGroupName -Location $location

# Create the Api Management service. Since the SKU is not specified, it creates a service with Developer SKU. 
New-AzureRmApiManagement -ResourceGroupName $resourceGroupName -Name $apimServiceName -Location $location -Organization $organisation -AdminEmail $adminEmail

# Create the api management context
$context = New-AzureRmApiManagementContext -ResourceGroupName $resourceGroupName -ServiceName $apimServiceName

# create a new user in api management
$user = New-AzureRmApiManagementUser -Context $context -FirstName $userFirstName -LastName $userLastName `
    -Password $userPassword -State $userState -Note $userNote -Email $userEmail

# get the details of the 'Starter' product in api management, which is created by default
$product = Get-AzureRmApiManagementProduct -Context $context -Title 'Starter' | Select-Object -First 1

# generate a subscription key for the user to call apis which are part of the 'Starter' product
New-AzureRmApiManagementSubscription -Context $context -UserId $user.UserId `
    -ProductId $product.ProductId -Name $subscriptionName -State $subscriptionState
```

## <a name="clean-up-resources"></a>清理资源

如果不再需要资源组和所有相关资源，可以使用 [Remove-AzResourceGroup](https://docs.microsoft.com//powershell/module/az.resources/remove-azresourcegroup) 命令将其删除。

```azurepowershell-interactive
Remove-AzResourceGroup -Name myResourceGroup
```

## <a name="next-steps"></a>后续步骤

有关 Azure PowerShell 模块的详细信息，请参阅 [Azure PowerShell 文档](https://docs.microsoft.com/powershell/azure/overview)。

可以在 [PowerShell 示例](../powershell-samples.md)中找到 Azure API 管理的其他 Azure Powershell 示例。
