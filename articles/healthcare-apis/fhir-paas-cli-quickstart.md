---
title: 快速入门：使用 Azure CLI 部署 Azure API for FHIR
description: 本快速入门介绍如何使用 Azure CLI 在 Azure 中部署 Azure API for FHIR。
services: healthcare-apis
author: hansenms
ms.service: healthcare-apis
ms.subservice: fhir
ms.topic: quickstart
ms.date: 10/15/2019
ms.author: mihansen
ms.openlocfilehash: 7a9065bbcfed3d485f378a7a7b517736a4413709
ms.sourcegitcommit: 6ddc26f9b27acec207b887531bea942b413046ad
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/27/2020
ms.locfileid: "80343839"
---
# <a name="quickstart-deploy-azure-api-for-fhir-using-azure-cli"></a>快速入门：使用 Azure CLI 部署 Azure API for FHIR

本快速入门介绍如何使用 Azure CLI 在 Azure 中部署 Azure API for FHIR。

如果没有 Azure 订阅，可在开始前创建一个[试用帐户](https://www.azure.cn/pricing/1rmb-trial)。


## <a name="add-healthcareapis-extension"></a>添加 HealthcareAPIs 扩展

```azurecli
az extension add --name healthcareapis
```

获取 HealthcareAPIs 的命令列表：

```azurecli
az healthcareapis --help
```

## <a name="locate-your-identity-object-id"></a>找到标识对象 ID

对象 ID 值是与订阅关联的目录中特定 Azure Active Directory 用户或服务主体的对象 ID 相对应的 GUID。 如果你想要知道特定用户的对象 ID，可使用如下命令找到它：

```azurecli
az ad user show --id myuser@consoso.com | jq -r .objectId
```
如需更多详细信息，请阅读有关[查找标识对象 ID](find-identity-object-ids.md) 的操作指南。

## <a name="create-azure-resource-group"></a>创建 Azure 资源组

选择要包含 Azure API for FHIR 的资源组的名称，并创建该资源组：

```azurecli
az group create --name "myResourceGroup" --location westus2
```

## <a name="deploy-the-azure-api-for-fhir"></a>部署 Azure API for FHIR

```azurecli
az healthcareapis create --resource-group myResourceGroup --name nameoffhiraccount --kind fhir-r4 --location westus2 --access-policies-object-id "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
```

其中，`xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx` 是你希望其有权访问 FHIR API 的用户或服务主体的标识对象 ID。

## <a name="fetch-fhir-api-capability-statement"></a>提取 FHIR API 功能语句

使用以下命令从 FHIR API 中获取功能语句：

```azurecli
curl --url "https://nameoffhiraccount.azurehealthcareapis.com/metadata"
```

## <a name="clean-up-resources"></a>清理资源

如果你不打算继续使用此应用程序，请按以下步骤删除资源组：

```azurecli
az group delete --name "myResourceGroup"
```

## <a name="next-steps"></a>后续步骤

在本快速入门指南中，你已将 Azure API for FHIR 部署到订阅中。 若要在 Azure API for FHIR 中指定其他设置，请转到有关其他设置的操作指南。

>[!div class="nextstepaction"]
>[Azure API for FHIR 中的其他设置](azure-api-for-fhir-additional-settings.md)
