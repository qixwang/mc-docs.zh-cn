---
title: Azure AD 应用同意体验
titleSuffix: Microsoft identity platform
description: 详细了解 Azure AD 许可体验，确定如何在管理和开发基于 Azure AD 的应用程序时使用它
services: active-directory
author: rwike77
manager: CelesteDG
ms.assetid: ''
ms.service: active-directory
ms.subservice: develop
ms.custom: aaddev
ms.workload: identity
ms.topic: conceptual
ms.date: 03/10/2020
ms.author: v-junlch
ms.reviewer: zachowd
ms.openlocfilehash: 513e69ea9423493c61884a453121b04f9ae9603d
ms.sourcegitcommit: 4ba6d7c8bed5398f37eb37cf5e2acafcdcc28791
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/12/2020
ms.locfileid: "79133830"
---
# <a name="understanding-azure-ad-application-consent-experiences"></a>了解 Azure AD 应用程序许可体验

详细了解 Azure Active Directory (Azure AD) 应用程序许可用户体验， 以便通过智能方式为组织管理应用程序，并且/或者开发可提供更流畅许可体验的应用程序。

## <a name="consent-and-permissions"></a>许可和权限

许可是指用户进行应用程序授权，让应用程序代表自己来访问受保护资源的过程。 可以要求管理员或用户同意访问其组织数据/个人数据。

授予许可时的实际用户体验取决于以下因素：在用户的租户上设置的策略、用户的权限范围（或角色）、客户端应用程序请求的[权限](/active-directory/azuread-dev/v1-permissions-consent)的类型。 这意味着应用程序开发人员和租户管理员可以对许可体验进行某种控制。 管理员可以灵活地设置和禁用租户或应用上的策略，以便控制其租户中的许可体验。 应用程序开发人员可以规定哪些类型的权限可以请求，以及是否需要引导用户完成用户许可流或管理员许可流。

- **用户许可流**：应用程序开发人员将用户引导到授权终结点，目的是只记录当前用户的许可。
- **管理员许可流**：应用程序开发人员将用户引导到管理员许可终结点，目的是记录整个租户的许可。 若要确保管理员许可流正常运行，应用程序开发人员必须在应用程序清单的 `RequiredResourceAccess` 属性中列出所有权限。 有关详细信息，请参阅[应用程序清单](/active-directory/develop/reference-app-manifest)。

## <a name="building-blocks-of-the-consent-prompt"></a>许可提示的构建基块

许可提示旨在确保用户可以根据足够的信息来确定是否应信任客户端应用程序代表自己来访问受保护资源。 了解构建基块有助于授予许可的用户进行更明智的决策，并有助于开发人员构建更好的用户体验。

下面的图和表介绍了许可提示的构建基块。

![许可提示的构建基块](./media/application-consent-experience/consent_prompt.png)

| # | 组件 | 目的 |
| ----- | ----- | ----- |
| 1 | 用户标识符 | 此标识符代表的用户是客户端应用程序请求访问受保护资源时代表的用户。 |
| 2 | 标题 | 此标题会更改，具体取决于用户是进入用户许可流还是管理员许可流。 在用户许可流中，标题会是“请求权限”，而在管理员许可流中，标题会有另外一行，即“代表组织接受”。 |
| 3 | 应用徽标 | 用户应该可以通过此图像直观地确定此应用是否是其要访问的应用。 此图像由应用程序开发人员提供，系统不会验证其所有权。 |
| 4 | 应用程序名称 | 此值会告知用户，哪个应用程序正请求访问其数据。 请注意，此应用名称由开发人员提供，系统不会验证其所有权。 |
| 5 | 发行者域 | 此值会为用户提供一个域，供用户评估其可信度。 此发行者域由开发人员提供，系统不会验证其所有权。 |
| 6 | 权限 | 此列表包含客户端应用程序请求的权限。 用户应始终评估请求的权限的类型，以便了解客户端应用程序在获得授权后将要代表用户访问的具体数据，前提是用户接受请求。 作为应用程序开发人员，最好是通过最低特权对请求访问权限进行限制。 |
| 7 | 权限说明 | 此值由公开权限的服务提供。 若要查看权限说明，必须切换权限旁边的 V 形图标。 |
| 8 | 应用条款 | 这些条款包含指向服务条款和应用程序隐私声明的链接。 发行商负责在其服务条款中概述其规则。 另外，发行商负责在其隐私声明中披露其使用和共享用户数据的方式。 如果发行商没有为多租户应用程序提供这些值的链接，则会在许可提示中显示一个加粗的警告。 |

## <a name="common-consent-scenarios"></a>常见许可场景

下面是用户可能会在常见许可场景中看到的许可体验：

1. 用户访问一个应用，该应用将其引导到用户许可流，同时要求一个在其权限范围内的许可集。
    
    1. 管理员会在传统的许可提示中看到一个额外的控件，可以通过该控件代表整个租户进行许可。 该控件将默认设置为关闭，因此只有在管理员显式勾选此框的情况下，才能由其代表整个租户授予许可。 目前只为全局管理员角色显示此复选框，因此云管理员和应用管理员看不到此复选框。

        ![场景 1a 的许可提示](./media/application-consent-experience/consent_prompt_1a.png)
    
    2. 用户会看到传统的许可提示。

        ![场景 1b 的许可提示](./media/application-consent-experience/consent_prompt_1b.png)

2. 用户访问一个应用，该应用要求至少一个在用户权限范围外的许可。
    1. 管理员会看到与上面显示的 1.i 相同的提示。
    2. 系统会阻止用户向应用程序授予许可，并且会要求用户向其管理员寻求应用的访问权限。 
                
        ![场景 1b 的许可提示](./media/application-consent-experience/consent_prompt_2b.png)

3. 用户导航到或者被引导到管理员许可流。
    1. 管理员用户会看到管理员许可提示。 此提示的标题和权限说明已更改，所做的更改突出显示这样一个事实：接受此提示会授予应用代表整个租户访问请求的数据的权限。
        
        ![场景 1b 的许可提示](./media/application-consent-experience/consent_prompt_3a.png)
        
    1. 非管理员用户会看到与上面显示的 2.ii 相同的屏幕。

## <a name="next-steps"></a>后续步骤
- 获取有关 [Azure AD 同意框架如何实现同意](/active-directory/develop/active-directory-integrating-applications)的分步概述。
- 如需更深入的了解，请参阅[多租户应用程序如何使用许可框架](active-directory-devhowto-multi-tenant-overview.md)来实现“用户”许可和“管理员”许可，为更高级的多层应用程序模式提供支持。
- 了解[如何配置应用的发布者域](howto-configure-publisher-domain.md)。

