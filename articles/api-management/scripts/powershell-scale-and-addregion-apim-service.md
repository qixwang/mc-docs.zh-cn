---
title: Azure PowerShell 脚本示例 - 缩放服务实例
description: Azure PowerShell 脚本示例 - 缩放服务实例
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
ms.author: v-yiso
ms.custom: mvc
ms.openlocfilehash: 62a0c008bbfcbc4c5e5ca29f24d085a2e3e8c63f
ms.sourcegitcommit: c1ba5a62f30ac0a3acb337fb77431de6493e6096
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2020
ms.locfileid: "78213709"
---
# <a name="scale-the-service-instance"></a>缩放服务实例

此示例脚本可缩放 API 管理服务实例，并向其添加区域。

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

如果选择在本地安装并使用 PowerShell，则本教程需要安装 Azure PowerShell 模块 1.0 或更高版本。 运行 `Get-Module -ListAvailable Az` 即可查找版本。 如果需要升级，请参阅[安装 Azure PowerShell 模块](https://docs.microsoft.com//powershell/azure/install-Az-ps)。 如果在本地运行 PowerShell，则还需运行 `Connect-AzAccount` 来创建与 Azure 的连接。

## <a name="sample-script"></a>示例脚本

```powershell
##########################################################
#  Script to create an apim service and scale to premium 
#  with an additional region.
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

# Set the context to the subscription Id where the cluster will be created
Select-AzureRmSubscription -SubscriptionId $subscriptionId

# Create a resource group.
New-AzureRmResourceGroup -Name $resourceGroupName -Location $location

# Create the Api Management service. Since the SKU is not specified, it creates a service with Developer SKU. 
New-AzureRmApiManagement -ResourceGroupName $resourceGroupName -Name $apimServiceName -Location $location -Organization $organisation -AdminEmail $adminEmail

# Scale master region to 'Premium' 1
$sku = "Premium"
$capacity = 1

# Add new 'Premium' region 1 unit
$additionLocation = Get-ProviderLocations "Microsoft.ApiManagement/service" | Where-Object {$_ -ne $location} | Select-Object -First 1

Get-AzureRmApiManagement -ResourceGroupName $resourceGroupName -Name $apimServiceName |
Update-AzureRmApiManagementRegion -Sku $sku -Capacity $capacity |
Add-AzureRmApiManagementRegion -Location $additionLocation -Sku $sku |
Update-AzureRmApiManagementDeployment

Get-AzureRmApiManagement -ResourceGroupName $resourceGroupName -Name $apimServiceName
```

## <a name="clean-up-resources"></a>清理资源

如果不再需要资源组和所有相关资源，可以使用 [Remove-AzResourceGroup](/powershell/module/az.resources/remove-azresourcegroup) 命令将其删除。

```azurepowershell
Remove-AzResourceGroup -Name myResourceGroup
```

## <a name="next-steps"></a>后续步骤

有关 Azure PowerShell 模块的详细信息，请参阅 [Azure PowerShell 文档](https://docs.microsoft.com/powershell/azure/overview)。

可以在 [PowerShell 示例](../powershell-samples.md)中找到 Azure API 管理的其他 Azure Powershell 示例。
