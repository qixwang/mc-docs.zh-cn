---
title: 在 Azure 通知中心配置百度云推送 | Azure
description: 了解如何为 Azure 通知中心配置百度设置。
services: notification-hubs
author: sethmanheim
manager: femila
ms.service: notification-hubs
ms.workload: mobile
ms.topic: article
origin.date: 03/25/2019
ms.date: 12/09/2019
ms.author: v-tawe
ms.reviewer: jowargo
ms.lastreviewed: 03/25/2019
ms.openlocfilehash: f628667fbca01f2f411b204a37c36dbd18345e97
ms.sourcegitcommit: c1ba5a62f30ac0a3acb337fb77431de6493e6096
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2020
ms.locfileid: "74884915"
---
# <a name="configure-baidu-cloud-push-settings-for-a-notification-hub-in-the-azure-portal"></a>在 Azure 门户中为通知中心配置百度云推送设置

本文介绍如何使用 Azure 门户为 Azure 通知中心配置百度云推送设置。


## <a name="prerequisites"></a>必备条件
如果尚未创建通知中心，请立即创建。 有关详细信息，请参阅[在 Azure 门户中创建 Azure 通知中心](create-notification-hub-portal.md)。 

## <a name="configure-baidu-cloud-push"></a>配置百度云推送
以下过程提供的步骤演示了如何为通知中心配置百度云推送设置：

1. 在 Azure 门户的“通知中心”页上，在左侧菜单中选择“Baidu (Android China)”。   
2. 在百度云推送项目中，输入从百度控制台获得的 API 密钥  。 
3. 在百度云推送项目中，输入从百度控制台获得的机密密钥  。 
4. 选择“保存”。  

    ![显示百度 (Android China) 推送通知配置的通知中心屏幕截图](./media/notification-hubs-baidu-get-started/AzureNotificationServicesBaidu.png)

## <a name="next-steps"></a>后续步骤
如需通过教程的分步说明来了解如何使用 Azure 通知中心和百度云推送将通知推送到百度，请参阅[使用百度的通知中心入门](notification-hubs-baidu-china-android-notifications-get-started.md)。
