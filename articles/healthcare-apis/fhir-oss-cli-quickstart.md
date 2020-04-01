---
title: Azure CLI：部署开源的适用于 Azure 的 FHIR 服务器 - Azure API for Azure
description: 本快速入门介绍如何部署开源的适用于 Azure 的 Microsoft FHIR 服务器。
services: healthcare-apis
author: hansenms
ms.service: healthcare-apis
ms.subservice: fhir
ms.topic: quickstart
ms.date: 02/07/2019
ms.author: mihansen
ms.openlocfilehash: 6fa14f818a5b83e403fd50e045a3575862630d07
ms.sourcegitcommit: 6ddc26f9b27acec207b887531bea942b413046ad
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/27/2020
ms.locfileid: "80343849"
---
# <a name="quickstart-deploy-open-source-fhir-server-using-azure-cli"></a>快速入门：使用 Azure CLI 部署开源 FHIR 服务器

本快速入门介绍如何使用 Azure CLI 在 Azure 中部署开源 FHIR&reg; 服务器。

如果没有 Azure 订阅，可在开始前创建一个[试用帐户](https://www.azure.cn/pricing/1rmb-trial)。


## <a name="create-resource-group"></a>创建资源组

选择要包含预配资源的资源组的名称，并创建该资源组：

```azurecli
servicename="myfhirservice"
az group create --name $servicename --location westus2
```

## <a name="deploy-template"></a>部署模板

适用于 Azure 的 Microsoft FHIR 服务器 [GitHub 存储库](https://github.com/Microsoft/fhir-server)包含一个用于部署所有必要资源的模板。 使用以下命令部署该模板：

```azurecli
az group deployment create -g $servicename --template-uri https://raw.githubusercontent.com/Microsoft/fhir-server/master/samples/templates/default-azuredeploy.json --parameters serviceName=$servicename
```

## <a name="verify-fhir-server-is-running"></a>验证 FHIR 服务器是否正在运行

使用以下命令从 FHIR 服务器中获取功能语句：

```azurecli
metadataurl="https://${servicename}.azurewebsites.net/metadata"
curl --url $metadataurl
```

服务器首次做出响应大约需要一分钟时间。

## <a name="clean-up-resources"></a>清理资源

如果你不打算继续使用此应用程序，请按以下步骤删除资源组：

```azurecli
az group delete --name $servicename
```

## <a name="next-steps"></a>后续步骤

在本教程中，你已将适用于 Azure 的 Microsoft 开源 FHIR 服务器部署到订阅中。 若要了解如何使用 Postman 访问 FHIR API，请继续阅读 Postman 教程。
 
>[!div class="nextstepaction"]
>[使用 Postman 访问 FHIR API](access-fhir-postman-tutorial.md)