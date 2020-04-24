---
title: include 文件
description: include 文件
author: robinsh
ms.service: iot-hub
services: iot-hub
ms.topic: include
ms.date: 08/07/2019
ms.author: robinsh
ms.custom: include file
ms.openlocfilehash: 1e86877c91e077040048573a91d49a7096958429
ms.sourcegitcommit: c1ba5a62f30ac0a3acb337fb77431de6493e6096
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2020
ms.locfileid: "69993106"
---
<!-- This contains intro text for the "Get an IoT hub connection string" section in the iot-hub-lang-lang-twin-getstarted.md files-->

在本文中，将创建一个后端服务，该服务将所需的属性添加到设备孪生，然后查询标识注册表以查找具有已相应更新的报告属性的所有设备。 你的服务需要“服务连接”  权限才能修改设备孪生的所需属性，并且需要“注册表读取”  权限才能查询标识注册表。 没有仅包含这两个权限的默认共享访问策略，因此需要创建一个。
