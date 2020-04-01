---
title: PowerShell：部署适用于 Azure 的 FHIR 服务器 - Azure API for FHIR
description: 本快速入门介绍如何使用 PowerShell 部署 Microsoft 开源 FHIR 服务器。
services: healthcare-apis
author: hansenms
ms.service: healthcare-apis
ms.subservice: fhir
ms.topic: quickstart
ms.date: 02/07/2019
ms.author: mihansen
ms.openlocfilehash: 8f93464e8942b96797df09a0e7fc32fa1d4688e4
ms.sourcegitcommit: 6ddc26f9b27acec207b887531bea942b413046ad
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/27/2020
ms.locfileid: "80343840"
---
# <a name="quickstart-deploy-open-source-fhir-server-using-powershell"></a>快速入门：使用 PowerShell 部署开源 FHIR 服务器

本快速入门介绍如何使用 PowerShell 部署开源的适用于 Azure 的 Microsoft FHIR 服务器。

如果没有 Azure 订阅，可在开始前创建一个[试用帐户](https://www.azure.cn/pricing/1rmb-trial)。

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]


## <a name="create-a-resource-group"></a>创建资源组

选择要包含预配资源的资源组的名称，并创建该资源组：

```azurepowershell
$fhirServiceName = "MyFhirService"
$rg = New-AzResourceGroup -Name $fhirServiceName -Location westus2
```

## <a name="deploy-the-fhir-server-template"></a>部署 FHIR 服务器模板

适用于 Azure 的 Microsoft FHIR 服务器 [GitHub 存储库](https://github.com/Microsoft/fhir-server)包含一个用于部署所有必要资源的模板。 使用以下命令部署该模板：

```azurepowershell
New-AzResourceGroupDeployment -TemplateUri https://raw.githubusercontent.com/Microsoft/fhir-server/master/samples/templates/default-azuredeploy.json -ResourceGroupName $rg.ResourceGroupName -serviceName $fhirServiceName
```

## <a name="verify-fhir-server-is-running"></a>验证 FHIR 服务器是否正在运行

```azurepowershell
$metadataUrl = "https://" + $fhirServiceName + ".azurewebsites.net/metadata" 
$metadata = Invoke-WebRequest -Uri $metadataUrl
$metadata.RawContent
```

服务器首次做出响应大约需要一分钟时间。

## <a name="clean-up-resources"></a>清理资源

如果你不打算继续使用此应用程序，请按以下步骤删除资源组：

```azurepowershell
Remove-AzResourceGroup -Name $rg.ResourceGroupName
```

## <a name="next-steps"></a>后续步骤

在本教程中，你已将适用于 Azure 的 Microsoft 开源 FHIR 服务器部署到订阅中。 若要了解如何使用 Postman 访问 FHIR API，请继续阅读 Postman 教程。
 
>[!div class="nextstepaction"]
>[使用 Postman 访问 FHIR API](access-fhir-postman-tutorial.md)
