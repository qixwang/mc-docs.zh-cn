---
title: Azure Active Directory B2B 最佳做法和建议
description: 了解有关 Azure Active Directory 中企业到企业 (B2B) 来宾用户访问的最佳做法和建议。
services: active-directory
ms.service: active-directory
ms.subservice: B2B
ms.topic: conceptual
ms.date: 04/29/2020
ms.author: v-junlch
author: msmimart
manager: celestedg
ms.reviewer: elisol
ms.custom: it-pro
ms.collection: M365-identity-device-management
ms.openlocfilehash: d1f8f7b4bebc044ea3b39e952cfb9f8c0bf53a24
ms.sourcegitcommit: 95efd248f5ee3701f671dbd5cfe0aec9c9959a24
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/29/2020
ms.locfileid: "82515722"
---
# <a name="azure-active-directory-b2b-best-practices"></a>Azure Active Directory B2B 最佳做法
本文包含有关 Azure Active Directory (Azure AD) 中企业到企业 (B2B) 协作的建议和最佳做法。

## <a name="b2b-recommendations"></a>B2B 建议
| 建议 | 注释 |
| --- | --- |
| 将隐私声明添加到 B2B 来宾用户兑换体验 | 可以将组织隐私声明的 URL 添加到首次邀请兑换过程，使受邀用户必须同意你的隐私条款才能继续。 请参阅[操作指南：在 Azure Active Directory 中添加组织的隐私信息](/active-directory/fundamentals/active-directory-properties-area)。 |
| 使用批量邀请（预览版）功能同时邀请多个 B2B 来宾用户 | 使用 Azure 门户中的批量邀请预览版功能，同时邀请多个来宾用户加入你的组织。 使用此功能可以上传 CSV 文件来创建 B2B 来宾用户并批量发送邀请。 参阅[有关批量邀请 B2B 用户的教程](tutorial-bulk-invite.md)。 |
| 向 B2B 来宾用户提供直接链接时使用特定于租户的 URL | 可以向来宾提供应用或门户的直接链接，作为邀请电子邮件的替代方法。 此直接链接必须特定于租户。这意味着，它必须包含租户 ID 或已验证的域，以便能够在共享应用所在的租户中对来宾进行身份验证。 请参阅[来宾用户的兑换体验](redemption-experience.md)。 |
| 开发应用时，使用 UserType 来确定来宾用户体验  | 如果你正在开发某个应用程序，并想要为租户用户和来宾用户提供不同的体验，请使用 UserType 属性。 令牌中当前未包括 UserType 声明。 应用程序应使用 Microsoft Graph API 从目录中查询用户以获取其 UserType。 |
| 只有在用户与组织之间的关系发生变化的情况下，才更改 UserType 属性  | 虽然可以使用 PowerShell 将用户的 UserType 属性从 Member 转换为 Guest（反之亦然），但是，只有在用户与组织之间的关系发生变化的情况下，才应更改此属性。 请参阅 [B2B 来宾用户的属性](user-properties.md)。|

## <a name="next-steps"></a>后续步骤

[管理 B2B 共享](delegate-invitations.md)

