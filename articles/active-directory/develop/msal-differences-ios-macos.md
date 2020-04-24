---
title: 适用于 iOS 和 macOS 的 Microsoft 身份验证库 (MSAL) | Azure
description: 介绍 iOS 与 macOS 之间的 Microsoft 身份验证库 (MSAL) 使用差异。
services: active-directory
documentationcenter: dev-center-name
author: TylerMSFT
manager: CelesteDG
editor: ''
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
origin.date: 08/28/2019
ms.date: 11/01/2019
ms.author: v-junlch
ms.reviewer: ''
ms.custom: aaddev, identityplatformtop40
ms.collection: M365-identity-device-management
ms.openlocfilehash: fd735859eafefe3e599a3f72dc922594305bf50e
ms.sourcegitcommit: c1ba5a62f30ac0a3acb337fb77431de6493e6096
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2020
ms.locfileid: "73831034"
---
# <a name="microsoft-authentication-library-for-ios-and-macos-differences"></a>适用于 iOS 的 Microsoft 身份验证库和适用于 macOS 的 Microsoft 身份验证库的差异

本文介绍适用于 iOS 的 Microsoft 身份验证库 (MSAL) 和适用于 macOS 的 Microsoft 身份验证库 (MSAL) 之间的功能差异。

> [!NOTE]
> 在 Mac 上，MSAL 仅支持 macOS 应用。

## <a name="general-differences"></a>一般差异

适用于 macOS 的 MSAL 是可用于 iOS 的功能的子集。

适用于 macOS 的 MSAL 不支持：

- 不同的浏览器类型，例如 `ASWebAuthenticationSession`、`SFAuthenticationSession`、`SFSafariViewController`。
- macOS 不支持通过 Microsoft Authenticator 应用进行代理身份验证。

在 macOS 10.14 及更早版本中，同一发布者的应用之间的密钥链共享受到更多限制。 使用[访问控制列表](https://developer.apple.com/documentation/security/keychain_services/access_control_lists?language=objc)指定应共享密钥链的应用的路径。 用户可能会看到其他密钥链提示。

在 macOS 10.15+ 中，MSAL 的行为在 iOS 和 macOS 之间是相同的。 MSAL 使用[密钥链访问组](https://developer.apple.com/documentation/security/keychain_services/keychain_items/sharing_access_to_keychain_items_among_a_collection_of_apps?language=objc)进行密钥链共享。 

### <a name="project-setup-differences"></a>项目设置差异

**macOS**

- 在 macOS 上设置项目时，请确保应用程序使用有效的开发或生产证书进行签名。 MSAL 仍在未签名模式下工作，但它在缓存持久性方面的行为会有所不同。 应用应仅出于调试目的而未签名运行。 如果分发未签名的应用，它将：
1. 在 10.14 及更早版本中，MSAL 将在用户每次重启应用时提示用户输入密钥链密码。
2. 在 10.15+ 中，MSAL 将提示用户提供每次令牌获取的凭据。 

- macOS 应用无需实现 AppDelegate 调用。

**iOS**

- 还需要执行其他步骤来设置项目以支持身份验证代理流。 本教程介绍了这些步骤。
- iOS 项目需要在 info.plist 中注册自定义方案。 在 macOS 上这不是必需的。

