---
title: Azure IoT Central 中的设备连接 | Microsoft Docs
description: 本文概述如何通过 Azure IoT Central 让设备保持连接状态和正常运行状态。
author: aaronbjork
ms.author: abjork
origin.date: 10/21/2019
ms.date: 12/16/2019
ms.topic: conceptual
ms.service: iot-central
services: iot-central
manager: ''
ms.openlocfilehash: 8fcf8887b65af8a1b93a3cc71b7d62127fd16e73
ms.sourcegitcommit: c1ba5a62f30ac0a3acb337fb77431de6493e6096
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2020
ms.locfileid: "74883222"
---
# <a name="stay-connected-with-azure-iot-central-preview-features"></a>始终连接到 Azure IoT Central（预览版功能）

[!INCLUDE [iot-central-pnp-original](../../../includes/iot-central-pnp-original-note.md)]

本文概述如何通过 Azure IoT Central 让设备保持连接状态和正常运行状态。

使用任何旨在进行大规模操作的 IoT 解决方案时，必须通过构造良好的方法进行设备管理。 仅仅让设备“连接”到云是不够的，还需要在解决方案演化、扩大和老化时通过一种方法让设备保持连接状态和正常运行状态。 Azure IoT Central 提供各种必需的功能，确保 IoT 解决方案中的设备在应用程序的整个生命周期都得到很好的维护。

## <a name="dashboards"></a>仪表板 
内置的[仪表板](howto-manage-devices.md#import-devices)提供了一个可自定义的图面区域，用于监视设备运行状况和遥测。 一开始使用[应用程序模板](howto-use-app-templates.md)中提供的预构建仪表板，或者根据应用程序需求创建你自己的仪表板。 仪表板可以与应用程序中的所有用户共享，也可以始终专用。

## <a name="rules-and-actions"></a>规则和操作 
可以根据设备状态和遥测构建[自定义规则](tutorial-create-telemetry-rules.md)，快速确定需要关注的设备。 配置操作，以便向合适的人员发送通知，确保及时采取纠正措施。

## <a name="jobs"></a>作业 
使用[作业](../core/howto-run-a-job.md?toc=/iot-central/preview/toc.json&bc=/iot-central/preview/breadcrumb/toc.json)可以对设备属性、设置和命令逐个进行更新或批量进行更新。 

## <a name="user-roles-and-permissions"></a>用户角色和权限
可以通过[角色和权限](howto-manage-users-roles.md)定制每个用户的体验。 直接通过 UI 创建角色，轻松地分配适当的权限。 




