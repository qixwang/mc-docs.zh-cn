---
title: Azure AD Connect：预览版功能 | Microsoft Docs
description: 本主题详细介绍 Azure AD Connect 中以预览版形式提供的功能。
services: active-directory
documentationcenter: ''
author: billmath
manager: daveba
editor: ''
ms.assetid: c75cd8cf-3eff-4619-bbca-66276757cc07
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: how-to
ms.date: 07/06/2020
ms.subservice: hybrid
ms.author: v-junlch
ms.collection: M365-identity-device-management
ms.openlocfilehash: 1b2debff5bab6405e7b9d3c906967a2415e85f9f
ms.sourcegitcommit: 92b9b1387314b60661f5f62db4451c9ff2c49500
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/09/2020
ms.locfileid: "86165048"
---
# <a name="more-details-about-features-in-preview"></a>有关预览版功能的详细信息
本主题介绍如何使用当前以预览版形式提供的功能。

## <a name="azure-ad-connect-sync-v2-endpoint-api-public-preview"></a>Azure AD Connect 同步 V2 终结点 API（公共预览版） 

我们已部署新的 Azure AD Connect 终结点 (API)，可提高 Azure Active Directory 的同步服务操作性能。 通过利用新的 V2 终结点，导出或导入 Azure AD 时的性能会有显著提升。 这个新的终结点还支持同步最多包含 250000 个成员的组。 有关详细信息，请参阅 [Azure AD Connect 同步 V2 终结点 API（公共预览版）](how-to-connect-sync-endpoint-api-v2.md)。

## <a name="user-writeback"></a>用户写回
> [!IMPORTANT]
> Azure AD Connect 的 2015 年 8 月更新版中删除了用户写回预览版功能。 如果已启用此功能，现在应将它禁用。
>
>

## <a name="next-steps"></a>后续步骤
配置 [Azure AD Connect 的自定义安装](how-to-connect-install-custom.md)。

了解有关 [将本地标识与 Azure Active Directory 集成](whatis-hybrid-identity.md)的详细信息。

