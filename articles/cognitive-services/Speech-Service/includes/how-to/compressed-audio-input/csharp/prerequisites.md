---
author: trevorbye
ms.service: cognitive-services
ms.topic: include
origin.date: 03/09/2020
ms.date: 04/20/2020
ms.author: v-tawe
ms.openlocfilehash: 799fcd9d5673309640cd42cba349a20fa28a1ef5
ms.sourcegitcommit: a4a2521da9b29714aa6b511fc6ba48279b5777c8
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/24/2020
ms.locfileid: "82127014"
---
处理压缩音频是使用 [GStreamer](https://gstreamer.freedesktop.org) 实现的。 出于许可原因，GStreamer 二进制文件未编译，也未与语音 SDK 链接。 开发人员需要安装几个依赖项和插件，请参阅[在 Windows 上安装](https://gstreamer.freedesktop.org/documentation/installing/on-windows.html?gi-language=c)。 Gstreamer 二进制文件需要在系统路径中，以便语音 SDK 可以在运行时加载 Gstreamer 二进制文件。 如果语音 SDK 能够在运行时找到 libgstreamer-1.0-0.dll，这意味着 gstreamer 二进制文件在系统路径中。

