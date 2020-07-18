---
title: 通过 HTTPS 进行身份验证 - Azure 数据资源管理器 | Microsoft Docs
description: 本文介绍如何在 Azure 数据资源管理器中通过 HTTPS 进行身份验证。
services: data-explorer
author: orspod
ms.author: v-tawe
ms.reviewer: rkarlin
ms.service: data-explorer
ms.topic: reference
origin.date: 10/30/2019
ms.date: 07/01/2020
ms.openlocfilehash: a7affeaa9747dc37d0748a8c0ee720eb26fb91fb
ms.sourcegitcommit: 9bc3e55f01e0999f05e7b4ebaea95f3ac91d32eb
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/10/2020
ms.locfileid: "86226336"
---
# <a name="authentication-over-https"></a>通过 HTTPS 进行身份验证

使用 HTTPS 时，服务支持用于执行身份验证的标准 HTTP `Authorization` 标头。

支持的 HTTP 身份验证方法包括：

* **Azure Active Directory**（通过 `bearer` 方法）。

使用 Azure AD 进行身份验证时，`Authorization` 标头的格式为：

```txt
Authorization: bearer TOKEN
```

其中的 `TOKEN` 是调用方通过与 Azure AD 服务通信获取的访问令牌。 该令牌有以下属性：

* 资源是服务 URI（例如 `https://help.kusto.chinacloudapi.cn`）
* Azure AD 服务终结点为 `https://login.partner.microsoftonline.cn/TENANT/`

其中的 `TENANT` 是 Azure AD 租户 ID 或名称。 例如，在 Microsoft 租户下创建的服务可以使用 `https://login.partner.microsoftonline.cn/microsoft.com/`。 也可以向 `https://login.partner.microsoftonline.cn/common/` 发出请求（仅适用于用户身份验证）。

> [!NOTE]
> Azure AD 服务终结点在国家云中运行时会发生更改。
> 若要更改终结点，请将环境变量 `AadAuthorityUri` 设为所需的 URI。

有关详细信息，请参阅[身份验证概述](../../management/access-control/index.md)和 [Azure AD 身份验证指南](../../management/access-control/how-to-authenticate-with-aad.md)。
