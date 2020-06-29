---
title: 快速入门：将语音翻译为语音，Python - 语音服务
titleSuffix: Azure Cognitive Services
description: TBD
services: cognitive-services
author: chlandsi
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: include
origin.date: 12/09/2019
ms.date: 03/16/2020
ms.author: v-tawe
ms.openlocfilehash: 606fb5891f1f7a6e3c6395220391831be5b1baf9
ms.sourcegitcommit: 304d3ef3c9e65c3e85977b3afb9985fbc0f908d6
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/19/2020
ms.locfileid: "85095966"
---
## <a name="prerequisites"></a>先决条件

在开始之前，请务必：

> [!div class="checklist"]
> * [创建一个 Azure 搜索资源](../../../../get-started.md)
> * [设置开发环境并创建空项目](../../../../quickstarts/setup-platform.md?tabs=python)
> * 需要语音 SDK 1.11.0 或更高版本。

## <a name="add-sample-code"></a>添加示例代码

1. 打开 `quickstart.py` 并将其中的所有代码替换为以下内容。

    ````python
    import azure.cognitiveservices.speech as speechsdk

    speech_key, service_region = "YourSubscriptionKey", "YourServiceRegion"

    def translate_speech_to_speech():

        # Creates an instance of a speech translation config with specified host and subscription key.
        # Replace with your own subscription key and region identifier from here: https://docs.azure.cn/cognitive-services/speech-service/regions
        speech_host, speech_key = "wss://YourServiceRegion.stt.speech.azure.cn/", "YourSubscriptionKey"
        translation_config = speechsdk.translation.SpeechTranslationConfig(host=speech_host, subscription=speech_key)

        # Sets source and target languages.
        # Replace with the languages of your choice, from list found here: https://docs.azure.cn/cognitive-services/speech-service/language-support#speech-translation
        fromLanguage = 'en-US'
        toLanguage = 'de'
        translation_config.speech_recognition_language = fromLanguage
        translation_config.add_target_language(toLanguage)

        # Sets the synthesis output voice name.
        # Replace with the languages of your choice, from list found here: https://docs.azure.cn/cognitive-services/speech-service/language-support#text-to-speech
        translation_config.voice_name = "de-DE-Hedda"

        # Creates a translation recognizer using and audio file as input.
        recognizer = speechsdk.translation.TranslationRecognizer(translation_config=translation_config)

        # Prepare to handle the synthesized audio data.
        def synthesis_callback(evt):
            size = len(evt.result.audio)
            print('AUDIO SYNTHESIZED: {} byte(s) {}'.format(size, '(COMPLETED)' if size == 0 else ''))

        recognizer.synthesizing.connect(synthesis_callback)

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
            print("TRANSLATED into {}: {}".format(toLanguage, result.translations['de']))
        elif result.reason == speechsdk.ResultReason.RecognizedSpeech:
            print("RECOGNIZED: {} (text could not be translated)".format(result.text))
        elif result.reason == speechsdk.ResultReason.NoMatch:
            print("NOMATCH: Speech could not be recognized: {}".format(result.no_match_details))
        elif result.reason == speechsdk.ResultReason.Canceled:
            print("CANCELED: Reason={}".format(result.cancellation_details.reason))
            if result.cancellation_details.reason == speechsdk.CancellationReason.Error:
                print("CANCELED: ErrorDetails={}".format(result.cancellation_details.error_details))

    translate_speech_to_speech()
    ````

1. 在同一文件中，将字符串 `YourSubscriptionKey` 替换为你的订阅密钥。

1. 将字符串 `YourServiceRegion` 替换为与订阅关联的[区域](../../../../regions.md)（例如，对于试用订阅，为 `chinaeast2`）。

1. 保存对 `quickstart.py` 所做的更改。

## <a name="build-and-run-your-app"></a>生成并运行应用

1. 通过控制台或 IDE 运行示例：

   ```
   python quickstart.py
   ```

1. 说一个英语短语或句子。 应用程序将语音传输到语音服务，该服务会将语音翻译并转录为文本（在本例中为德语）。 然后，语音服务会将合成的语音和文本发送回应用程序以供显示。

   ```
   Say something...
   AUDIO SYNTHESIZED: 76784 byte(s)
   AUDIO SYNTHESIZED: 0 byte(s) (COMPLETE)
   RECOGNIZED 'en-US': What's the weather in Seattle?
   TRANSLATED into 'de': Wie ist das Wetter in Seattle?
   ```

## <a name="next-steps"></a>后续步骤

[!INCLUDE [footer](./footer.md)]
