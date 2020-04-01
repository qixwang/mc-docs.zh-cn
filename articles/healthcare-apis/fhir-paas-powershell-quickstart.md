---
title: 快速入门：使用 PowerShell 部署 Azure API for FHIR
description: 本快速入门介绍如何使用 PowerShell 部署 Azure API for FHIR。
services: healthcare-apis
author: hansenms
ms.service: healthcare-apis
ms.subservice: fhir
ms.topic: quickstart
ms.date: 10/15/2019
ms.author: mihansen
ms.openlocfilehash: 64b1d67d46d41356fd98942970cfb96b2192d5e9
ms.sourcegitcommit: 6ddc26f9b27acec207b887531bea942b413046ad
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/27/2020
ms.locfileid: "80343834"
---
# <a name="quickstart-deploy-azure-api-for-fhir-using-powershell"></a>快速入门：使用 PowerShell 部署 Azure API for FHIR

本快速入门介绍如何使用 PowerShell 部署 Azure API for FHIR。

如果没有 Azure 订阅，可在开始前创建一个[试用帐户](https://www.azure.cn/pricing/1rmb-trial)。

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="register-the-azure-api-for-fhir-resource-provider"></a>注册 Azure API for FHIR 资源提供程序

如果尚未为订阅注册 `Microsoft.HealthcareApis` 资源提供程序，可使用以下命令注册它：

```azurepowershell
Register-AzResourceProvider -ProviderNamespace Microsoft.HealthcareApis
```

## <a name="locate-your-identity-object-id"></a>找到标识对象 ID

对象 ID 值是与订阅关联的目录中特定 Azure Active Directory 用户或服务主体的对象 ID 相对应的 GUID。 如果你想要知道特定用户的对象 ID，可使用如下命令找到它：

```azurepowershell
$(Get-AzureADUser -Filter "UserPrincipalName eq 'myuser@consoso.com'").ObjectId
```

如需更多详细信息，请阅读有关[查找标识对象 ID](find-identity-object-ids.md) 的操作指南。

## <a name="create-azure-resource-group"></a>创建 Azure 资源组

```azurepowershell
New-AzResourceGroup -Name "myResourceGroupName" -Location westus2
```

## <a name="deploy-azure-api-for-fhir"></a>部署 Azure API for FHIR

```azurepowershell
New-AzHealthcareApisService -Name nameoffhirservice -ResourceGroupName myResourceGroupName -Location westus2 -Kind fhir-R4 -AccessPolicyObjectId "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
```

其中，`xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx` 是你希望其有权访问 FHIR API 的用户或服务主体的标识对象 ID。

## <a name="fetch-capability-statement"></a>提取功能语句

可以通过提取 FHIR 功能语句来验证 Azure API for FHIR 帐户是否正在运行：

```azurepowershell
$metadata = Invoke-WebRequest -Uri "https://nameoffhirservice.azurehealthcareapis.com/metadata"
$metadata.RawContent
```

## <a name="clean-up-resources"></a>清理资源

如果你不打算继续使用此应用程序，请按以下步骤删除资源组：

```azurepowershell
Remove-AzResourceGroup -Name myResourceGroupName
```

## <a name="next-steps"></a>后续步骤

在本快速入门指南中，你已将 Azure API for FHIR 部署到订阅中。 若要在 Azure API for FHIR 中指定其他设置，请转到有关其他设置的操作指南。

>[!div class="nextstepaction"]
>[Azure API for FHIR 中的其他设置](azure-api-for-fhir-additional-settings.md)
