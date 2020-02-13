---
title: 将调用 Web API 的 Web API 移到生产环境 - Microsoft 标识平台 | Azure
description: 了解如何将调用 Web API 的 Web API 移到生产环境。
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
ms.openlocfilehash: 20f62c93feb845d2dd90272e8dfc66008a1b0a8b
ms.sourcegitcommit: 7c80405a6b48380814b4b414e9f8a5756c007880
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/07/2020
ms.locfileid: "77067701"
---
# <a name="a-web-api-that-calls-web-apis-move-to-production"></a>调用 Web API 的 Web API：移到生产环境

获得调用 Web API 的令牌后，就可以将应用移到生产环境了。

[!INCLUDE [Move to production common steps](../../../includes/active-directory-develop-scenarios-production.md)]

## <a name="learn-more"></a>了解详细信息

既然你已了解了如何从自己的 Web API 调用 Web API 的基础知识，你可能会对下一个教程感兴趣，它介绍了用于构建调用 Web API 的受保护 Web API 的代码。

| 示例 | 平台 | 说明 |
|--------|----------|-------------|
| [active-directory-aspnetcore-webapi-tutorial-v2](https://github.com/Azure-Samples/active-directory-dotnet-native-aspnetcore-v2/tree/master/2.%20Web%20API%20now%20calls%20Microsoft%20Graph) | ASP.NET Core 2.2 Web API, 桌面 (WPF) | ASP.NET Core 2.2 Web API 调用 Microsoft Graph，你可以使用 Microsoft 标识平台 (v2.0) 从 WPF 应用程序调用它。 |

<!-- Update_Description: wording update -->