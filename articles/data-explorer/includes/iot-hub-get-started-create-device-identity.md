---
title: include 文件
description: include 文件
services: iot-hub
author: dominicbetts
ms.service: iot-hub
ms.topic: include
origin.date: 09/07/2018
ms.date: 05/15/2020
ms.author: v-tawe
ms.custom: include file
ms.openlocfilehash: 6a9c42b4699f2235b596e5daf00bf4126592aac0
ms.sourcegitcommit: bfbd6694da33f703481386f2a3f16850c4e94bfa
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/15/2020
ms.locfileid: "83417755"
---
在本部分中，将使用 Azure CLI 为本文创建设备标识。 设备 ID 区分大小写。

1. 在 Azure CLI 中，运行以下命令以安装适用于 Azure CLI 的 Microsoft Azure IoT 扩展：

    ```azurecli
    az extension add --name azure-iot
    ```

2. 使用以下命令创建一个名为 `myDeviceId` 的新设备标识并检索设备连接字符串：

    ```azurecli
    az iot hub device-identity create --device-id myDeviceId --hub-name {Your IoT Hub name}
    az iot hub device-identity show-connection-string --device-id myDeviceId --hub-name {Your IoT Hub name} -o table
    ```

   [!INCLUDE [iot-hub-pii-note-naming-device](iot-hub-pii-note-naming-device.md)]

记下结果中的设备连接字符串。 设备应用使用此设备连接字符串以设备身份连接到 IoT 中心。

<!-- images and links -->
