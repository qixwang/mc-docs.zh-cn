---
title: Azure PowerShell 脚本示例 - 保护后端
description: Azure PowerShell 脚本示例 - 保护后端
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
ms.openlocfilehash: 9b293fddf3ebd0a93b46e3aa1696ec8ca686ca4a
ms.sourcegitcommit: c1ba5a62f30ac0a3acb337fb77431de6493e6096
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2020
ms.locfileid: "78213712"
---
# <a name="secure-back-end"></a>保护后端

此示例脚本通过相互证书身份验证保护后端。

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

如果选择在本地安装并使用 PowerShell，则本教程需要安装 Azure PowerShell 模块 1.0 或更高版本。 运行 `Get-Module -ListAvailable Az` 即可查找版本。 如果需要升级，请参阅[安装 Azure PowerShell 模块](https://docs.microsoft.com//powershell/azure/install-Az-ps)。 如果在本地运行 PowerShell，则还需运行 `Connect-AzAccount` 来创建与 Azure 的连接。

## <a name="sample-script"></a>示例脚本

```powershell
##########################################################
#  Script to setup backend mutual authentication using certificates
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

# Certificate needed for Custom Domain Setup
$certificateFilePath = "<Replace with path to the Certificate to be used for Mutual Authentication>"
$certificatePassword = '<Password used to secure the Certificate>'

# Set the context to the subscription Id where the cluster will be created
Select-AzureRmSubscription -SubscriptionId $subscriptionId

# Create a resource group.
New-AzureRmResourceGroup -Name $resourceGroupName -Location $location

# Create the Api Management service. Since the SKU is not specified, it creates a service with Developer SKU. 
New-AzureRmApiManagement -ResourceGroupName $resourceGroupName -Name $apimServiceName -Location $location -Organization $organisation -AdminEmail $adminEmail

# Create the api management context
$context = New-AzureRmApiManagementContext -ResourceGroupName $resourceGroupName -ServiceName $apimServiceName

# upload the certificate
$cert = New-AzureRmApiManagementCertificate -Context $context -PfxFilePath $certificateFilePath -PfxPassword $certificatePassword

# create an authentication-certificate policy with the thumbprint of the certificate
$apiPolicy = "<policies><inbound><base /><authentication-certificate thumbprint=""" + $cert.Thumbprint + """ /></inbound><backend><base /></backend><outbound><base /></outbound><on-error><base /></on-error></policies>"
$echoApi = Get-AzureRmApiManagementApi -Context $context -Name "Echo API"

# setup Policy at the Product Level. Policies can be applied at entire API Management Service Scope, Api Scope, Product Scope and Api Operation Scope
Set-AzureRmApiManagementPolicy -Context $context  -Policy $apiPolicy -ApiId $echoApi.ApiId
```

## <a name="clean-up-resources"></a>清理资源

如果不再需要资源组和所有相关资源，可以使用 [Remove-AzResourceGroup](https://docs.microsoft.com//powershell/module/az.resources/remove-azresourcegroup) 命令将其删除。

```azurepowershell
Remove-AzResourceGroup -Name myResourceGroup
```

## <a name="next-steps"></a>后续步骤

有关 Azure PowerShell 模块的详细信息，请参阅 [Azure PowerShell 文档](https://docs.microsoft.com/powershell/azure/overview)。

可以在 [PowerShell 示例](../powershell-samples.md)中找到 Azure API 管理的其他 Azure Powershell 示例。
