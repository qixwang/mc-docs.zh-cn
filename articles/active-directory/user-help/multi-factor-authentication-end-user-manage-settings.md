---
title: 管理双重验证设置 - Azure Active Directory | Microsoft Docs
description: 管理 Azure 多重身份验证的使用方式包括更改联系信息或配置设备。
services: active-directory
keywords: 多重身份验证客户端, 身份验证问题, 相关性 ID
author: eross-msft
manager: mtillman
ms.reviewer: richagi
ms.assetid: d3372d9a-9ad1-4609-bdcf-2c4ca9679a3b
ms.workload: identity
ms.service: active-directory
ms.component: user-help
ms.topic: conceptual
origin.date: 05/23/2017
ms.date: 08/27/2018
ms.author: v-junlch
ms.openlocfilehash: ba776aef90009113448c23b5625f66054accdecf
ms.sourcegitcommit: b80d236ce3c706abc25bbaa41b0ccddd896e48fc
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/22/2020
ms.locfileid: "81873130"
---
# <a name="manage-your-settings-for-two-step-verification"></a>管理双重验证设置
本文回答有关如何更新双重验证或多重身份验证的设置的问题。 如果在登录帐户时遇到问题，请参阅[使用双重验证时遇到问题](multi-factor-authentication-end-user-troubleshoot.md)获取疑难解答帮助。

## <a name="where-to-find-the-settings-page"></a>哪里可以找到设置页
具体取决于公司设置 Azure 多重身份验证的方式，可在其中几个位置更改设置，例如电话号码。

如果 IT 管理员发出特定的 URL 或步骤管理双重验证，请按照这些说明进行操作。 否则，以下说明应适用于其他所有人。 如果按照这些步骤操作，但未看到相同的选项，这意味着工作场所或学校自定义了它们自己的门户。 向管理员寻求指向 Azure 多重身份验证门户的链接。

1. 登录到 [https://login.partner.microsoftonline.cn](https://login.partner.microsoftonline.cn)  

    ![1](./media/multi-factor-authentication-end-user-manage/1.png)  

    输入用户帐户和密码，并单击“登录”。  

2. 选择所需的验证。

   - 默认值为“身份验证电话”，可以选择接收短信或通话。
        
     ![2](./media/multi-factor-authentication-end-user-manage/2.png)  

   - 也可以选择“办公电话”。
    
       ![3](./media/multi-factor-authentication-end-user-manage/3.png)     
    
   - 还可以选择“移动电话”，以便安装执行验证的“Azure 身份验证应用”。
    
       ![4](./media/multi-factor-authentication-end-user-manage/4.png) 


## <a name="i-want-to-change-my-phone-number"></a>我想要更改我的电话号码

若要更改电话号码，可执行以下步骤。

1. 登录 [Azure 门户](https://portal.azure.cn/)

2. 从 Active Directory 进入“多重身份验证”配置页。

3. 单击“管理用户设置”。 会有一个弹出页。

    ![5](./media/multi-factor-authentication-end-user-manage/5.png)  

4. 选择页面上的“要求选定用户重新提供联系方式”。

    ![6](./media/multi-factor-authentication-end-user-manage/6.png)  

5. 单击“保存”。

现在，下次登录 [https://login.partner.microsoftonline.cn](https://login.partner.microsoftonline.cn) 时，就可以选择新的验证方法或电话号码。

## <a name="how-do-i-clean-up-microsoft-authenticator-from-my-old-device-and-move-to-a-new-one"></a>如何从旧设备清除 Microsoft Authenticator 并将其迁移到新设备？
从设备上卸载该应用或重置设备时，不会删除应用在后端的激活。 有关详细信息，请参阅 [Microsoft Authenticator](./microsoft-authenticator-app-how-to.md)。

## <a name="next-steps"></a>后续步骤
- 在[使用双重验证时遇到问题](./multi-factor-authentication-end-user-troubleshoot.md)中获得疑难解答提示和帮助
<!-- Update_Description: update metedata properties -->
