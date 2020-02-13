---
title: 排查 Azure IoT 中心错误 409001 DeviceAlreadyExists
description: 了解如何修复错误 409001 DeviceAlreadyExists
author: jlian
manager: briz
ms.service: iot-hub
services: iot-hub
ms.topic: troubleshooting
origin.date: 01/30/2020
ms.date: 02/17/2020
ms.author: v-yiso
ms.openlocfilehash: 688330b4a9bb06f1ddb648fe349693fdffeeafe5
ms.sourcegitcommit: 925c2a0f6c9193c67046b0e67628d15eec5205c3
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/07/2020
ms.locfileid: "77068600"
---
# <a name="409001-devicealreadyexists"></a>409001 DeviceAlreadyExists

本文介绍 **409001 DeviceAlreadyExists** 错误的原因和解决方案。

## <a name="symptoms"></a>症状

尝试在 IoT 中心注册设备时，请求失败且错误为 **409001 DeviceAlreadyExists**。

## <a name="cause"></a>原因

IoT 中心中已存在具有相同设备 ID 的设备。 

## <a name="solution"></a>解决方案

请使用其他设备 ID，然后重试。