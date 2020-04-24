---
title: 应用程序同意操作的工作原理 |Microsoft 文档
description: 详细了解 Azure AD 同意框架的工作原理以便了解如何在 Azure AD上开发应用程序时使用该框架
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
ms.date: 06/24/2019
ms.author: v-junlch
ms.collection: M365-identity-device-management
ms.openlocfilehash: c5e3c1b9e3bbea383a69d54ceb6aa04bfcf0c335
ms.sourcegitcommit: c1ba5a62f30ac0a3acb337fb77431de6493e6096
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2020
ms.locfileid: "67568519"
---
# <a name="how-application-consent-works"></a>应用程序同意操作的工作原理

本文旨在帮助读者深入了解 Azure AD 同意框架的工作原理以便更有效地开发应用程序。

## <a name="recommended-documents"></a>建议的文档

- 概括性了解：[允许资源所有者如何通过同意操作管理应用程序对资源的访问权限](/active-directory/develop/active-directory-dev-glossary#consent)。
- 获取有关 [Azure AD 同意框架如何实现同意](/active-directory/develop/active-directory-integrating-applications)的分步概述。
- 有关详细信息，请参阅[多租户应用程序如何使用同意框架](/active-directory/develop/active-directory-devhowto-multi-tenant-overview)实现“用户”和“管理员”同意（支持更多高级多层应用程序模式）。
- 有关详细信息，请参阅[在授权代码授权流期间如何在 OAuth 2.0 协议层支持同意](/active-directory/develop/active-directory-protocols-oauth-code#request-an-authorization-code)

## <a name="next-steps"></a>后续步骤
[AzureAD StackOverflow](https://stackoverflow.com/questions/tagged/azure-active-directory)

<!-- Update_Description: update metedata properties -->