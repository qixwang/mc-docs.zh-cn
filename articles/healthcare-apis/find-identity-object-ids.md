---
title: 查找用于身份验证的标识对象 ID - Azure API for FHIR
description: 本文介绍如何查找为 Azure API for FHIR 配置身份验证所需的标识对象 ID
services: healthcare-apis
author: hansenms
ms.service: healthcare-apis
ms.subservice: fhir
ms.topic: conceptual
ms.date: 02/07/2019
ms.author: mihansen
ms.openlocfilehash: d918ca20e3e7f800ed681e8808528503e74be7f1
ms.sourcegitcommit: c1ba5a62f30ac0a3acb337fb77431de6493e6096
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2020
ms.locfileid: "80343832"
---
# <a name="find-identity-object-ids-for-authentication-configuration"></a>查找用于身份验证配置的标识对象 ID

本文介绍如何查找为 Azure API for FHIR 配置允许的标识对象 ID 列表而需要的标识对象 ID。

完全托管式的 Azure API for FHIR&reg; 服务配置为仅允许一系列预定义的标识对象 ID 进行访问。 应用程序或用户在尝试访问 FHIR API 时必须提供持有者令牌。 此持有者令牌将包含特定的声明（字段）。 若要授予对 FHIR API 的访问权限，该令牌必须包含正确的颁发者 (`iss`)、受众 (`aud`)，以及允许的对象 ID 列表中的某个对象 ID (`oid`)。 标识对象 ID 是 Azure Active Directory 中的用户或服务主体的对象 ID。

## <a name="configure-list-of-allowed-object-ids"></a>配置允许的对象 ID 列表

创建新的 Azure API for FHIR 实例时，可以配置允许的对象 ID 列表：

![配置允许的对象 ID](media/quickstart-paas-portal/configure-allowed-oids.png)

这些对象 ID 可以是 Azure Active Directory 中特定用户或服务主体的 ID。

## <a name="find-user-object-id"></a>查找用户对象 ID

如果用户的用户名为 `myuser@consoso.com`，你可以使用以下 PowerShell 命令查找该用户的 `ObjectId`：

```azurepowershell
$(Get-AzureADUser -Filter "UserPrincipalName eq 'myuser@consoso.com'").ObjectId
```

或者，可以使用 Azure CLI：

```azurecli
az ad user show --upn-or-object-id myuser@consoso.com | jq -r .objectId
```

## <a name="find-service-principal-object-id"></a>查找服务主体对象 ID

假设你已注册一个[服务客户端应用程序](register-service-azure-ad-client-app.md)并希望允许此服务客户端访问 Azure API for FHIR，在此情况下，可以使用以下 PowerShell 命令查找客户端服务主体的对象 ID：

```azurepowershell
$(Get-AzureADServicePrincipal -Filter "AppId eq 'XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX'").ObjectId
```

其中，`XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX` 是服务客户端应用程序 ID。 或者，可以使用服务客户端的 `DisplayName`：

```azurepowershell
$(Get-AzureADServicePrincipal -Filter "DisplayName eq 'testapp'").ObjectId
```

如果使用的是 Azure CLI，可以使用：

```azurecli
az ad sp show --id XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX | jq -r .objectId
```

## <a name="next-steps"></a>后续步骤

在本文中，你已了解如何查找配置有权访问 Azure API for FHIR 实例的标识而需要的标识对象 ID。 接下来请部署完全托管式的 Azure API for FHIR：
 
>[!div class="nextstepaction"]
>[部署 Azure API for FHIR](fhir-paas-portal-quickstart.md)