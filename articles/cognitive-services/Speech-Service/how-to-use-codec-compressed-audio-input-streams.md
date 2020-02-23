---
title: 使用语音 SDK 流式传输编解码器压缩的音频 - 语音服务
titleSuffix: Azure Cognitive Services
description: 了解如何使用语音 SDK 将压缩音频流式传输到语音服务。 适用于用于 Linux 的 C++、C# 和 Java，Android 中的 Java 和 iOS 中的 Objective-C。
services: cognitive-services
author: amitkumarshukla
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
origin.date: 09/20/2019
ms.date: 01/13/2020
ms.author: v-tawe
ms.openlocfilehash: 825298c904f99795fca6f2192fa1da65bb734c9b
ms.sourcegitcommit: ada94ca4685855f58616e4bf1dd5ca757878dfdc
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/18/2020
ms.locfileid: "77428772"
---
# <a name="using-codec-compressed-audio-input-with-the-speech-sdk"></a>在语音 SDK 中使用编解码器压缩的音频输入

语音 SDK 的**压缩音频输入流** API 提供了一种使用 PullStream 或 PushStream 将压缩音频流式传输到语音服务的方法。

> [!IMPORTANT]
> 目前，Linux（Ubuntu 16.04、Ubuntu 18.04、Debian 9）上的 C++、C# 和 Java 支持流式传输压缩输入音频。 [Android 中的 Java](how-to-use-codec-compressed-audio-input-streams-android.md) 和 [iOS 中的 Objective-C](how-to-use-codec-compressed-audio-input-streams-ios.md) 平台也支持该功能。
> 需要语音 SDK 版本 1.7.0 或更高版本。

有关 wav/PCM，请参阅主线语音文档。  在 wav/PCM 之外，支持以下编解码器压缩的输入格式：

- MP3
- OPUS/OGG
- FLAC
- wav 容器中的 ALAW
- wav 容器中的 MULAW

## <a name="prerequisites"></a>必备条件

处理压缩音频是使用 [GStreamer](https://gstreamer.freedesktop.org) 实现的。 由于许可原因，GStreamer 二进制文件没有编译并与语音 SDK 链接。 因此，应用程序开发人员需要在 18.04、16.04 和 Debian 9 上安装以下项才能使用压缩输入音频。

```sh
sudo apt install libgstreamer1.0-0 gstreamer1.0-plugins-base gstreamer1.0-plugins-good gstreamer1.0-plugins-bad gstreamer1.0-plugins-ugly
```

## <a name="example-code-using-codec-compressed-audio-input"></a>使用编解码器压缩的音频输入的示例代码

若要以压缩音频格式流式传输到语音服务，请创建 `PullAudioInputStream` 或 `PushAudioInputStream`。 然后，从流类的实例创建 `AudioConfig`，并指定流的压缩格式。

让我们假设你有一个名为 `myPushStream` 的输入流类，并且使用 OPUS/OGG。 你的代码可能如下所示：

```csharp
using Microsoft.CognitiveServices.Speech;
using Microsoft.CognitiveServices.Speech.Audio;

var speechConfig = SpeechConfig.FromHost(new Uri("wss://YourServiceRegion.stt.speech.azure.cn/"), "YourSubscriptionKey");

// Create an audio config specifying the compressed audio format and the instance of your input stream class.
var audioFormat = AudioStreamFormat.GetCompressedFormat(AudioStreamContainerFormat.OGG_OPUS);
var audioConfig = AudioConfig.FromStreamInput(myPushStream, audioFormat);

var recognizer = new SpeechRecognizer(speechConfig, audioConfig);

var result = await recognizer.RecognizeOnceAsync();

var text = result.GetText();
```

## <a name="next-steps"></a>后续步骤

- [获取语音试用订阅](https://www.azure.cn/home/features/cognitive-services/)
* [查看如何在 Java 中识别语音](~/articles/cognitive-services/Speech-Service/quickstarts/speech-to-text-from-microphone.md?pivots=programming-language-java)
