---
title: 受保护的 Web API - 概述
titleSuffix: Microsoft identity platform
description: 了解如何构建受保护的 Web API（概述）。
services: active-directory
documentationcenter: dev-center-name
author: jmprieur
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 02/06/2020
ms.author: v-junlch
ms.custom: aaddev, identityplatformtop40
ms.openlocfilehash: b2bcae00db11b72769bc9ecc4975871551b9e1dd
ms.sourcegitcommit: c1ba5a62f30ac0a3acb337fb77431de6493e6096
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2020
ms.locfileid: "77067645"
---
# <a name="scenario-protected-web-api"></a>方案：受保护的 Web API

通过此方案了解如何公开 Web API。 还了解如何保护 Web API，以便只有经过身份验证的用户才能访问它。

若要使用 Web API，需要启用使用工作和学校帐户进行了身份验证的用户。

## <a name="prerequisites"></a>先决条件

[!INCLUDE [Pre-requisites](../../../includes/active-directory-develop-scenarios-prerequisites.md)]

## <a name="specifics"></a>详情

下面是保护 Web API 时需要了解的具体信息：

- 你的应用注册必须至少公开一个范围。 Web API 接受的令牌版本取决于登录受众。
- Web API 的代码配置必须验证调用 Web API 时使用的令牌。

## <a name="next-steps"></a>后续步骤

> [!div class="nextstepaction"]
> [应用注册](scenario-protected-web-api-app-registration.md)

<!-- Update_Description: wording update -->