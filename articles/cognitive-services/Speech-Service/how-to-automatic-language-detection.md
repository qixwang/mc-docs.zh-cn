---
title: 如何使用语音转文本的自动语言检测
titleSuffix: Azure Cognitive Services
description: 语音 SDK 支持语音转文本的自动语言检测。 使用此功能时，系统会将提供的音频与提供的语言列表进行比较，并确定最可能的匹配。 然后，可以使用返回的值选择用于语音转文本的语言模型。
services: cognitive-services
author: susanhu
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
origin.date: 10/26/2019
ms.date: 01/27/2020
ms.author: v-tawe
ms.openlocfilehash: ab7e9e7ec55fb1e44483caad9978449ea5c9766e
ms.sourcegitcommit: 94e1c9621b8f81a7078f1412b3a73281d0a8668b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/16/2020
ms.locfileid: "76123376"
---
# <a name="automatic-language-detection-for-speech-to-text"></a>语音转文本的自动语言检测

自动语言检测用于确定传递给语音 SDK 的音频在与一系列提供的语言进行比较时的最可能匹配。 然后，系统会使用自动语言检测返回的值来选择语音转文本的语言模型，为你提供更准确的听录。 若要查看哪些语言可用，请参阅[语言支持](language-support.md)。

本文介绍如何使用 `AutoDetectSourceLanguageConfig` 来构造 `SpeechRecognizer` 对象并检索检测到的语言。

> [!IMPORTANT]
> 此功能仅适用于 C++ 的语音 SDK 和 Java 的语音 SDK。

## <a name="automatic-language-detection-with-the-speech-sdk"></a>使用语言 SDK 进行自动语言检测

自动语言检测目前存在每次检测仅限两种语言的服务端限制。 在构造 `AudoDetectSourceLanguageConfig` 对象时，请牢记此限制。 在下面的示例中，我们将创建 `AutoDetectSourceLanguageConfig`，然后使用它来构造 `SpeechRecognizer`。

> [!TIP]
> 还可以指定一个自定义模型，供执行语音转文本操作时使用。 有关详细信息，请参阅[使用自定义模型来自动检测语言](#use-a-custom-model-for-automatic-language-detection)。

以下代码片段演示了如何在应用中使用自动语言检测：

```C++
auto autoDetectSourceLanguageConfig = AutoDetectSourceLanguageConfig::FromLanguages({ "en-US", "de-DE" });
auto recognizer = SpeechRecognizer::FromConfig(speechConfig, autoDetectSourceLanguageConfig, audioConfig);
speechRecognitionResult = recognizer->RecognizeOnceAsync().get();
auto autoDetectSourceLanguageResult = AutoDetectSourceLanguageResult::FromResult(speechRecognitionResult);
auto detectedLanguage = autoDetectSourceLanguageResult->Language;
```

```Java
AutoDetectSourceLanguageConfig autoDetectSourceLanguageConfig = AutoDetectSourceLanguageConfig.fromLanguages(Arrays.asList("en-US", "de-DE"));
SpeechRecognizer recognizer = new SpeechRecognizer(speechConfig, autoDetectSourceLanguageConfig, audioConfig);
Future<SpeechRecognitionResult> future = recognizer.recognizeOnceAsync();
SpeechRecognitionResult result = future.get(30, TimeUnit.SECONDS);
AutoDetectSourceLanguageResult autoDetectSourceLanguageResult = AutoDetectSourceLanguageResult.fromResult(result);
String detectedLanguage = autoDetectSourceLanguageResult.getLanguage();

recognizer.close();
speechConfig.close();
autoDetectSourceLanguageConfig.close();
audioConfig.close();
result.close();
```

## <a name="use-a-custom-model-for-automatic-language-detection"></a>使用自定义模型来自动检测语言

除了使用语音服务模型来检测语言，还可以指定自定义模型来增强识别功能。 如果未提供自定义模型，服务会使用默认的语言模型。

以下代码片段演示了如何在调用语音服务时指定自定义模型。 如果检测到的语言为 `en-US`，则使用默认模型。 如果检测到的语言为 `fr-FR`，则使用自定义模型的终结点模型：

```C++
std::vector<std::shared_ptr<SourceLanguageConfig>> sourceLanguageConfigs;
sourceLanguageConfigs.push_back(SourceLanguageConfig::FromLanguage("en-US"));
sourceLanguageConfigs.push_back(SourceLanguageConfig::FromLanguage("fr-FR", "The Endpoint Id for custom model of fr-FR"));
auto autoDetectSourceLanguageConfig = AutoDetectSourceLanguageConfig::FromSourceLanguageConfigs(sourceLanguageConfigs);
```

```Java
List sourceLanguageConfigs = new ArrayList<SourceLanguageConfig>();
sourceLanguageConfigs.add(SourceLanguageConfig.fromLanguage("en-US"));
sourceLanguageConfigs.add(SourceLanguageConfig.fromLanguage("fr-FR", "The Endpoint Id for custom model of fr-FR"));
AutoDetectSourceLanguageConfig autoDetectSourceLanguageConfig = AutoDetectSourceLanguageConfig.fromSourceLanguageConfigs(sourceLanguageConfigs);
```

## <a name="next-steps"></a>后续步骤

- [语音 SDK 参考文档](speech-sdk.md)
