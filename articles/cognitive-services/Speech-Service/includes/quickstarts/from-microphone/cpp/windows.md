---
author: trevorbye
ms.service: cognitive-services
ms.topic: include
origin.date: 04/03/2019
ms.date: 04/20/2020
ms.author: v-tawe
ms.openlocfilehash: 5cb0bbf74c6ba098819fce0e78e3fa85f63a36b8
ms.sourcegitcommit: f9c242ce5df12e1cd85471adae52530c4de4c7d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/24/2020
ms.locfileid: "82150858"
---
## <a name="prerequisites"></a>先决条件

准备工作：

> [!div class="checklist"]
> * <a href="https://portal.azure.cn/#create/Microsoft.CognitiveServicesSpeechServices" target="_blank">创建 Azure 语音资源<span class="docon docon-navigate-external x-hidden-focus"></span></a>
> * [设置开发环境并创建空项目](../../../../quickstarts/setup-platform.md?tabs=windows&pivots=programming-language-cpp)
> * 请确保你有权访问麦克风，以便进行音频捕获
> * 需要语音 SDK 版本 1.10.0 或更高版本。

## <a name="source-code"></a>源代码

创建一个名为 *helloworld.cpp* 的 C++ 源文件，并将以下代码粘贴到其中。

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

[!INCLUDE [replace key and region](../replace-key-and-region.md)]

## <a name="code-explanation"></a>代码说明

[!INCLUDE [code explanation](../code-explanation.md)]

## <a name="build-and-run-app"></a>生成并运行应用

1. 从菜单栏中，选择“构建”   > “构建解决方案”  以构建应用程序。 现在，编译代码时应不会提示错误。

1. 选择“调试”   > “开始调试”  （或按 F5<kbd></kbd>）以启动 helloworld  应用程序。

1. 说一个英语短语或句子。 应用程序将语音传输到语音服务，该服务转录文本并将其发送回应用程序以供显示。

   ![成功识别后的控制台输出](~/articles/cognitive-services/Speech-Service/media/sdk/qs-cpp-windows-08-console-output-release.png)

## <a name="next-steps"></a>后续步骤

[!INCLUDE [Speech recognition basics](../../speech-to-text-next-steps.md)]