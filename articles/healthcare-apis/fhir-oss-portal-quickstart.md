---
title: Azure 门户：部署开源的适用于 Azure 的 FHIR 服务器 - Azure API for FHIR
description: 本快速入门介绍如何使用 Azure 门户部署 Microsoft 开源 FHIR 服务器。
services: healthcare-apis
author: hansenms
ms.service: healthcare-apis
ms.subservice: fhir
ms.topic: quickstart
ms.date: 02/07/2019
ms.author: mihansen
ms.openlocfilehash: 64e443d860ddf5999dc3c91f62e2f55c9e0066f3
ms.sourcegitcommit: c1ba5a62f30ac0a3acb337fb77431de6493e6096
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2020
ms.locfileid: "80343841"
---
# <a name="quickstart-deploy-open-source-fhir-server-using-azure-portal"></a>快速入门：使用 Azure 门户部署开源 FHIR 服务器

本快速入门介绍如何使用 Azure 门户在 Azure 中部署开源 FHIR 服务器。 我们将使用[开源存储库](https://github.com/Microsoft/fhir-server)中的简易部署链接

如果没有 Azure 订阅，可在开始前创建一个[试用帐户](https://www.azure.cn/pricing/1rmb-trial)。

## <a name="github-open-source-repository"></a>GitHub 开源存储库

导航到 [GitHub 部署页](https://github.com/Microsoft/fhir-server/blob/master/docs/DefaultDeployment.md)并找到“部署到 Azure”按钮：

![开源部署页](media/quickstart-oss-portal/deployment-page-oss.png)

单击“部署”按钮，此时会打开 Azure 门户。

## <a name="fill-in-deployment-parameters"></a>填写部署参数

选择创建新的资源组并为其命名。 其他唯一一个必需参数是服务的名称。

![自定义部署参数](media/quickstart-oss-portal/deployment-custom-parameters.png)

请注意，部署将直接从 GitHub 上的开源存储库中提取源代码。 如果你已创建存储库的分支，可以指向自己的和特定的分支。

填写详细信息后，可以开始部署。

## <a name="validate-fhir-server-is-running"></a>验证 FHIR 服务器是否正在运行

部署完成后，可将浏览器指向 `https://SERVICENAME.azurewebsites.net/metadata` 以获取功能语句。 服务器首次做出响应大约需要一分钟时间。

## <a name="clean-up-resources"></a>清理资源

不再需要上述资源组和所有相关资源时，可将其删除。 为此，请选择包含预配资源的资源组，选择“删除资源组”，然后确认要删除的资源组的名称。 

## <a name="next-steps"></a>后续步骤

在本教程中，你已将适用于 Azure 的 Microsoft 开源 FHIR 服务器部署到订阅中。 若要了解如何使用 Postman 访问 FHIR API，请继续阅读 Postman 教程。
 
>[!div class="nextstepaction"]
>[使用 Postman 访问 FHIR API](access-fhir-postman-tutorial.md)