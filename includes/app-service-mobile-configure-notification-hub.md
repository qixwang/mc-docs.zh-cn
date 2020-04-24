---
author: conceptdev
ms.author: v-tawe
ms.service: app-service-mobile
ms.topic: include
origin.date: 08/23/2018
ms.date: 12/16/2019
ms.openlocfilehash: 1f07e05a24909e1f1b1a44d400dc2ab0e59f294a
ms.sourcegitcommit: c1ba5a62f30ac0a3acb337fb77431de6493e6096
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2020
ms.locfileid: "75334627"
---
Azure 应用服务的移动应用功能使用 [Azure 通知中心]发送推送内容，因此用户需为移动应用配置通知中心。

1. 在 [Azure 门户]中，转到“应用服务”  ，并选择应用后端。 在“设置”  下，选择“推送”  。
2. 若要将通知中心资源添加到应用中，请选择“连接”  。 可以创建一个中心，也可以连接到一个现有的中心。

    ![配置中心](./media/app-service-mobile-create-notification-hub/configure-hub-flow.png)

现在已将通知中心连接到移动应用后端项目。 稍后需对此通知中心进行配置，以便连接到将内容推送到设备的平台通知系统 (PNS)。

[Azure 门户]: https://portal.azure.cn/
[Azure 通知中心]: /notification-hubs/notification-hubs-push-notification-overview