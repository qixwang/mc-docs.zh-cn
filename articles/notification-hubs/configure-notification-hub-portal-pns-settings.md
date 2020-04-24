---
title: 在 Azure 通知中心设置推送通知 | Azure
description: 了解如何在 Azure 门户中使用平台通知系统 (PNS) 设置来设置 Azure 通知中心。
services: notification-hubs
author: sethmanheim
manager: femila
editor: jwargo
ms.service: notification-hubs
ms.workload: mobile
ms.topic: quickstart
origin.date: 02/14/2019
ms.date: 12/09/2019
ms.author: v-tawe
ms.reviewer: jowargo
ms.lastreviewed: 02/14/2019
ms.openlocfilehash: dfd3a67a06a80f248b58934b0e09d5488f257dfb
ms.sourcegitcommit: c1ba5a62f30ac0a3acb337fb77431de6493e6096
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2020
ms.locfileid: "77501461"
---
# <a name="set-up-push-notifications-in-a-notification-hub-in-the-azure-portal"></a>使用 Azure 门户在通知中心设置推送通知

Azure 通知中心提供一个易于使用且可横向扩展的推送引擎。使用通知中心可将通知发送到任意平台（iOS、Android、Windows 和百度），也可从任意后端（云或本地）进行发送。 有关详细信息，请参阅[什么是 Azure 通知中心？](notification-hubs-push-notification-overview.md)。

在本快速入门中，你将使用通知中心的平台通知系统 (PNS) 设置在多个平台上设置推送通知。 快速入门介绍了在 Azure 门户中执行的步骤。

如果你尚未创建通知中心，现在请创建一个。 有关详细信息，请参阅[在 Azure 门户中创建 Azure 通知中心](create-notification-hub-portal.md)。 

## <a name="apple-push-notification-service"></a>Apple Push Notification 服务

设置 Apple Push Notification 服务 (APNS)：

1. 在 Azure 门户的“通知中心”页上，从左侧菜单中选择“Apple (APNS)”。  

1. 对于“身份验证模式”，请选择“证书”或“令牌”。   

   a. 如果选择“证书”： 
   * 选择“文件”图标，然后选择要上传的“.p12”文件。 
   * 输入密码。
   * 选择“沙盒”  模式。 或者，若要将推送通知发送给从应用商店中购买了你的应用的用户，请选择“生产”模式。 

     ![Azure 门户中 APNS 证书配置的屏幕截图](./media/notification-hubs-ios-get-started/notification-hubs-apple-config-cert.png)

   b. 如果选择“令牌”： 

   * 输入“密钥 ID”、“绑定 ID”、“团队 ID”和“令牌”的值     。
   * 选择“沙盒”  模式。 或者，若要将推送通知发送给从应用商店中购买了你的应用的用户，请选择“生产”模式。 

     ![Azure 门户中 APNS 令牌配置的屏幕截图](./media/configure-notification-hub-portal-pns-settings/notification-hubs-apple-config-token.png)

有关详细信息，请参阅[通过 Azure 通知中心向 iOS 推送通知](notification-hubs-ios-apple-push-notification-apns-get-started.md)。

<!-- ## Google Firebase Cloud Messaging -->


## <a name="windows-push-notification-service"></a>Windows 推送通知服务

设置 Windows 推送通知服务 (WNS)：

1. 在 Azure 门户的“通知中心”页上，从左侧菜单中选择“Windows (WNS)”。  
2. 输入“包 SID”和“安全密钥”的值。  
3. 选择“保存”  。

   ![显示“包 SID”和“安全密钥”框的屏幕截图](./media/notification-hubs-windows-store-dotnet-get-started/notification-hub-configure-wns.png)

有关信息，请参阅[使用 Azure 通知中心将通知发送到 UWP 应用](notification-hubs-windows-store-dotnet-get-started-wns-push-notification.md)。

## <a name="microsoft-push-notification-service-for-windows-phone"></a>适用于 Windows Phone 的 Microsoft 推送通知服务

设置适用于 Windows Phone 的 Microsoft 推送通知服务 (MPNS)： 

1. 在 Azure 门户的“通知中心”页上，从左侧菜单中选择“Windows Phone (MPNS)”。  
1. 启用未经身份验证或经过身份验证的通知：

   a. 若要启用未经身份验证的推送通知，请选择“启用未经身份验证的推送” > “保存”。  

      ![显示如何启用未经身份验证的推送通知的屏幕截图](./media/notification-hubs-windows-phone-get-started/azure-portal-unauth.png)

   b. 启用经过身份验证的推送通知：
      * 在工具栏上选择“上传证书”  。
      * 选择“文件”图标，然后选择证书文件。
      * 输入证书的密码。
      * 选择“确定”  。
      * 在“Windows Phone (MPNS)”页上选择“保存”。  

有关详细信息，请参阅[使用通知中心将通知推送到 Windows Phone 应用](notification-hubs-windows-mobile-push-notifications-mpns.md)。
      

## <a name="baidu-android-china"></a>Baidu (Android China)

为百度设置推送通知：

1. 在 Azure 门户的“通知中心”页上，从左侧菜单中选择“Baidu (Android China)”。   
2. 在百度云推送项目中，输入从百度控制台获取的“API 密钥”。  
3. 在百度云推送项目中，输入从百度控制台获取的“机密密钥”。  
4. 选择“保存”  。 

    ![通知中心的屏幕截图，其中显示了百度 (Android China) 的推送通知配置](./media/notification-hubs-baidu-get-started/AzureNotificationServicesBaidu.png)

完成这些步骤后，会有一条警报会指示已成功更新通知中心。 “保存”按钮已禁用。  

有关详细信息，请参阅[通过百度开始使用通知中心](notification-hubs-baidu-china-android-notifications-get-started.md)。

## <a name="next-steps"></a>后续步骤
本快速入门介绍了如何在 Azure 门户中为通知中心配置平台通知系统设置。 

若要详细了解如何将通知推送到各种平台，请参阅以下教程：

[使用通知中心和 Baidu 向 Android 设备推送通知](notification-hubs-baidu-china-android-notifications-get-started.md)

- [使用通知中心和 APNS 将通知推送到 iOS 设备](notification-hubs-ios-apple-push-notification-apns-get-started.md)
- [将通知推送到 Windows 设备上运行的 UWP 应用](notification-hubs-windows-store-dotnet-get-started-wns-push-notification.md)
- [使用 MPNS 将通知推送到 Windows Phone 8 应用](notification-hubs-windows-mobile-push-notifications-mpns.md)
- [使用通知中心和百度云推送来推送通知](notification-hubs-baidu-china-android-notifications-get-started.md)。
