---
title: include 文件
description: include 文件
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: include
ms.date: 02/14/2020
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: 4a2995acca9347ad4367b2f752b50d425a8a4ae2
ms.sourcegitcommit: 4810b75d1e1db78d9747e99735468a6ab861be2d
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/26/2020
ms.locfileid: "80291307"
---
如果希望在授予访问权限之前提示用户提供第二个身份验证因素，可以配置 Azure 多重身份验证 (MFA)。 可以基于每个用户配置 MFA。

<!--Not Available on  or you can leverage MFA via [Conditional Access](../articles/active-directory/conditional-access/overview.md)-->

* 可以为每个用户启用 MFA，而不收费额外费用。 为每个用户启用 MFA 时，将提示用户针对绑定到 Azure AD 租户的所有应用程序进行第二因素身份验证。 有关步骤，请参阅[选项 1](#peruser)。

<!--Not Available on * Conditional Access allows for finer-grained control over how a second factor should be promoted. It can allow assignment of MFA to only VPN, and exclude other applications tied to the Azure AD tenant. See [Option 2](#conditional) for steps.-->