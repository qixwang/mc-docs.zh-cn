---
title: include 文件
description: include 文件
services: virtual-network
author: rockboyfor
ms.service: virtual-network
ms.topic: include
origin.date: 04/13/2018
ms.date: 06/11/2018
ms.author: v-yeche
ms.custom: include file
ms.openlocfilehash: e839cf1b29c5375f3fa52be668f54f8426eb814c
ms.sourcegitcommit: c1ba5a62f30ac0a3acb337fb77431de6493e6096
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2020
ms.locfileid: "63823291"
---
## <a name="scenario"></a>场景
为了更好地说明如何创建 UDR，本文档使用以下方案：

![图像说明](./media/virtual-network-create-udr-scenario-include/figure1.png)

在此方案中，需要为*前端子网*创建一个 UDR，为*后端子网*创建另一个 UDR，如下所述： 

* **UDR-FrontEnd**。 前端 UDR 应用于 *FrontEnd* 子网，并且包含一个路由：    
  * **RouteToBackend**。 此路由将目的地是后端子网的所有流量发送到 **FW1** 虚拟机。
* **UDR-BackEnd**。 后端 UDR 应用于 *BackEnd* 子网，并且包含一个路由：    
  * **RouteToFrontend**。 此路由将目的地是前端子网的所有流量发送到 **FW1** 虚拟机。

这些路由的组合将确保从一个子网发送到另一个子网的所有流量都将路由到正用作虚拟设备的 **FW1** 虚拟机。 还需要为 **FW1** VM 开启 IP 转发，以确保它可以接收目的地是其他 VM 的流量。
