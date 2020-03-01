---
title: 快速入门：识别语音，Python - 语音服务
titleSuffix: Azure Cognitive Services
description: TBD
services: cognitive-services
author: chlandsi
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: include
origin.date: 12/09/2019
ms.date: 03/09/2020
ms.author: v-tawe
ms.openlocfilehash: 17a3c7bb85100904cd9b5644c99346f227520b4d
ms.sourcegitcommit: ced17aa58e800b9e4335276a1595b8045836b256
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/25/2020
ms.locfileid: "77590209"
---
## <a name="prerequisites"></a>先决条件

在开始之前，请务必：

> [!div class="checklist"]
> * [创建一个 Azure 搜索资源](../../../../get-started.md)
> * [设置开发环境](../../../../quickstarts/setup-platform.md?tabs=python)
> * [创建空示例项目](../../../../quickstarts/create-project.md?tabs=python)

## <a name="add-sample-code"></a>添加示例代码

1. 打开 `quickstart.py` 并将其中的所有代码替换为以下内容。

    ````python
    import azure.cognitiveservices.speech as speechsdk

    speech_key, service_region = "YourSubscriptionKey", "YourServiceRegion"

    def translate_speech_to_text():

        # Creates an instance of a speech translation config with specified host and subscription key.
        # Replace with your own subscription key and service region (e.g., "chinaeast2", use the one of SpeechSDKParameters
        # from here: https://docs.azure.cn/cognitive-services/speech-service/regions).
        speech_host, speech_key = "wss://YourServiceRegion.stt.speech.azure.cn/", "YourSubscriptionKey"
        translation_config = speechsdk.translation.SpeechTranslationConfig(host=speech_host, subscription=speech_key)

        # Sets source and target languages.
        # Replace with the languages of your choice, from list found here: https://docs.azure.cn/cognitive-services/speech-service/language-support#speech-translation
        fromLanguage = 'en-US'
        translation_config.speech_recognition_language = fromLanguage
        translation_config.add_target_language('de')
        translation_config.add_target_language('fr')

        # Creates a translation recognizer using and audio file as input.
        recognizer = speechsdk.translation.TranslationRecognizer(translation_config=translation_config)

        # Starts translation, and returns after a single utterance is recognized. The end of a
        # single utterance is determined by listening for silence at the end or until a maximum of 15
        # seconds of audio is processed. It returns the recognized text as well as the translation.
        # Note: Since recognize_once() returns only a single utterance, it is suitable only for single
        # shot recognition like command or query.
        # For long-running multi-utterance recognition, use start_continuous_recognition() instead.
        print("Say something...")
        result = recognizer.recognize_once()

        # Check the result
        if result.reason == speechsdk.ResultReason.TranslatedSpeech:
            print("RECOGNIZED '{}': {}".format(fromLanguage, result.text))
            print("TRANSLATED into {}: {}".format('de', result.translations['de']))
            print("TRANSLATED into {}: {}".format('fr', result.translations['fr']))
        elif result.reason == speechsdk.ResultReason.RecognizedSpeech:
            print("RECOGNIZED: {} (text could not be translated)".format(result.text))
        elif result.reason == speechsdk.ResultReason.NoMatch:
            print("NOMATCH: Speech could not be recognized: {}".format(result.no_match_details))
        elif result.reason == speechsdk.ResultReason.Canceled:
            print("CANCELED: Reason={}".format(result.cancellation_details.reason))
            if result.cancellation_details.reason == speechsdk.CancellationReason.Error:
                print("CANCELED: ErrorDetails={}".format(result.cancellation_details.error_details))

    translate_speech_to_text()
    ````

1. 在同一文件中，将字符串 `YourSubscriptionKey` 替换为你的订阅密钥。

1. 将字符串 `YourServiceRegion` 替换为与订阅关联的[区域](../../../../regions.md)（例如，对于试用订阅，为 `chinaeast2`）。

1. 保存对 `quickstart.py` 所做的更改。

## <a name="build-and-run-your-app"></a>生成并运行应用

1. 通过控制台或 IDE 运行示例：

   ```
   python quickstart.py
   ```

1. 说一个英语短语或句子。 应用程序将语音传输到语音服务，该服务会将语音翻译并转录为文本（在本例中为法语和德语）。 然后，语音服务会将该文本发送回应用程序以供显示。

   ```
   Say something...
   RECOGNIZED 'en-US': What's the weather in Seattle?
   TRANSLATED into 'de': Wie ist das Wetter in Seattle?
   TRANSLATED into 'fr': Quel temps fait-il à Seattle ?
   ```

## <a name="next-steps"></a>后续步骤

[!INCLUDE [footer](./footer.md)]
