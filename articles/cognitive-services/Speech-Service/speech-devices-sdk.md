---
title: 关于语音设备 SDK - 语音服务
titleSuffix: Azure Cognitive Services
description: 语音设备 SDK 入门。 “语音服务”适用于多种设备和音频源。 语音设备 SDK 是与特制的麦克风阵列开发工具包配对的一个预优化库。
services: cognitive-services
author: erhopf
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
origin.date: 02/12/2020
ms.date: 03/09/2020
ms.author: v-tawe
ms.openlocfilehash: 10fa9c0f8a48257b11c2e7653a0161c517290a26
ms.sourcegitcommit: ced17aa58e800b9e4335276a1595b8045836b256
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/25/2020
ms.locfileid: "77590264"
---
# <a name="about-the-speech-devices-sdk"></a>关于语音设备 SDK

[语音服务](overview.md)适用于多种设备和音频源。 现在，可以通过匹配的硬件和软件进一步利用语音应用程序。 语音设备 SDK 是与特制的麦克风阵列开发工具包配对的一个预优化库。

语音设备 SDK 有助于：

- 快速测试新的语音方案。
- 更轻松地将基于云的语音服务集成到设备中。
- 为客户创建出色的用户体验。

语音设备 SDK 使用[语音 SDK](speech-sdk.md)。 将我们的高级音频处理算法与设备的麦克风阵列配合使用，将音频发送到[语音服务](overview.md)。 它通过噪声抑制、回声消除、波束赋形和混响消减来提供准确的远场[语音识别](speech-to-text.md)。

<!-- You can also use the Speech Devices SDK to build ambient devices that have your own [customized keyword](speech-devices-sdk-create-kws.md). A Custom Keyword provides a cue that starts a user interaction which is unique to your brand. -->

<!-- The Speech Devices SDK enables a variety of voice-enabled scenarios, drive-thru ordering systems, [conversation transcription](conversation-transcription-service.md), and smart speakers. You can respond to users with text, speak back to them in a default or [custom voice](how-to-custom-voice-create-voice.md), provide search results, and more. We look forward to seeing what you build! -->

## <a name="get-the-speech-devices-sdk"></a>获取语音设备 SDK

### <a name="android"></a>Android

对于 Android，设备将下载最新版本的 [Android 语音设备 SDK](https://aka.ms/sdsdk-download-android)。

### <a name="windows"></a>Windows

对于 Windows，示例应用程序以跨平台 Java 应用程序的形式提供。 下载最新版本的 [JRE 语音设备 SDK](https://aka.ms/sdsdk-download-JRE)。
该应用程序是使用语音 SDK 程序包和 Eclipse Java IDE (v4) 在 64 位 Windows 上构建的。 它在 64 位 Java 8 运行时环境 (JRE) 中运行。

### <a name="linux"></a>Linux

对于 Linux，示例应用程序以跨平台 Java 应用程序的形式提供。 下载最新版本的 [JRE 语音设备 SDK](https://aka.ms/sdsdk-download-JRE)。
该应用程序是使用语音 SDK 程序包和 Eclipse Java IDE (v4) 在 64 位 Linux（Ubuntu 16.04、Ubuntu 18.04、Debian 9）上构建的。 它在 64 位 Java 8 运行时环境 (JRE) 中运行。

提供了其他二进制文件来支持即将推出的设备，例如 [Roobo v2 DDK](https://aka.ms/sdsdk-download-roobov2) 和 [Urbetter DDK](https://aka.ms/sdsdk-download-urbetter)。

## <a name="next-steps"></a>后续步骤

> [!div class="nextstepaction"]
> [选择语音设备](get-speech-devices-sdk.md)
> [!div class="nextstepaction"]
> [获取语音服务订阅密钥以进行试用](get-started.md)
