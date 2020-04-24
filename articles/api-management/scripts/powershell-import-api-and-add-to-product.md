---
title: Azure PowerShell 脚本示例 - 导入 API
description: Azure PowerShell 脚本示例 - 导入 API
services: api-management
documentationcenter: ''
author: juliako
manager: cfowler
editor: ''
ms.service: api-management
ms.workload: mobile
ms.devlang: na
ms.topic: sample
origin.date: 11/16/2017
ms.date: 08/13/2018
ms.author: v-yiso
ms.custom: mvc
ms.openlocfilehash: 1092efd90cd00204d2569746f252b4cf10c7480f
ms.sourcegitcommit: c1ba5a62f30ac0a3acb337fb77431de6493e6096
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2020
ms.locfileid: "78213714"
---
# <a name="import-an-api"></a>导入 API

此示例脚本导入 API，并将其添加到“API 管理”产品。 

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]
如果选择在本地安装并使用 PowerShell，则本教程需要安装 Azure PowerShell 模块 1.0 或更高版本。 运行 `Get-Module -ListAvailable Az` 即可查找版本。 如果需要升级，请参阅[安装 Azure PowerShell 模块](https://docs.microsoft.com//powershell/azure/install-Az-ps)。 如果在本地运行 PowerShell，则还需运行 `Connect-AzAccount` 来创建与 Azure 的连接。

## <a name="sample-script"></a>示例脚本

```powershell
##########################################################
#  Script to import an API and add it to a Product in api Management 
#  Adding the Imported api to a product is necessary, so that it can be called by a subscription
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

# Api Specific Details
$swaggerUrl = "http://petstore.swagger.io/v2/swagger.json"
$apiPath = "petstore"

# Set the context to the subscription Id where the cluster will be created
Select-AzureRmSubscription -SubscriptionId $subscriptionId

# Create a resource group.
New-AzureRmResourceGroup -Name $resourceGroupName -Location $location

# Create the Api Management service. Since the SKU is not specified, it creates a service with Developer SKU. 
New-AzureRmApiManagement -ResourceGroupName $resourceGroupName -Name $apimServiceName -Location $location -Organization $organisation -AdminEmail $adminEmail

# Create the API Management context
$context = New-AzureRmApiManagementContext -ResourceGroupName $resourceGroupName -ServiceName $serviceName

# import api from Url
$api = Import-AzureRmApiManagementApi -Context $context -SpecificationUrl $swaggerUrl -SpecificationFormat Swagger -Path $apiPath

$productName = "Pet Store Product"
$productDescription = "Product giving access to Petstore api"
$productState = "Published"

# Create a Product to publish the Imported Api. This creates a product with a limit of 10 Subscriptions
$product = New-AzureRmApiManagementProduct -Context $context -Title $productName -Description $productDescription -State $productState -SubscriptionsLimit 10 

# Add the petstore api to the published Product, so that it can be called in developer portal console
Add-AzureRmApiManagementApiToProduct -Context $context -ProductId $product.ProductId -ApiId $api.ApiId
```

## <a name="clean-up-resources"></a>清理资源

如果不再需要资源组和所有相关资源，可以使用 [Remove-AzResourceGroup](https://docs.microsoft.com//powershell/module/az.resources/remove-azresourcegroup) 命令将其删除。

```azurepowershell
Remove-AzResourceGroup -Name myResourceGroup
```

## <a name="next-steps"></a>后续步骤

有关 Azure PowerShell 模块的详细信息，请参阅 [Azure PowerShell 文档](https://docs.microsoft.com/powershell/azure/overview)。

可以在 [PowerShell 示例](../powershell-samples.md)中找到 Azure API 管理的其他 Azure Powershell 示例。
