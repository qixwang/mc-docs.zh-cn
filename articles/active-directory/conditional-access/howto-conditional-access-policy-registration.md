---
title: 条件访问 - 组合安全信息 - Azure Active Directory
description: 创建要求在受信任位置进行安全信息注册的自定义条件访问策略
services: active-directory
ms.service: active-directory
ms.subservice: conditional-access
ms.topic: conceptual
ms.date: 01/09/2020
ms.author: v-junlch
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: calebb, rogoya
ms.collection: M365-identity-device-management
ms.openlocfilehash: e74f5eb0e1227b7b38e55775b7d9c7d8df517f2a
ms.sourcegitcommit: bc5f8b4f8ccd7c723f64055825508d1dfcc2162b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/10/2020
ms.locfileid: "75859272"
---
# <a name="conditional-access-require-trusted-location-for-mfa-registration"></a>条件访问：要求在进行 MFA 注册时处于受信任的位置

使用“条件访问策略”中的“用户操作”，现在可对用户注册 Azure 多重身份验证和自助式密码重置的时间和方式提供保护。 此预览功能适用于已启用组合注册预览的组织。 如果组织要使用受信任的网络位置等条件来限制对 Azure 重身份验证和 SSPR 的注册访问，则可启用此功能。 有关在条件访问中创建受信任位置的详细信息，请参阅文档 [Azure Active Directory 条件访问中的位置条件是什么？](../conditional-access/location-condition.md#named-locations)

## <a name="create-a-policy-to-require-registration-from-a-trusted-location"></a>创建要求从受信任的位置注册的策略

以下策略适用于所有试图使用组合注册体验进行注册的选定用户，并将阻止访问，除非他们从标记为受信任网络的位置进行连接。

1. 在“Azure 门户”  中，浏览到“Azure Active Directory”   > “安全性”   > “条件访问”  。
1. 选择“新策略”  。
1. 在“名称”中，输入此策略的名称。 例如，受信任网络上的组合安全信息注册  。
1. 在“分配”  下，单击“用户和组”  ，并选择此策略适用的用户和组。

   > [!WARNING]
   > 对于组合注册预览，必须启用用户。

1. 在“云应用或操作”  下，选择“用户操作”  ，选中“注册安全信息(预览)”  。
1. 在“条件”   >   “位置”下：
   1. 配置：“是”  。
   1. 包括：“任何位置”  。
   1. 排除：“所有受信任的位置”  。
   1. 单击“位置”边栏选项卡上的“完成”  。
   1. 单击“条件”边栏选项卡上的“完成”  。
1. 在“访问控制”   >   “授予”下：
   1. 单击“阻止访问”  。
   1. 然后单击“选择”  。
1. 将“启用策略”设置为“打开”。  
1. 然后单击“保存”  。

## <a name="next-steps"></a>后续步骤

[条件访问常见策略](concept-conditional-access-policy-common.md)

[使用条件访问 What If 工具模拟登录行为](troubleshoot-conditional-access-what-if.md)

