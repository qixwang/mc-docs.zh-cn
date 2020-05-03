---
title: 排查语音设备 SDK 问题 - 语音服务
titleSuffix: Azure Cognitive Services
description: 本文提供的信息可帮助你解决在使用语音设备 SDK 时可能遇到的问题。
services: cognitive-services
author: mswellsi
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
origin.date: 07/05/2019
ms.date: 01/13/2020
ms.author: v-tawe
ms.openlocfilehash: bfd9ab5af7978b8b44aa340bbd2ed819f116a957
ms.sourcegitcommit: a4a2521da9b29714aa6b511fc6ba48279b5777c8
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/24/2020
ms.locfileid: "82127009"
---
# <a name="troubleshoot-the-speech-devices-sdk"></a>排查语音设备 SDK 问题

> [!IMPORTANT]
> 需要语音 SDK 版本 1.10.0 或更高版本。

本文提供的信息可帮助你解决在使用语音设备 SDK 时可能遇到的问题。

## <a name="certificate-failures"></a>证书错误

如果在使用语音服务时遇到证书错误，请确保设备的日期和时间正确：

1. 转到“设置”  。 在“系统”下选择“日期和时间”。  

    ![在“设置”下选择“日期和时间”](media/speech-devices-sdk/qsg-12.png)

1. 将“自动日期和时间”选项保持选中状态。  在“选择时区”下，选择你的当前时区。 

    ![选择日期和时区选项](media/speech-devices-sdk/qsg-13.png)

    看到开发工具包的时间与电脑上的时间匹配时，表示开发工具包已连接到 Internet。

## <a name="next-steps"></a>后续步骤

* [查看发行说明](devices-sdk-release-notes.md)
