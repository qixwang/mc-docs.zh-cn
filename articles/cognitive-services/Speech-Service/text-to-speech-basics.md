---
title: 语音合成基础知识 - 语音服务
titleSuffix: Azure Cognitive Services
description: 了解如何使用语音 SDK 将文本转换为语音。 本文介绍有关语音合成的对象构造、支持的音频输出格式以及自定义配置选项。
services: cognitive-services
author: trevorbye
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: quickstart
origin.date: 04/14/2020
ms.date: 04/20/2020
ms.author: v-tawe
zone_pivot_groups: programming-languages-set-sixteen
ms.openlocfilehash: 597c16ead5d7d7bc40d86f8e301bded6112d2d69
ms.sourcegitcommit: a4a2521da9b29714aa6b511fc6ba48279b5777c8
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/24/2020
ms.locfileid: "82127223"
---
# <a name="learn-the-basics-of-speech-synthesis"></a>了解语音合成的基础知识

本文介绍使用语音 SDK 进行文本到语音合成的常见设计模式。 首先，请进行基本的配置和合成，然后通过更高级的示例来了解自定义应用程序开发，其中包括：

* 获取内存中流形式的响应
* 自定义输出采样率和比特率
* 使用 SSML（语音合成标记语言）提交合成请求
* 使用神经语音

> [!TIP]
> 如果你没有机会完成我们的快速入门之一，我们建议你进行尝试，自行试用语音识别。
> * [识别来自麦克风的语音](quickstarts/text-to-speech.md)

::: zone pivot="programming-language-csharp"
[!INCLUDE [C# Basics include](includes/how-to/text-to-speech-basics/text-to-speech-basics-csharp.md)]
::: zone-end

::: zone pivot="programming-language-cpp"
[!INCLUDE [C++ Basics include](includes/how-to/text-to-speech-basics/text-to-speech-basics-cpp.md)]
::: zone-end

::: zone pivot="programming-language-java"
[!INCLUDE [Java Basics include](includes/how-to/text-to-speech-basics/text-to-speech-basics-java.md)]
::: zone-end

::: zone pivot="programming-language-javascript"
[!INCLUDE [JavaScript Basics include](includes/how-to/text-to-speech-basics/text-to-speech-basics-javascript.md)]
::: zone-end

::: zone pivot="programming-language-python"
[!INCLUDE [Python Basics include](includes/how-to/text-to-speech-basics/text-to-speech-basics-python.md)]
::: zone-end

::: zone pivot="programming-language-more"
[!INCLUDE [More languages include](./includes/how-to/speech-to-text-basics/more.md)]
::: zone-end

## <a name="next-steps"></a>后续步骤

<!-- * [Get started with Custom Voice](how-to-custom-voice.md) -->

* [通过 SSML 改进合成](speech-synthesis-markup.md)