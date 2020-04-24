---
title: 将调用 Web API 的 Web 应用移到生产环境 - Microsoft 标识平台 | Azure
description: 了解如何将调用 Web API 的 Web 应用移到生产环境。
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
ms.custom: aaddev
ms.openlocfilehash: 851fd6e645f6d37c55fc600a37ca44a79fbc27cd
ms.sourcegitcommit: c1ba5a62f30ac0a3acb337fb77431de6493e6096
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2020
ms.locfileid: "77067697"
---
# <a name="a-web-app-that-calls-web-apis-move-to-production"></a>调用 Web API 的 Web 应用：移到生产环境

现在你已了解如何获取用于调用 Web API 的令牌，接下来了解如何移到生产环境。

[!INCLUDE [Common steps to move to production](../../../includes/active-directory-develop-scenarios-production.md)]

## <a name="next-steps"></a>后续步骤

通过试用 ASP.NET Core Web 应用的完整渐进式教程了解更多信息。 本教程：

- 演示如何将用户登录到多个受众或国家云，或使用社交标识登录。
- 调用 Microsoft Graph。
- 调用多个 Microsoft API。
- 处理增量同意。
- 调用自己的 Web API。

> [!div class="nextstepaction"]
> [ASP.NET Core Web 应用教程](https://github.com/Azure-Samples/ms-identity-aspnetcore-webapp-tutorial#scope-of-this-tutorial)

<!--- Removing this diagram as it's already shown from the next step linked tutorial

![Tutorial overview](./media/scenarios/aspnetcore-webapp-tutorial.svg)

--->

<!-- Update_Description: wording update -->