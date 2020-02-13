---
title: 排查 Azure IoT 中心错误 404103 DeviceNotOnline
description: 了解如何修复错误 404103 DeviceNotOnline
author: jlian
manager: briz
ms.service: iot-hub
services: iot-hub
ms.topic: troubleshooting
origin.date: 01/30/2020
ms.date: 02/17/2020
ms.author: v-yiso
ms.openlocfilehash: a667262bfa817da64c430f80705b733337bf5ef0
ms.sourcegitcommit: 925c2a0f6c9193c67046b0e67628d15eec5205c3
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/07/2020
ms.locfileid: "77068547"
---
# <a name="404103-devicenotonline"></a>404103 DeviceNotOnline

本文介绍 **404103 DeviceNotOnline** 错误的原因和解决方案。

## <a name="symptoms"></a>症状

即使设备处于联机状态，对设备执行直接方法也会失败，并显示错误 **404103 DeviceNotOnline**。 

## <a name="cause"></a>原因

如果你知道设备处于联机状态，但仍收到错误，很可能是因为直接方法回调未在设备上注册。

## <a name="solution"></a>解决方案

若要为直接方法回调正确配置设备，请参阅[在设备上处理直接方法](iot-hub-devguide-direct-methods.md#handle-a-direct-method-on-a-device)。