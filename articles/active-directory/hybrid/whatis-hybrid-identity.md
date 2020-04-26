---
title: 将 Active Directory 与 Azure Active Directory 连接。 | Microsoft 文档
description: Azure AD Connect 会将本地目录与 Azure Active Directory 集成。 这样便可以为集成到 Azure AD 的 Office 365、Azure 和 SaaS 应用程序提供一个通用标识。
keywords: Azure AD Connect 介绍, Azure AD Connect 概述, 什么是 Azure AD Connect, 安装 active directory
services: active-directory
author: billmath
manager: daveba
ms.assetid: 59bd209e-30d7-4a89-ae7a-e415969825ea
ms.service: active-directory
ms.workload: identity
ms.topic: overview
origin.date: 11/28/2018
ms.date: 04/23/2019
ms.subservice: hybrid
ms.author: v-junlch
ms.collection: M365-identity-device-management
ms.openlocfilehash: d087a4172c00088f067785752f54c28d30a56d2b
ms.sourcegitcommit: a4a2521da9b29714aa6b511fc6ba48279b5777c8
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/24/2020
ms.locfileid: "82126543"
---
# <a name="hybrid-identity-and-microsoft-identity-solutions"></a>混合标识和 Microsoft 标识解决方案
使用 [Azure Active Directory (Azure AD)](../../active-directory/fundamentals/active-directory-whatis.md) 混合标识解决方案可将本地目录与 Azure AD 同步，同时仍可在本地管理用户。 如果计划将本地 Windows Server Active Directory 与 Azure AD 进行同步，首先需要决定是使用托管标识还是使用联合标识。 

- **托管标识** - 从本地 Active Directory 同步的用户帐户和组以及用户身份验证由 Azure 进行管理。   
- **联合标识**可以对用户进行更多的控制，其方法是将用户身份验证与 Azure 隔离开，将身份验证委托给受信任的本地标识提供者。 

有多个选项可用于配置混合标识。 当考虑哪个标识模型最适合组织需求时，还需考虑时间、现有基础结构、复杂性和成本。 这些因素对每个组织都不同，并可能随时间变化。 但是，如果需求确实发生更改，则还可灵活切换到不同的标识模型。

## <a name="managed-identity"></a>托管标识 

托管标识是将本地目录对象（用户和组）与 Azure AD 同步的最简单方法。 

![同步混合标识](./media/whatis-hybrid-identity/managed.png)

尽管托管标识是最简单且最快速的方法，你的用户将仍需针对基于云的资源维护一个单独的密码。 若要避免此问题，还可以（可选）将[用户密码哈希同步](how-to-connect-password-hash-synchronization.md)到 Azure AD 目录中。 同步密码哈希使用户能够使用与本地使用的相同用户名和密码登录基于云的组织资源。 Azure AD Connect 会定期检查本地目录是否发生更改，并保持 Azure AD 目录同步。 如果本地 Active Directory 的用户属性或密码已更改，则会在 Azure AD 中自动更新此信息。 

由于大多数组织只想让用户登录 Office 365 和其他基于 Azure AD 的资源，因此建议使用默认的密码哈希同步选项。 

> [!TIP]
> 用户密码以表示实际用户密码的哈希值形式存储在本地 Windows Server Active Directory 中。 哈希值是单向数学函数（哈希算法）的计算结果。 没有任何方法可将单向函数的结果还原为纯文本版本的密码。 无法使用密码哈希来登录本地网络。 如果选择同步密码，Azure AD Connect 会从本地 Active Directory 提取密码哈希，并在同步到 Azure AD 之前将额外安全处理应用于密码哈希。 
>

## <a name="federated-identity-ad-fs"></a>联合标识 (AD FS)

使用 AD FS 联合验证用户的登录时，可将身份验证委托给验证用户凭据的本地服务器。 在此模型中，本地 Active Directory 凭据永远不会传递到 Azure AD 中。

![联合标识](./media/whatis-hybrid-identity/federated-identity.png)

也称为“联合身份验证”，这种登录方法可确保所有用户身份验证均在本地得以控制，并且允许管理员实施更严格的访问控制。 使用 AD FS 的联合身份验证是最复杂的选项，需要在本地环境中部署其他服务器。 联合身份验证还承诺为 Active Directory 和 AD FS 基础结构提供全天候支持。 如果本地 Internet 访问、域控制器或 AD FS 服务器不可用，用户无法登录云服务，此时就需要这种高级支持。

> [!TIP]
> 如果决定使用 Active Directory 联合身份验证服务 (AD FS) 进行联合身份验证，则可以选择性地设置密码哈希同步，作为在 AD FS 基础结构发生故障时的备用身份验证方式。
>

## <a name="what-is-azure-ad-connect"></a>什么是 Azure AD Connect？

Azure AD Connect 专用于满足和完成混合标识目标的 Microsoft 工具。  这样便可以为集成到 Azure AD 的 Office 365、Azure 和 SaaS 应用程序的用户提供一个通用标识。  它提供以下功能：
    
- [同步](how-to-connect-sync-whatis.md) - 此组件负责创建用户、组和其他对象。 它还负责确保本地用户和组的标识信息与云匹配。  它负责将密码哈希与 Azure AD 进行同步。
- [密码哈希同步](how-to-connect-password-hash-synchronization.md) - 一个可选组件，可以将用户密码哈希与 Azure AD 同步，这样用户就可以在本地和云中使用相同的密码。
- [AD FS 和联合身份验证集成](how-to-connect-fed-whatis.md) - 联合身份验证是 Azure AD Connect 的可选部件，可用于使用本地 AD FS 基础结构配置混合环境。 它还提供了 AD FS 管理功能，例如证书续订和其他 AD FS 服务器部署。
- [PingFederate 和联合身份验证集成](how-to-connect-install-custom.md#configuring-federation-with-pingfederate) - 另一联合身份验证选项，允许你使用 PingFederate 作为标识提供者。

![什么是 Azure AD Connect](./media/whatis-hybrid-identity/arch.png)
 
## <a name="why-use-azure-ad-connect"></a>为何使用 Azure AD Connect？
将本地目录与 Azure AD 集成可提供用于访问云和本地资源的通用标识，来提高用户的工作效率。 用户和组织可以享受到以下好处：

- 用户可以使用单个标识来访问本地应用程序和云服务，例如 Office 365。
- 单个工具即可提供轻松同步和登录的部署体验。

## <a name="next-steps"></a>后续步骤

- [硬件和先决条件](how-to-connect-install-prerequisites.md) 
- [快速设置](how-to-connect-install-express.md)
- [自定义设置](how-to-connect-install-custom.md)
- [密码哈希同步](how-to-connect-password-hash-synchronization.md)|
- [Azure AD Connect 和联合身份验证](how-to-connect-fed-whatis.md)
- [Azure AD Connect 同步](how-to-connect-sync-whatis.md)
- [Azure AD Connect 常见问题解答](reference-connect-faq.md)

<!-- Update_Description: update metedata properties -->








