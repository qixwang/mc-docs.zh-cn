---
title: 将推送通知添加到 iOS
description: 了解如何使用 Azure 移动应用将推送通知发送到 iOS 应用。
ms.assetid: fa503833-d23e-4925-8d93-341bb3fbab7d
ms.tgt_pltfrm: mobile-ios
ms.devlang: objective-c
ms.topic: article
origin.date: 06/25/2019
md.date: 03/23/2020
ms.author: v-tawe
ms.openlocfilehash: 292cedb3b37acd874eecda687ea82710885fcc84
ms.sourcegitcommit: c1ba5a62f30ac0a3acb337fb77431de6493e6096
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2020
ms.locfileid: "79084419"
---
# <a name="add-push-notifications-to-your-ios-app"></a>将推送通知添加到 iOS 应用

[!INCLUDE [app-service-mobile-selector-get-started-push](../../includes/app-service-mobile-selector-get-started-push.md)]

## <a name="overview"></a>概述

本教程介绍如何向 [iOS 快速入门]项目添加推送通知，以便每次插入一条记录时，都向设备发送一条推送通知。

如果不使用下载的快速入门服务器项目，则需要推送通知扩展包。 有关详细信息，请参阅[使用适用于 Azure 移动应用的 .NET 后端服务器 SDK](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md) 指南。

[iOS 模拟器不支持推送通知](https://developer.apple.com/library/ios/documentation/IDEs/Conceptual/iOS_Simulator_Guide/TestingontheiOSSimulator.html)。 需要一个物理 iOS 设备和 [Apple 开发人员计划成员身份](https://developer.apple.com/programs/ios/)。

## <a name="configure-notification-hub"></a><a name="configure-hub"></a>配置通知中心

[!INCLUDE [app-service-mobile-configure-notification-hub](../../includes/app-service-mobile-configure-notification-hub.md)]

## <a name="register-app-for-push-notifications"></a><a id="register"></a>为推送通知注册应用

[!INCLUDE [Enable Apple Push Notifications](../../includes/enable-apple-push-notifications.md)]

## <a name="configure-azure-to-send-push-notifications"></a>配置 Azure 以发送推送通知

[!INCLUDE [app-service-mobile-apns-configure-push](../../includes/app-service-mobile-apns-configure-push.md)]

## <a name="update-backend-to-send-push-notifications"></a><a id="update-server"></a>更新后端以发送推送通知

[!INCLUDE [app-service-mobile-dotnet-backend-configure-push-apns](../../includes/app-service-mobile-dotnet-backend-configure-push-apns.md)]

## <a name="add-push-notifications-to-app"></a><a id="add-push"></a>向应用添加推送通知

[!INCLUDE [app-service-mobile-add-push-notifications-to-ios-app.md](../../includes/app-service-mobile-add-push-notifications-to-ios-app.md)]

## <a name="test-push-notifications"></a><a id="test"></a>测试推送通知

[!INCLUDE [Test Push Notifications in App](../../includes/test-push-notifications-in-app.md)]

## <a name="more"></a><a id="more"></a>更多

* 使用模板可以灵活地发送跨平台推送和本地化推送。 [如何使用适用于 Azure 移动应用的 iOS 客户端库](app-service-mobile-ios-how-to-use-client-library.md#templates)说明了如何注册模板。

<!-- Anchors.  -->

<!-- Images. -->

<!-- URLs. -->
[iOS 快速入门]: app-service-mobile-ios-get-started.md
