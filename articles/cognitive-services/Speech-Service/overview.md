---
title: 什么是语音服务？
titleSuffix: Azure Cognitive Services
description: 语音服务在单个 Azure 订阅中统合了语音转文本、文本转语音以及语音翻译功能。 使用语音 SDK、语音设备 SDK 或 REST API 在应用程序、工具和设备中添加语音。
services: cognitive-services
author: trevorbye
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: overview
origin.date: 03/12/2020
ms.date: 04/20/2020
ms.author: v-tawe
ms.openlocfilehash: 7f8c40a1bcd517c46b9b4392caf35ddcc68c974a
ms.sourcegitcommit: 304d3ef3c9e65c3e85977b3afb9985fbc0f908d6
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/19/2020
ms.locfileid: "85095889"
---
# <a name="what-is-the-speech-service"></a>什么是语音服务？

语音服务在单个 Azure 订阅中统合了语音转文本、文本转语音以及语音翻译功能。 使用[语音 SDK](speech-sdk-reference.md)、[语音设备 SDK](speech-devices-sdk-android-quickstart.md) 或 [REST API](rest-apis.md) 可以轻松在应用程序、工具和设备中启用语音。

<!-- > [!IMPORTANT] -->
<!-- > The Speech service has replaced Bing Speech API and Translator Speech. See _How-to guides > Migration_ for migration instructions. -->

这些功能构成了语音服务。 请使用下表中的链接详细了解每项功能的常见用例或浏览 API 参考信息。

| 服务 | 功能 | 说明 | SDK | REST |
|---------|---------|-------------|-----|------|
| [语音转文本](speech-to-text.md) | 实时语音转文本 | 语音转文本可将音频流或本地文件实时转录或翻译为文本，应用程序、工具或设备可以使用或显示这些文本。 结合[语言理解 (LUIS)](https://docs.azure.cn/cognitive-services/luis/) 使用语音转文本可以从听录的语音中派生用户意向，以及处理语音命令。 | [是](https://docs.azure.cn/cognitive-services/speech-service/speech-sdk-reference) | [是](https://docs.azure.cn/cognitive-services/speech-service/rest-apis) |
| | [批量语音转文本](batch-transcription.md) | 批量语音转文本支持对 Azure Blob 存储中存储的大量语音音频数据进行异步语音到文本转录。 除了将语音音频转换为文本，批量语音转文本还允许进行分割聚类和情感分析。 | 否 | [是](https://chinaeast2.cris.azure.cn/swagger/ui/index) |
| | [创建自定义语音识别模型](#customize-your-speech-experience) | 如果使用语音转文本在独特的环境中进行识别和听录，则可以创建并训练自定义的声学、语言和发音模型，以解决环境干扰或行业特定的词汇。 | 否 | [是](https://chinaeast2.cris.azure.cn/swagger/ui/index) |
| [文本转语音](text-to-speech.md) | 文本转语音 | 文本转语音可使用[语音合成标记语言 (SSML)](speech-synthesis-markup.md) 将输入文本转换为类似人类的合成语音。 可以选择标准语音或神经语音（请参阅[语言支持](language-support.md)）。 | [是](https://docs.azure.cn/cognitive-services/speech-service/speech-sdk-reference) | [是](https://docs.azure.cn/cognitive-services/speech-service/rest-apis) |
| [语音翻译](speech-translation.md) | 语音翻译 | 使用语音翻译可在应用程序、工具和设备中实现实时的多语言语音翻译。 进行语音转语音和语音转文本翻译时可以使用此服务。 | [是](https://docs.azure.cn/cognitive-services/speech-service/speech-sdk-reference) | 否 |


[!INCLUDE [TLS 1.2 enforcement](../../../includes/cognitive-services-tls-announcement.md)]

## <a name="try-the-speech-service"></a>试用语音服务

我们提供了适用于大多数流行编程语言的快速入门，旨在帮助你在 10 分钟以内运行代码。 下表包含有关每项功能在最流行编程语言中的用法的快速入门。 使用左侧的导航栏可以浏览其他语言和平台。

| 语音转文本 (SDK) | 文本转语音 (SDK) | 翻译 (SDK) |
|----------------------|----------------------|-------------------|
| [识别来自音频文件的语音](quickstarts/speech-to-text-from-file.md) | [将语音合成为音频文件](quickstarts/text-to-speech-audio-file.md) | [将语音转换为文本](quickstarts/translate-speech-to-text.md) |
| [使用麦克风识别语音](quickstarts/speech-to-text-from-microphone.md) | [将语音合成到扬声器](quickstarts/text-to-speech.md) | [将语音翻译为多种目标语言](quickstarts/translate-speech-to-text-multiple-languages.md) |
| [识别存储在 Blob 存储中的语音](quickstarts/from-blob.md) | | [将语音转换为语音](quickstarts/translate-speech-to-speech.md) |

> [!NOTE]
> “语音转文本”和“文本转语音”功能也有 REST 终结点和相关联的快速入门。

<!-- After you've had a chance to use the Speech service, try our tutorial that teaches you how to recognize intents from speech using the Speech SDK and LUIS. -->

<!-- - [Tutorial: Build a Flask app to translate text, analyze sentiment, and synthesize translated text to speech, REST](/cognitive-services/translator/tutorial-build-flask-app-translation-synthesis) -->

## <a name="get-sample-code"></a>获取示例代码

> [!IMPORTANT]
> 需要语音 SDK 版本 1.11.0 或更高版本。

GitHub 上提供了语音服务的示例代码。 这些示例涵盖了常见方案，例如，从文件或流中读取音频、连续和单次识别，以及使用自定义模型。 使用以下链接查看 SDK 和 REST 示例：

- [语音转文本、文本转语音和语音翻译示例 (SDK)](https://github.com/Azure-Samples/cognitive-services-speech-sdk)
- [批量听录示例 (REST)](https://github.com/Azure-Samples/cognitive-services-speech-sdk/tree/master/samples/batch)
- [文本转语音示例 (REST)](https://github.com/Azure-Samples/Cognitive-Speech-TTS)

## <a name="customize-your-speech-experience"></a>自定义语音体验

语音服务能够很好地与内置模型配合工作，但是，你可能想要根据自己的产品或环境，进一步自定义和优化体验。 自定义选项的范围从声学模型优化，到专属于自有品牌的语音字体。

| 语音服务 | 平台 | 说明 |
| -------------- | -------- | ----------- |
| 语音转文本 | [自定义语音识别](https://speech.azure.cn/customspeech) | 根据需要和可用数据自定义语音识别模型。 克服语音识别障碍，如说话风格、词汇和背景噪音。 |

## <a name="reference-docs"></a>参考文档

- [语音 SDK](speech-sdk-reference.md)
- [语音设备 SDK](speech-devices-sdk.md)
- [REST API：语音转文本](rest-speech-to-text.md)
- [REST API：文本转语音](rest-text-to-speech.md)
- [REST API：批量听录和自定义](https://chinaeast2.cris.azure.cn/swagger/ui/index)

## <a name="next-steps"></a>后续步骤

> [!div class="nextstepaction"]
> [获取语音服务订阅密钥以进行试用](get-started.md)
