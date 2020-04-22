---
title: Azure 中的 Postman FHIR 服务器 - Azure API for FHIR
description: 本教程逐步说明使用 Postman 访问 FHIR 服务器所要执行的步骤。 Postman 有助于调试访问 API 的应用程序。
services: healthcare-apis
ms.service: healthcare-apis
ms.subservice: fhir
ms.topic: tutorial
ms.reviewer: dseven
ms.author: mihansen
author: hansenms
ms.date: 02/07/2019
ms.openlocfilehash: 444833595adbb194186dd1cb73b5c5afe54333de
ms.sourcegitcommit: c1ba5a62f30ac0a3acb337fb77431de6493e6096
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2020
ms.locfileid: "80343877"
---
# <a name="access-azure-api-for-fhir-with-postman"></a>使用 Postman 访问 Azure API for FHIR

某个客户端应用程序通过 [REST API](https://www.hl7.org/fhir/http.html)访问 FHIR API。 在生成应用程序时，出于调试等目的，你可能还希望直接与 FHIR 服务器交互。 本教程将逐步说明使用 [Postman](https://www.getpostman.com/) 访问 FHIR 服务器所要执行的步骤。 生成访问 API 的应用程序时，Postman 是经常用于调试的工具。

## <a name="prerequisites"></a>先决条件

- Azure 中的 FHIR 终结点。 可以使用托管的 Azure API for FHIR 或开源的适用于 Azure 的 FHIR 服务器设置该终结点。 使用 [Azure 门户](fhir-paas-portal-quickstart.md)、[PowerShell](fhir-paas-powershell-quickstart.md) 或 [Azure CLI](fhir-paas-cli-quickstart.md) 设置托管的 Azure API for FHIR。
- 已安装 Postman。 可以从 [https://www.getpostman.com](https://www.getpostman.com) 获取 Postman。

## <a name="fhir-server-and-authentication-details"></a>FHIR 服务器和身份验证详细信息

若要使用 Postman，需提供以下详细信息：

- FHIR 服务器 URL，例如 `https://MYACCOUNT.azurehealthcareapis.com`
- FHIR 服务器的标识提供者 `Authority`，例如 `https://login.microsoftonline.com/.{TENANT-ID}`
- 配置的 `audience`。 这通常是 FHIR 服务器的 URL，例如 `https://MYACCOUNT.azurehealthcareapis.com` 或简单的 `https://azurehealthcareapis.com`。
- 用于访问 FHIR 服务的[客户端应用程序](register-confidential-azure-ad-client-app.md)的 `client_id`（或应用程序 ID）。
- 客户端应用程序的 `client_secret`（或应用程序机密）。

最后，应检查 `https://www.getpostman.com/oauth2/callback` 是否为客户端应用程序的已注册回复 URL。

## <a name="connect-to-fhir-server"></a>连接到 FHIR 服务器

使用 Postman 向 `https://fhir-server-url/metadata` 发出 `GET` 请求：

![Postman 元数据功能语句](media/tutorial-postman/postman-metadata.png)

Azure API for FHIR 的元数据 URL 是 `https://MYACCOUNT.azurehealthcareapis.com/metadata`。 在此示例中，FHIR 服务器 URL 是 `https://fhirdocsmsft.azurewebsites.net`，`https://fhirdocsmsft.azurewebsites.net/metadata` 上已提供服务器的功能声明。 无需身份验证应该即可访问该终结点。

如果尝试访问受限制的资源，应会收到“身份验证失败”响应：

![身份验证失败](media/tutorial-postman/postman-authentication-failed.png)

## <a name="obtaining-an-access-token"></a>获取访问令牌

若要获取有效的访问令牌，请选择“授权”，然后选择“OAuth 2.0”类型：

![设置 OAuth 2.0](media/tutorial-postman/postman-select-oauth2.png)

点击“获取新访问令牌”，此时会显示一个对话框：

![请求新访问令牌](media/tutorial-postman/postman-request-token.png)

需要提供一些详细信息：

| 字段                 | 示例值                                                                                                   | 注释                    |
|-----------------------|-----------------------------------------------------------------------------------------------------------------|----------------------------|
| 令牌名称            | MYTOKEN                                                                                                         | 所选的名称          |
| 授权类型            | 授权代码                                                                                              |                            |
| 回调 URL          | `https://www.getpostman.com/oauth2/callback`                                                                      |                            |
| 身份验证 URL              | `https://login.microsoftonline.com/{TENANT-ID}/oauth2/authorize?resource=<audience>` | Azure API for FHIR 的 `audience` 是 `https://MYACCOUNT.azurehealthcareapis.com` |
| 访问令牌 URL      | `https://login.microsoftonline.com/{TENANT ID}/oauth2/token`                                                      |                            |
| 客户端 ID             | `XXXXXXXX-XXX-XXXX-XXXX-XXXXXXXXXXXX`                                                                            | 应用程序 ID             |
| 客户端机密         | `XXXXXXXX`                                                                                                        | 机密客户端密钥          |
| 作用域 | `<Leave Blank>` |
| 状态                 | `1234`                                                                                                            |                            |
| 客户端身份验证 | 在正文中发送客户端凭据                                                                                 |                 

点击“请求令牌”，系统会引导你完成 Azure Active Directory 身份验证流，并将令牌返回给 Postman。 如果遇到问题，请打开 Postman 控制台（通过“视图”->“显示 Postman 控制台”菜单项）。

在返回的令牌屏幕中向下滚动，并点击“使用令牌”：

![使用令牌](media/tutorial-postman/postman-use-token.png)

现在，该令牌应会填充到“访问令牌”字段中，你可以从“可用令牌”中选择令牌。 如果再次选择“发送”来重复 `Patient` 资源搜索，应会获得状态 `200 OK`：

![200 正常](media/tutorial-postman/postman-200-OK.png)

这种情况表示数据库中没有患者，因此搜索结果是空的。

如果使用 [https://jwt.ms](https://jwt.ms) 之类的工具检查访问令牌，应会看到如下所示的内容：

```jsonc
{
  "aud": "https://MYACCOUNT.azurehealthcareapis.com",
  "iss": "https://sts.windows.net/{TENANT-ID}/",
  "iat": 1545343803,
  "nbf": 1545343803,
  "exp": 1545347703,
  "acr": "1",
  "aio": "AUQAu/8JXXXXXXXXXdQxcxn1eis459j70Kf9DwcUjlKY3I2G/9aOnSbw==",
  "amr": [
    "pwd"
  ],
  "appid": "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
  "oid": "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
  "appidacr": "1",

  ...// Truncated
}
```

在故障排除场合下，最好是先验证是否设置了正确的受众（`aud` 声明）。 托管的 Azure API for FHIR 使用[标识对象 ID](find-identity-object-ids.md) 来限制对服务的访问。 确保令牌的 `oid` 声明包含允许的对象 ID 列表中的对象 ID。

还可以[使用 Azure CLI 获取 Azure API for FHIR 的令牌](get-healthcare-apis-access-token-cli.md)。

## <a name="inserting-a-patient"></a>插入患者

现已获取有效的访问令牌。 接下来可以插入新患者。 切换到方法“POST”，在请求正文中添加以下 JSON 文档：

[!code-json[](samples/sample-patient.json)]

点击“发送”，随后应会看到成功创建了患者：

![已创建患者](media/tutorial-postman/postman-patient-created.png)

如果重复患者搜索，现在应会看到患者记录：

![已创建患者](media/tutorial-postman/postman-patient-found.png)

## <a name="next-steps"></a>后续步骤

在本教程中，你已使用 Postman 访问了 FHIR API。 请在“支持的功能”部分了解支持的 API 功能。
 
>[!div class="nextstepaction"]
>[受支持的功能](fhir-features-supported.md)