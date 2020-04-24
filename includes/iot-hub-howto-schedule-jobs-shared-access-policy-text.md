---
title: include 文件
description: include 文件
author: robinsh
ms.service: iot-hub
services: iot-hub
ms.topic: include
ms.date: 07/17/2019
ms.author: robinsh
ms.custom: include file
ms.openlocfilehash: 55c3c96ab19019434c9fc7c7d5caf3877a9bc483
ms.sourcegitcommit: c1ba5a62f30ac0a3acb337fb77431de6493e6096
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2020
ms.locfileid: "69993102"
---
<!-- This contains intro text for the "Get an IoT hub connection string" section in the iot-hub-lang-lang-schedule-jobs.md files-->

在本文中，将创建一个后端服务，该服务调度作业以对设备调用直接方法，调度作业以更新设备孪生，并监视每个作业的进度。 若要执行这些操作，你的服务需要“注册表读取”  和“注册表写入”  权限。 默认情况下，每个 IoT 中心都使用名为 **registryReadWrite** 的共享访问策略创建，该策略授予这些权限。
