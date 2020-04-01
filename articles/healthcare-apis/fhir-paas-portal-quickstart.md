---
title: 快速入门：使用 Azure 门户部署 Azure API for FHIR
description: 本快速入门介绍如何使用 Azure 门户部署 Azure API for FHIR 和配置设置。
services: healthcare-apis
author: hansenms
ms.service: healthcare-apis
ms.subservice: fhir
ms.topic: quickstart
ms.date: 02/07/2019
ms.author: mihansen
ms.openlocfilehash: 022c131058609e8d65d7b1699caf7bf0c44ac363
ms.sourcegitcommit: 6ddc26f9b27acec207b887531bea942b413046ad
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/27/2020
ms.locfileid: "80343836"
---
# <a name="quickstart-deploy-azure-api-for-fhir-using-azure-portal"></a>快速入门：使用 Azure 门户部署 Azure API for FHIR

本快速入门介绍如何使用 Azure 门户部署 Azure API for FHIR。

如果没有 Azure 订阅，可在开始前创建一个[试用帐户](https://www.azure.cn/pricing/1rmb-trial)。

## <a name="create-new-resource"></a>新建资源

打开 [Azure 门户](https://portal.azure.cn)，单击“创建资源” 

![创建资源](media/quickstart-paas-portal/portal-create-resource.png)

## <a name="search-for-azure-api-for-fhir"></a>搜索 Azure API for FHIR

可以通过在搜索框中键入“FHIR”来查找 Azure API for FHIR：

![搜索医疗保健 API](media/quickstart-paas-portal/portal-search-healthcare-apis.png)

## <a name="create-azure-api-for-fhir-account"></a>创建 Azure API for FHIR 帐户

选择“创建”以创建新的 Azure API for FHIR 帐户： 

![创建 Azure API for FHIR 帐户](media/quickstart-paas-portal/portal-create-healthcare-apis.png)

## <a name="enter-account-details"></a>输入帐户详细信息

选择现有资源组或创建新的资源组，选择帐户名称，最后单击“查看 + 创建”： 

![新医疗保健 API 的详细信息](media/quickstart-paas-portal/portal-new-healthcareapi-details.png)

确认创建，并等待 FHIR API 部署完成。

## <a name="additional-settings"></a>其他设置

单击“下一步:  其他设置”来配置颁发机构、受众以及应有权访问此 Azure API for FHIR 的标识对象 ID，根据需要启用 SMART on FHIR，并配置数据库吞吐量：

- **颁发机构：** 可以指定不同的 Azure AD 租户，而不是你登录到的、作为服务身份验证颁发机构的租户。
- **受众:** 最佳做法是将受众设置为 FHIR 服务器的 URL，这也是默认设置。 可在此处更改此设置。 受众标识令牌的目标接收者。 在此上下文中，它应设置为代表 FHIR API 本身的值。
- **允许的对象 ID：** 可以指定应有权访问此 Azure API for FHIR 的标识对象 ID。 可以在[查找标识对象 ID](find-identity-object-ids.md) 操作指南中详细了解如何查找用户和服务主体的对象 ID。  
- **SMART On FHIR 代理：** 可以启用 SMART on FHIR 代理。 有关如何配置 SMART on FHIR 代理的详细信息，请参阅教程 [Azure API for FHIR SMART on FHIR 代理](https://docs.azure.cn/healthcare-apis/use-smart-on-fhir-proxy)  
- **预配的吞吐量(RU/秒)：** 在此处可为 Azure API for FHIR 的底层数据库指定吞吐量设置。 以后可以在“数据库”边栏选项卡中更改此设置。 有关更多详细信息，请参阅[配置数据库设置](configure-database.md)页。


![配置允许的对象 ID](media/quickstart-paas-portal/configure-audience.png)

## <a name="fetch-fhir-api-capability-statement"></a>提取 FHIR API 功能语句

若要验证是否预配了新的 FHIR API 帐户，请通过将浏览器指向 `https://<ACCOUNT-NAME>.azurehealthcareapis.com/metadata` 来提取功能语句。

## <a name="clean-up-resources"></a>清理资源

不再需要上述资源组、Azure API for FHIR 和所有相关资源时，可将其删除。 为此，请选择包含 Azure API for FHIR 帐户的资源组，选择“删除资源组”，然后确认要删除的资源组的名称。 

## <a name="next-steps"></a>后续步骤

在本快速入门指南中，你已将 Azure API for FHIR 部署到订阅中。 若要在 Azure API for FHIR 中指定其他设置，请转到有关其他设置的操作指南。

>[!div class="nextstepaction"]
>[Azure API for FHIR 中的其他设置](azure-api-for-fhir-additional-settings.md)
