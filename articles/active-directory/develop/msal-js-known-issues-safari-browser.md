---
title: 浏览器上的已知问题（适用于 JavaScript 的 Microsoft 身份验证库）| Azure
description: 了解将适用于 JavaScript 的 Microsoft 身份验证库 (MSAL.js) 与 Safari 浏览器配合使用时的已知问题。
services: active-directory
documentationcenter: dev-center-name
author: navyasric
manager: CelesteDG
editor: ''
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: troubleshooting
ms.tgt_pltfrm: na
ms.workload: identity
origin.date: 05/16/2019
ms.date: 06/17/2019
ms.author: v-junlch
ms.reviewer: saeeda
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: 095bf8d46b957939092264ec8ed2f3d64144e1ff
ms.sourcegitcommit: c1ba5a62f30ac0a3acb337fb77431de6493e6096
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2020
ms.locfileid: "67305873"
---
# <a name="known-issues-on-safari-browser-with-msaljs"></a>将 Safari 浏览器与 MSAL.js 配合使用时的已知问题 

## <a name="silent-token-renewal-on-safari-12-and-itp-20"></a>在 Safari 12 和 ITP 2.0 上进行的无提示令牌续订

Apple iOS 12 和 MacOS 10.14 操作系统包含一个 [Safari 12 浏览器](https://developer.apple.com/safari/whats-new/)版本。 出于安全和隐私考虑，Safari 12 包含[智能跟踪防护 2.0](https://webkit.org/blog/8311/intelligent-tracking-prevention-2-0/)。 这实际上会导致浏览器删除设置的第三方 Cookie。 ITP 2.0 将标识提供者设置的 Cookie 视为第三方 Cookie。

### <a name="impact-on-msaljs"></a>对 MSAL.js 的影响

MSAL.js 使用隐藏的 Iframe 在 `acquireTokenSilent` 调用过程中执行无提示令牌获取和续订操作。 无提示令牌请求依赖于 Iframe 能否访问经身份验证的用户会话，该会话由 Azure AD 所设置的 Cookie 来表现。 由于 ITP 2.0 阻止对这些 Cookie 的访问，MSAL.js 无法以无提示方式获取并续订令牌，这会导致 `acquireTokenSilent` 故障。

目前没有此问题的解决方案，我们正在评估标准社区的选项。

### <a name="work-around"></a>变通方法

默认情况下，会在 Safari 浏览器上启用 ITP 设置。 可以禁用此设置，方法是：导航到“首选项”   -> “隐私”  ，然后取消选中“防止跨站点跟踪”选项。 

![safari 设置](./media/msal-js-known-issue-safari-browser/safari.png)

需使用交互式获取令牌调用来提示用户登录，以便处理 `acquireTokenSilent` 故障。
若要避免重复登录，可以实施的一个方法是处理 `acquireTokenSilent` 故障并为用户提供一个在 Safari 中禁用 ITP 设置的选项，然后再继续交互式调用。 禁用此设置以后，后续的无提示令牌续订应该会成功。

