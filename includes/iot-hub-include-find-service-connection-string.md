---
title: include 文件
description: include 文件
author: robinsh
ms.service: iot-hub
services: iot-hub
ms.topic: include
ms.date: 07/16/2019
ms.author: robinsh
ms.custom: include file
ms.openlocfilehash: 0c9ab261e36b8bf6ac09b7292fe650b1b727215b
ms.sourcegitcommit: c1ba5a62f30ac0a3acb337fb77431de6493e6096
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2020
ms.locfileid: "69993075"
---
<!-- This tells how to get the connection string for the service shared access policy of your IoT hub -->

若要获取 **service** 策略的 IoT 中心连接字符串，请执行以下步骤：

1. 在 [Azure 门户](https://portal.azure.cn)中，选择“资源组”  。 选择中心所在的资源组，然后从资源列表中选择中心。

1. 在 IoT 中心的左侧窗格中，选择“共享访问策略”  。

1. 从策略列表中选择“service”  策略。

1. 在“共享访问密钥”  下，选择“连接字符串 - 主密钥”  所对应的“复制”图标并保存该值。

    ![显示如何检索连接字符串](./media/iot-hub-include-find-service-connection-string/iot-hub-get-connection-string.png)

有关 IoT 中心共享访问策略和权限的详细信息，请参阅[访问控制和权限](../articles/iot-hub/iot-hub-devguide-security.md#access-control-and-permissions)。
