---
title: 快速入门：合成语音，C++ (Windows) - 语音服务
titleSuffix: Azure Cognitive Services
description: 了解如何在 Windows 桌面上使用语音 SDK 通过 C++ 合成语音
services: cognitive-services
author: yinhew
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: include
origin.date: 08/24/2019
ms.date: 03/16/2020
ms.author: v-tawe
ms.openlocfilehash: 89abc82be7fe8a5ec9125f7dc8f196ba87eb9a69
ms.sourcegitcommit: b2f2bb08ab1b5ccb3c596d84b3b6ddca5bba3903
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/24/2020
ms.locfileid: "80151617"
---
## <a name="prerequisites"></a>先决条件

在开始之前，请务必：

> [!div class="checklist"]
> * [创建一个 Azure 搜索资源](../../../../get-started.md)
> * [设置开发环境并创建空项目](../../../../quickstarts/setup-platform.md?tabs=windows)

## <a name="add-sample-code"></a>添加示例代码

1. 打开源文件 **helloworld.cpp**。

1. 将所有代码替换为以下片段：

    ```cpp
    #include <iostream>
    #include <speechapi_cxx.h>
    
    using namespace std;
    using namespace Microsoft::CognitiveServices::Speech;
    
    void synthesizeSpeech()
    {
        // Creates an instance of a speech config with specified host and subscription key.
        // Replace with your own subscription key and service region (e.g., "chinaeast2").
        auto config = SpeechConfig::FromHost("https://YourServiceRegion.tts.speech.azure.cn/", "YourSubscriptionKey");
    
        // Creates a speech synthesizer using the default speaker as audio output. The default spoken language is "en-us".
        auto synthesizer = SpeechSynthesizer::FromConfig(config);
    
        // Receive a text from console input and synthesize it to speaker.
        cout << "Type some text that you want to speak..." << std::endl;
        cout << "> ";
        std::string text;
        getline(cin, text);
    
        auto result = synthesizer->SpeakTextAsync(text).get();
    
        // Checks result.
        if (result->Reason == ResultReason::SynthesizingAudioCompleted)
        {
            cout << "Speech synthesized to speaker for text [" << text << "]" << std::endl;
        }
        else if (result->Reason == ResultReason::Canceled)
        {
            auto cancellation = SpeechSynthesisCancellationDetails::FromResult(result);
            cout << "CANCELED: Reason=" << (int)cancellation->Reason << std::endl;
    
            if (cancellation->Reason == CancellationReason::Error)
            {
                cout << "CANCELED: ErrorCode=" << (int)cancellation->ErrorCode << std::endl;
                cout << "CANCELED: ErrorDetails=[" << cancellation->ErrorDetails << "]" << std::endl;
                cout << "CANCELED: Did you update the subscription info?" << std::endl;
            }
        }
    
        // This is to give some time for the speaker to finish playing back the audio
        cout << "Press enter to exit..." << std::endl;
        cin.get();
    }
    
    int wmain()
    {
        synthesizeSpeech();
        return 0;
    }
    ```

1. 在同一文件中，将字符串 `YourSubscriptionKey` 替换为你的订阅密钥。

1. 将字符串 `YourServiceRegion` 替换为与订阅关联的[区域](~/articles/cognitive-services/Speech-Service/regions.md)（例如，对于试用订阅，为 `chinaeast2`）。

1. 在菜单栏中，选择“文件”   > “全部保存”  。

## <a name="build-and-run-the-application"></a>生成并运行应用程序

1. 从菜单栏中，选择“构建”   > “构建解决方案”  以构建应用程序。 现在，编译代码时应不会提示错误。

1. 选择“调试”   > “开始调试”  （或按 F5  ）以启动 helloworld  应用程序。

1. 键入一个英语短语或句子。 应用程序将你的文本传输到语音服务，该服务会将合成的语音发送到应用程序以在你的扬声器上播放。

   ![成功语音合成后的控制台输出](~/articles/cognitive-services/Speech-Service/media/sdk/qs-tts-cpp-windows-console-output.png)

## <a name="next-steps"></a>后续步骤

[!INCLUDE [footer](./footer.md)]

<!-- ## See also -->

<!-- - [Create a Custom Voice](~/articles/cognitive-services/Speech-Service/how-to-custom-voice-create-voice.md) -->
<!-- - [Record custom voice samples](~/articles/cognitive-services/Speech-Service/record-custom-voice-samples.md) -->
