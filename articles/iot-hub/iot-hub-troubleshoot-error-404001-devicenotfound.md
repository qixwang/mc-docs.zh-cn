---
title: 排查 Azure IoT 中心错误 404001 DeviceNotFound
description: 了解如何修复错误 404001 DeviceNotFound
author: jlian
manager: briz
ms.service: iot-hub
services: iot-hub
ms.topic: troubleshooting
origin.date: 01/30/2020
ms.date: 02/17/2020
ms.author: v-yiso
ms.openlocfilehash: e64ab6fe49cef51c68169a268bb70460d0d39356
ms.sourcegitcommit: 925c2a0f6c9193c67046b0e67628d15eec5205c3
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/07/2020
ms.locfileid: "77068594"
---
# <a name="404001-devicenotfound"></a>404001 DeviceNotFound

本文介绍 **404001 DeviceNotFound** 错误的原因和解决方案。

## <a name="symptoms"></a>症状

在云到设备 (C2D) 通信期间（例如，C2D 消息、孪生更新或直接方法），操作失败且错误为 **404001 DeviceNotFound**。

## <a name="cause"></a>原因

操作失败，因为 IoT 中心找不到设备。 设备未注册或已禁用。

## <a name="solution"></a>解决方案

注册所用的设备 ID，然后重试。