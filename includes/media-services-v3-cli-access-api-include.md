---
title: include 文件
description: include 文件
services: media-services
author: WenJason
ms.service: media-services
ms.topic: include
origin.date: 05/01/2019
ms.date: 02/24/2020
ms.author: v-jay
ms.custom: include file
ms.openlocfilehash: 0cafeef330bfb3568909e0f9a795a4a1c14f3ce5
ms.sourcegitcommit: f5bc5bf51a4ba589c94c390716fc5761024ff353
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/20/2020
ms.locfileid: "77494468"
---
## <a name="access-the-media-services-api"></a>访问媒体服务 API

若要连接到 Azure 媒体服务 API，请使用 Azure AD 服务主体身份验证。 以下命令创建 Azure AD 应用程序并将服务主体附加到帐户。 应使用返回的值配置应用程序。

在运行脚本之前，应将 `amsaccount` 和 `amsResourceGroup` 替换为在创建这些资源时选择的名称。 `amsaccount` 是要向其附加服务主体的 Azure 媒体服务帐户的名称。

如果你有权访问多个订阅，请先将活动订阅设置为在其中创建了媒体服务帐户的订阅。

```azurecli
az account set --subscription subscriptionId
```

以下命令返回 `json` 输出：

```azurecli
az ams account sp create --account-name amsaccount --resource-group amsResourceGroup
```

此命令会生成如下响应：

```json
{
  "AadClientId": "00000000-0000-0000-0000-000000000000",
  "AadEndpoint": "https://login.chinacloudapi.cn",
  "AadSecret": "00000000-0000-0000-0000-000000000000",
  "AadTenantId": "00000000-0000-0000-0000-000000000000",
  "AccountName": "amsaccount",
  "ArmAadAudience": "https://management.core.chinacloudapi.cn/",
  "ArmEndpoint": "https://management.chinacloudapi.cn/",
  "Region": "chinaeast",
  "ResourceGroup": "amsResourceGroup",
  "SubscriptionId": "00000000-0000-0000-0000-000000000000"
}
```

如果想要在响应中获得 `xml`，请使用以下命令：

```azurecli
az ams account sp create --account-name amsaccount --resource-group amsResourceGroup --xml
```
