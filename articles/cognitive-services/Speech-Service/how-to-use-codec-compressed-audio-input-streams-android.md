---
title: 在 Android 上使用语音 SDK 流式传输编解码器压缩的音频
titleSuffix: Azure Cognitive Services
description: 了解如何在 Android 上使用语音 SDK 将压缩音频流式传输到语音服务。
services: cognitive-services
author: amitkumarshukla
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
origin.date: 09/20/2019
ms.date: 01/13/2020
ms.author: v-tawe
ms.openlocfilehash: 8d3cf689a87780e8f8fc8499f20996aa997abede
ms.sourcegitcommit: 6fb55092f9e99cf7b27324c61f5fab7f579c37dc
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/03/2020
ms.locfileid: "75630902"
---
# <a name="how-to-use-codec-compressed-audio-input-with-the-speech-sdk-on-android"></a>如何：在 Android 上的语音 SDK 中使用编解码器压缩的音频输入

语音 SDK 的**压缩音频输入流** API 提供了一种使用 PullStream 或 PushStream 将压缩音频流式传输到语音服务的方法。

> [!IMPORTANT]
> 目前，[Linux（Ubuntu 16.04、Ubuntu 18.04、Debian 9）上的 C++、C# 和 Java](how-to-use-codec-compressed-audio-input-streams.md) 支持流式传输压缩输入音频。 Android 中的 Java 和 [iOS 中的 Objective-C](how-to-use-codec-compressed-audio-input-streams-ios.md) 平台也支持该功能。
> 需要语音 SDK 版本 1.7.0 或更高版本。

有关 wav/PCM，请参阅主线语音文档。 在 wav/PCM 之外，支持以下编解码器压缩的输入格式：

- MP3
- OPUS/OGG
- FLAC
- wav 容器中的 ALAW
- wav 容器中的 MULAW

## <a name="prerequisites-to-using-codec-compressed-audio-input-on-android"></a>在 Android 上使用编解码器压缩的音频输入的先决条件

编解码器压缩的音频是使用 [GStreamer](https://gstreamer.freedesktop.org) 实现的。 出于许可原因，Gstreamer 二进制文件未使用 SDK 进行编译。 需要使用适用于 Android 的预生成二进制文件。 若要下载预生成库，请参阅[为 Android 开发安装](https://gstreamer.freedesktop.org/documentation/installing/for-android-development.html?gi-language=c)。

需要 `libgstreamer_android.so`。 请确保 GStreamer 插件已在 `libgstreamer_android.so` 中链接。

```make
GSTREAMER_PLUGINS := coreelements app audioconvert mpg123 audioresample audioparsers ogg opusparse opus wavparse alaw mulaw flac
```

下面提供了一个示例 `Android.mk` 和 `Application.mk` 文件。 按照以下步骤创建 gstreamer 共享对象：`libgstreamer_android.so`。

```make
# Android.mk
LOCAL_PATH := $(call my-dir)

include $(CLEAR_VARS)

LOCAL_MODULE    := dummy
LOCAL_SHARED_LIBRARIES := gstreamer_android
LOCAL_LDLIBS := -llog
include $(BUILD_SHARED_LIBRARY)


ifndef GSTREAMER_ROOT_ANDROID
$(error GSTREAMER_ROOT_ANDROID is not defined!)
endif

ifndef APP_BUILD_SCRIPT
$(error APP_BUILD_SCRIPT is not defined!)
endif

ifndef TARGET_ARCH_ABI
$(error TARGET_ARCH_ABI is not defined!)
endif

ifeq ($(TARGET_ARCH_ABI),armeabi)
GSTREAMER_ROOT        := $(GSTREAMER_ROOT_ANDROID)/arm
else ifeq ($(TARGET_ARCH_ABI),armeabi-v7a)
GSTREAMER_ROOT        := $(GSTREAMER_ROOT_ANDROID)/armv7
else ifeq ($(TARGET_ARCH_ABI),arm64-v8a)
GSTREAMER_ROOT        := $(GSTREAMER_ROOT_ANDROID)/arm64
else ifeq ($(TARGET_ARCH_ABI),x86)
GSTREAMER_ROOT        := $(GSTREAMER_ROOT_ANDROID)/x86
else ifeq ($(TARGET_ARCH_ABI),x86_64)
GSTREAMER_ROOT        := $(GSTREAMER_ROOT_ANDROID)/x86_64
else
$(error Target arch ABI not supported: $(TARGET_ARCH_ABI))
endif

GSTREAMER_NDK_BUILD_PATH  := $(GSTREAMER_ROOT)/share/gst-android/ndk-build/
include $(GSTREAMER_NDK_BUILD_PATH)/plugins.mk
GSTREAMER_PLUGINS         :=  coreelements app audioconvert mpg123 audioresample audioparsers ogg opusparse opus wavparse alaw mulaw flac
GSTREAMER_EXTRA_LIBS      := -liconv
include $(GSTREAMER_NDK_BUILD_PATH)/gstreamer-1.0.mk
```

```make
# Application.mk
APP_STL = c++_shared
APP_PLATFORM = android-21
APP_BUILD_SCRIPT = Android.mk
```

可以在 Ubuntu 16.04 或 18.04 上使用以下命令生成 `libgstreamer_android.so`。 以下命令行仅针对使用 [Android NDK b16b](https://dl.google.com/android/repository/android-ndk-r16b-linux-x86_64.zip) 的 [Gstreamer Android 版本1.14.4](https://gstreamer.freedesktop.org/data/pkg/android/1.14.4/gstreamer-1.0-android-universal-1.14.4.tar.bz2) 进行了测试。

```sh
# assuming wget and unzip already installed on the system
mkdir buildLibGstreamer
cd buildLibGstreamer
wget https://dl.google.com/android/repository/android-ndk-r16b-linux-x86_64.zip
unzip -q -o android-ndk-r16b-linux-x86_64.zip
export PATH=$PATH:$(pwd)/android-ndk-r16b
export NDK_PROJECT_PATH=$(pwd)/android-ndk-r16b
wget https://gstreamer.freedesktop.org/data/pkg/android/1.14.4/gstreamer-1.0-android-universal-1.14.4.tar.bz2
mkdir gstreamer_android
tar -xjf gstreamer-1.0-android-universal-1.14.4.tar.bz2 -C $(pwd)/gstreamer_android/
export GSTREAMER_ROOT_ANDROID=$(pwd)/gstreamer_android

mkdir gstreamer
# Copy the Application.mk and Android.mk from the documentation above and put it inside $(pwd)/gstreamer

# Enable only one of the following at one time to create the shared object for the targeted ABI
echo "building for armeabi-v7a. libgstreamer_android.so will be placed in $(pwd)/armeabi-v7a"
ndk-build -C $(pwd)/gstreamer "NDK_APPLICATION_MK=Application.mk" APP_ABI=armeabi-v7a NDK_LIBS_OUT=$(pwd)

#echo "building for arm64-v8a. libgstreamer_android.so will be placed in $(pwd)/arm64-v8a"
#ndk-build -C $(pwd)/gstreamer "NDK_APPLICATION_MK=Application.mk" APP_ABI=arm64-v8a NDK_LIBS_OUT=$(pwd)

#echo "building for x86_64. libgstreamer_android.so will be placed in $(pwd)/x86_64"
#ndk-build -C $(pwd)/gstreamer "NDK_APPLICATION_MK=Application.mk" APP_ABI=x86_64 NDK_LIBS_OUT=$(pwd)

#echo "building for x86. libgstreamer_android.so will be placed in $(pwd)/x86"
#ndk-build -C $(pwd)/gstreamer "NDK_APPLICATION_MK=Application.mk" APP_ABI=x86 NDK_LIBS_OUT=$(pwd)
```

生成共享对象 (libgstreamer_android.so) 后，应用程序开发人员需要将该共享对象放置在 Android 应用中，以便可以通过语音 SDK 加载该对象。

## <a name="example-code-using-codec-compressed-audio-input"></a>使用编解码器压缩的音频输入的示例代码

若要以压缩音频格式流式传输到语音服务，请创建 `PullAudioInputStream` 或 `PushAudioInputStream`。 然后，从流类的实例创建 `AudioConfig`，并指定流的压缩格式。

让我们假设你有一个名为 `myPullStream` 的输入流类，并且使用 OPUS/OGG。 你的代码可能如下所示：

```java
import com.microsoft.cognitiveservices.speech.audio.AudioConfig;
import com.microsoft.cognitiveservices.speech.audio.AudioInputStream;
import com.microsoft.cognitiveservices.speech.audio.AudioStreamFormat;
import com.microsoft.cognitiveservices.speech.audio.PullAudioInputStream;
import com.microsoft.cognitiveservices.speech.internal.AudioStreamContainerFormat;

SpeechConfig speechConfig = SpeechConfig.fromSubscription("YourSubscriptionKey", "YourServiceRegion");

// Create an audio config specifying the compressed audio format and the instance of your input stream class.
AudioStreamFormat audioFormat = AudioStreamFormat.getCompressedFormat(AudioStreamContainerFormat.OGG_OPUS);
AudioConfig audioConfig = AudioConfig.fromStreamInput(myPullStream, audioFormat);

SpeechRecognizer recognizer = new SpeechRecognizer(speechConfig, audioConfig);

SpeechRecognitionResult result = recognizer.recognizeOnceAsync().get()

String text = result.getText();
```

## <a name="next-steps"></a>后续步骤

- [获取语音试用订阅](https://www.azure.cn/pricing/1rmb-trial/)
- [查看如何在 Java 中识别语音](~/articles/cognitive-services/Speech-Service/quickstarts/speech-to-text-from-microphone.md?pivots=programming-language-java)
