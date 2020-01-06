---
title: 如何指定语音转文本的源语言
titleSuffix: Azure Cognitive Services
description: 有了语音 SDK，就可以在将语音转换为文本时指定源语言。 本文介绍如何使用 FromConfig 和 SourceLanguageConfig 方法让语音服务知道源语言并提供自定义模型目标。
services: cognitive-services
author: susanhu
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
origin.date: 10/26/2019
ms.date: 01/13/2020
ms.author: v-tawe
zone_pivot_groups: programming-languages-set-two
ms.openlocfilehash: a1eb7921e52faadaa778b6f3b634da6869783c21
ms.sourcegitcommit: 6fb55092f9e99cf7b27324c61f5fab7f579c37dc
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/03/2020
ms.locfileid: "75630980"
---
# <a name="specify-source-language-for-speech-to-text"></a>指定语音转文本的源语言

本文介绍如何指定某个传递给语音 SDK 进行语音识别的音频输入的源语言。 另外还提供示例代码，用于指定自定义语音模型以改进识别。

::: zone pivot="programming-language-csharp"

## <a name="how-to-specify-source-language-in-c"></a>如何在 C# 中指定源语言

第一步是创建 `SpeechConfig`：

```csharp
var speechConfig = SpeechConfig.FromSubscription("YourSubscriptionKey", "YourServiceRegion");
```

接下来，使用 `SpeechRecognitionLanguage` 指定音频的源语言：

```csharp
speechConfig.SpeechRecognitionLanguage = "de-DE";
```

如果使用自定义模型进行识别，则可使用 `EndpointId` 指定终结点：

```csharp
speechConfig.EndpointId = "The Endpoint ID for your custom model.";
```

::: zone-end

::: zone pivot="programming-language-cpp"


## <a name="how-to-specify-source-language-in-c"></a>如何在 C++ 中指定源语言

在此示例中，源语言是使用 `FromConfig` 方法作为参数显式提供的。

```C++
auto recognizer = SpeechRecognizer::FromConfig(speechConfig, "de-DE", audioConfig);
```

在此示例中，源语言是使用 `SourceLanguageConfig` 提供的。 然后，在创建 `recognizer` 时，`sourceLanguageConfig` 会作为参数传递给 `FromConfig`。

```C++
auto sourceLanguageConfig = SourceLanguageConfig::FromLanguage("de-DE");
auto recognizer = SpeechRecognizer::FromConfig(speechConfig, sourceLanguageConfig, audioConfig);
```

在此示例中，源语言和自定义终结点是使用 `SourceLanguageConfig` 提供的。 在创建 `recognizer` 时，`sourceLanguageConfig` 作为参数传递给 `FromConfig`。

```C++
auto sourceLanguageConfig = SourceLanguageConfig::FromLanguage("de-DE", "The Endpoint ID for your custom model.");
auto recognizer = SpeechRecognizer::FromConfig(speechConfig, sourceLanguageConfig, audioConfig);
```

>[!Note]
> 在 C++ 和 Java 中，`SetSpeechRecognitionLanguage` 和 `SetEndpointId` 是 `SpeechConfig` 类中弃用的方法。 建议不要使用这些方法，在构造 `SpeechRecognizer` 时不应使用它们。

::: zone-end

::: zone pivot="programming-language-java"

## <a name="how-to-specify-source-language-in-java"></a>如何在 Java 中指定源语言

在此示例中，源语言是在创建新的 `SpeechRecognizer` 时显式提供的。

```Java
SpeechRecognizer recognizer = new SpeechRecognizer(speechConfig, "de-DE", audioConfig);
```

在此示例中，源语言是使用 `SourceLanguageConfig` 提供的。 然后，在创建新的 `SpeechRecognizer` 时，`sourceLanguageConfig` 会作为参数传递。

```Java
SourceLanguageConfig sourceLanguageConfig = SourceLanguageConfig.fromLanguage("de-DE");
SpeechRecognizer recognizer = new SpeechRecognizer(speechConfig, sourceLanguageConfig, audioConfig);
```

在此示例中，源语言和自定义终结点是使用 `SourceLanguageConfig` 提供的。 然后，在创建新的 `SpeechRecognizer` 时，`sourceLanguageConfig` 会作为参数传递。

```Java
SourceLanguageConfig sourceLanguageConfig = SourceLanguageConfig.fromLanguage("de-DE", "The Endpoint ID for your custom model.");
SpeechRecognizer recognizer = new SpeechRecognizer(speechConfig, sourceLanguageConfig, audioConfig);
```

>[!Note]
> 在 C++ 和 Java 中，`setSpeechRecognitionLanguage` 和 `setEndpointId` 是 `SpeechConfig` 类中弃用的方法。 建议不要使用这些方法，在构造 `SpeechRecognizer` 时不应使用它们。

::: zone-end

::: zone pivot="programming-language-python"

## <a name="how-to-specify-source-language-in-python"></a>如何在 Python 中指定源语言

第一步是创建 `speech_config`：

```Python
speech_key, service_region = "YourSubscriptionKey", "YourServiceRegion"
speech_config = speechsdk.SpeechConfig(subscription=speech_key, region=service_region)
```

接下来，使用 `speech_recognition_language` 指定音频的源语言：

```Python
speech_config.speech_recognition_language="de-DE"
```

如果使用自定义模型进行识别，则可使用 `endpoint_id` 指定终结点：

```Python
speech_config.endpoint_id = "The Endpoint ID for your custom model."
```

::: zone-end

::: zone pivot="programming-language-more"

## <a name="how-to-specify-source-language-in-javascript"></a>如何在 Javascript 中指定源语言

第一步是创建 `SpeechConfig`：

```Javascript
var speechConfig = sdk.SpeechConfig.fromSubscription("YourSubscriptionkey", "YourRegion");
```

接下来，使用 `speechRecognitionLanguage` 指定音频的源语言：

```Javascript
speechConfig.speechRecognitionLanguage = "de-DE";
```

如果使用自定义模型进行识别，则可使用 `endpointId` 指定终结点：

```Javascript
speechConfig.endpointId = "The Endpoint ID for your custom model.";
```

## <a name="how-to-specify-source-language-in-objective-c"></a>如何在 Objective-C 中指定源语言

第一步是创建 `speechConfig`：

```Objective-C
SPXSpeechConfiguration *speechConfig = [[SPXSpeechConfiguration alloc] initWithSubscription:@"YourSubscriptionkey" region:@"YourRegion"];
```

接下来，使用 `speechRecognitionLanguage` 指定音频的源语言：

```Objective-C
speechConfig.speechRecognitionLanguage = @"de-DE";
```

如果使用自定义模型进行识别，则可使用 `endpointId` 指定终结点：

```Objective-C
speechConfig.endpointId = @"The Endpoint ID for your custom model.";
```

::: zone-end

## <a name="see-also"></a>另请参阅

* 如需语音转文本支持的语言和区域设置的列表，请参阅[语言支持](language-support.md)。

## <a name="next-steps"></a>后续步骤

* [语音 SDK 参考文档](speech-sdk.md)
