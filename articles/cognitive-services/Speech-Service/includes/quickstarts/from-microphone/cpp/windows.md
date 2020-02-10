---
title: 快速入门：从麦克风中识别语音，C++ (Windows) - 语音服务
titleSuffix: Azure Cognitive Services
description: 了解如何在 Windows 桌面上使用语音 SDK 通过 C++ 识别语音
services: cognitive-services
author: wolfma61
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: include
origin.date: 12/17/2019
ms.date: 02/17/2020
ms.author: v-tawe
ms.openlocfilehash: 2c45eac8cf8e8057b5f0c9b88d73867281da8fee
ms.sourcegitcommit: 888cbc10f2348de401d4839a732586cf266883bf
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/05/2020
ms.locfileid: "77029280"
---
## <a name="prerequisites"></a>必备条件

准备工作：

> [!div class="checklist"]
> * [创建一个 Azure 搜索资源](../../../../get-started.md)
> * [设置开发环境](../../../../quickstarts/setup-platform.md?tabs=windows)
> * [创建空示例项目](../../../../quickstarts/create-project.md?tabs=windows)
> * 请确保你有权访问麦克风，以便进行音频捕获

## <a name="add-sample-code"></a>添加示例代码

1. 打开源文件 **helloworld.cpp**。

1. 将所有代码替换为以下片段：

    ```cpp
    #include <iostream>
    #include <speechapi_cxx.h>
    
    using namespace std;
    using namespace Microsoft::CognitiveServices::Speech;
    
    void recognizeSpeech()
    {
        // Creates an instance of a speech config with specified subscription key and service region.
        // Replace with your own subscription key and service region (e.g., "chinaeast2").
        auto config = SpeechConfig::FromSubscription("YourSubscriptionKey", "YourServiceRegion");
    
        // Creates a speech recognizer.
        auto recognizer = SpeechRecognizer::FromConfig(config);
        cout << "Say something...\n";
    
        // Starts speech recognition, and returns after a single utterance is recognized. The end of a
        // single utterance is determined by listening for silence at the end or until a maximum of 15
        // seconds of audio is processed.  The task returns the recognition text as result. 
        // Note: Since RecognizeOnceAsync() returns only a single utterance, it is suitable only for single
        // shot recognition like command or query. 
        // For long-running multi-utterance recognition, use StartContinuousRecognitionAsync() instead.
        auto result = recognizer->RecognizeOnceAsync().get();
    
        // Checks result.
        if (result->Reason == ResultReason::RecognizedSpeech)
        {
            cout << "We recognized: " << result->Text << std::endl;
        }
        else if (result->Reason == ResultReason::NoMatch)
        {
            cout << "NOMATCH: Speech could not be recognized." << std::endl;
        }
        else if (result->Reason == ResultReason::Canceled)
        {
            auto cancellation = CancellationDetails::FromResult(result);
            cout << "CANCELED: Reason=" << (int)cancellation->Reason << std::endl;
    
            if (cancellation->Reason == CancellationReason::Error) 
            {
                cout << "CANCELED: ErrorCode= " << (int)cancellation->ErrorCode << std::endl;
                cout << "CANCELED: ErrorDetails=" << cancellation->ErrorDetails << std::endl;
                cout << "CANCELED: Did you update the subscription info?" << std::endl;
            }
        }
    }
    
    int wmain()
    {
        recognizeSpeech();
        cout << "Please press a key to continue.\n";
        cin.get();
        return 0;
    }
    ```

1. 在同一文件中，将字符串 `YourSubscriptionKey` 替换为你的订阅密钥。

1. 将字符串 `YourServiceRegion` 替换为与订阅关联的[区域](~/articles/cognitive-services/Speech-Service/regions.md)（例如，对于试用订阅，为 `chinaeast2`）。

1. 在菜单栏中，选择“文件”   > “全部保存”  。

> [!NOTE]
> 语音 SDK 将默认使用 en-us 作为语言进行识别。若要了解如何选择源语言，请参阅[指定语音转文本的源语言](../../../../how-to-specify-source-language.md)。

## <a name="build-and-run-the-application"></a>生成并运行应用程序

1. 从菜单栏中，选择“构建”   > “构建解决方案”  以构建应用程序。 现在，编译代码时应不会提示错误。

1. 选择“调试”   > “开始调试”  （或按 F5  ）以启动 helloworld  应用程序。

1. 说一个英语短语或句子。 应用程序将语音传输到语音服务，该服务转录文本并将其发送回应用程序以供显示。

   ![成功识别后的控制台输出](~/articles/cognitive-services/Speech-Service/media/sdk/qs-cpp-windows-08-console-output-release.png)

## <a name="next-steps"></a>后续步骤

[!INCLUDE [footer](./footer.md)]