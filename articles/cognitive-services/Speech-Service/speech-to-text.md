---
title: 语音转文本 - 语音服务
titleSuffix: Azure Cognitive Services
description: 使用语音转文本功能，可将音频流实时听录为文本。 应用程序、工具或设备可以使用、显示和处理此文本输入。
services: cognitive-services
author: erhopf
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
origin.date: 12/11/2019
ms.date: 01/13/2020
ms.author: v-tawe
ms.openlocfilehash: 50981d3032932726e480a79713ae7073551e388f
ms.sourcegitcommit: 6fb55092f9e99cf7b27324c61f5fab7f579c37dc
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/03/2020
ms.locfileid: "75630986"
---
# <a name="what-is-speech-to-text"></a>什么是语音转文本？

使用语音服务提供的语音转文本（简称语音转文本）功能，可将音频流实时听录为文本。 应用程序、工具或设备可以使用、显示和处理此文本即命令输入。 此服务由 Microsoft 对 Cortana 和 Office 产品使用的同一识别技术提供支持。  有关可用语音转文本语言的完整列表，请参阅[支持的语言](language-support.md#speech-to-text)。

语音转文本服务默认使用通用语言模型。 此模型已使用 Microsoft 自有的数据训练，部署在云中。 它非常适合用于对话和听写方案。 使用语音转文本在独特的环境中进行识别和听录时，可以创建并训练自定义的声学、语言和发音模型。 自定义有助于解决环境干扰或特定于行业的词汇的问题。

> [!NOTE]
> 如果你的应用程序、工具或产品使用自定义语音，则可参阅我们创建的指南来迁移到语音服务。
> - [从自定义语音迁移到语音服务](how-to-migrate-from-custom-speech-service.md)

## <a name="get-started-with-speech-to-text"></a>语音转文本入门

语音转文本服务通过[语音 SDK](speech-sdk.md) 提供。

<!--  - [Quickstart: Recognize speech with microphone input](quickstarts/speech-to-text-from-microphone.md) -->
<!--  - [Quickstart: Recognize speech from a file](quickstarts/speech-to-text-from-file.md) -->
<!--  - [Quickstart: Recognize speech stored in blob storage](quickstarts/from-blob.md) -->

如果你偏向于使用语音转文本 REST 服务，请参阅 [REST API](rest-speech-to-text.md)。

## <a name="tutorials-and-sample-code"></a>教程和示例代码

<!-- After you've had a chance to use the Speech Services, try our tutorial that teaches you how to recognize intents from speech using the Speech SDK and LUIS. -->

<!-- - [Tutorial: Recognize intents from speech with the Speech SDK and LUIS, C#](how-to-recognize-intents-from-speech-csharp.md) -->

GitHub 上提供了语音 SDK 的示例代码。 这些示例涵盖了常见方案，例如，从文件或流中读取音频、连续和单次识别，以及使用自定义模型。

- [语音转文本示例 (SDK)](https://github.com/Azure-Samples/cognitive-services-speech-sdk)
- [批量听录示例 (REST)](https://github.com/Azure-Samples/cognitive-services-speech-sdk/tree/master/samples/batch)

## <a name="customization"></a>自定义

除了标准语音服务模型外，还可以创建自定义模型。 自定义有助于克服语音识别障碍，如说话风格、词汇和背景噪音，详见[自定义语音](how-to-custom-speech.md)。 自定义选项因语言/区域设置而异，请参阅[支持的语言](supported-languages.md)以验证相关支持。

[!INCLUDE [speech-reference-doc-links](includes/speech-reference-doc-links.md)]

## <a name="next-steps"></a>后续步骤

- [获取语音服务订阅密钥以进行试用](get-started.md)
- [获取语音 SDK](speech-sdk.md)
