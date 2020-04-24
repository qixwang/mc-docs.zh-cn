---
title: 如何为给定 API 选择权限 | Microsoft Docs
description: 如何使用 Azure AD 查找要开发或注册的自定义应用程序的身份验证终结点。
services: active-directory
documentationcenter: ''
author: rwike77
manager: CelesteDG
ms.assetid: ''
ms.service: active-directory
ms.subservice: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
origin.date: 09/11/2018
ms.date: 07/01/2019
ms.author: v-junlch
ms.collection: M365-identity-device-management
ms.openlocfilehash: ef0632f7d3973e11ef718be5be0db16b25f5d0b9
ms.sourcegitcommit: c1ba5a62f30ac0a3acb337fb77431de6493e6096
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2020
ms.locfileid: "67568571"
---
# <a name="how-to-configure-endpoints"></a>如何配置终结点

可以在 [Azure 门户](https://portal.azure.cn)中找到应用程序的身份验证终结点。

-   导航到 [Azure 门户](https://portal.azure.cn)。

-   在左侧导航窗格中，单击“Azure Active Directory”  。

-   单击“应用注册”  并选择“终结点”  。

-   这会打开“终结点”  页，该页会列出租户的所有身份验证终结点。

-   将特定于要使用的身份验证协议的终结点与应用程序 ID 结合使用，生成特定于应用程序的身份验证请求。

## <a name="next-steps"></a>后续步骤
[Azure Active Directory 开发人员指南](/active-directory/develop/active-directory-developers-guide)

<!-- Update_Description: update metedata properties -->