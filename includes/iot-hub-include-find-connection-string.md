---
title: include 文件
description: include 文件
author: robinsh
ms.service: iot-hub
services: iot-hub
ms.topic: include
ms.date: 11/02/2018
ms.author: robinsh
ms.custom: include file
ms.openlocfilehash: 4b303e47abe3f6764a4307e2748d1cb29dbb4c37
ms.sourcegitcommit: c1ba5a62f30ac0a3acb337fb77431de6493e6096
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2020
ms.locfileid: "63823090"
---
<!-- this tells how to get the connection string for your hub -->
<!-- This assumes the user is looking at his hub in the portal. -->

创建中心以后，请检索中心的连接字符串。 该字符串用于将设备和应用程序连接到中心。 

1. 单击中心，查看“IoT 中心”窗格，其中包含“设置”等内容。 单击“共享访问策略”。 
   
2. 在“共享访问策略”中，选择 **iothubowner** 策略。  

3. 在“共享访问密钥”下，复制“连接字符串 -- 主密钥”供以后使用   。

    ![显示如何检索连接字符串](./media/iot-hub-include-find-connection-string/iot-hub-get-connection-string.png)

    有关详细信息，请参阅“IoT 中心开发人员指南”中的[访问控制](../articles/iot-hub/iot-hub-devguide-security.md)。
