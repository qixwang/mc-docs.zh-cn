---
title: 快速入门：从麦克风中识别语音，C++ (macOS) - 语音服务
titleSuffix: Azure Cognitive Services
services: cognitive-services
author: wolfma61
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: include
origin.date: 12/17/2019
ms.date: 03/16/2020
ms.author: v-tawe
ms.openlocfilehash: 16c8081b7bc8748f7ed540fb9e7da9c1fad58ea6
ms.sourcegitcommit: b2f2bb08ab1b5ccb3c596d84b3b6ddca5bba3903
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/24/2020
ms.locfileid: "80151635"
---
## <a name="prerequisites"></a>先决条件

准备工作：

> [!div class="checklist"]
> * [创建一个 Azure 搜索资源](../../../../get-started.md)
> * [设置开发环境并创建空项目](../../../../quickstarts/setup-platform.md?tabs=macos)
> * 请确保你有权访问麦克风，以便进行音频捕获

## <a name="add-sample-code"></a>添加示例代码

1. 创建一个名为 `helloworld.cpp` 的 C++ 源文件，并将以下代码粘贴到其中。

   ```cpp
   #include <iostream> // cin, cout
    #include <MicrosoftCognitiveServicesSpeech/speechapi_cxx.h>
    
    using namespace std;
    using namespace Microsoft::CognitiveServices::Speech;
    
    void recognizeSpeech() {
        // Creates an instance of a speech config with specified host and subscription key.
        // Replace with your own subscription key and service region (e.g., "chinaeast2").
        auto config = SpeechConfig::FromHost("wss://YourServiceRegion.stt.speech.azure.cn/", "YourSubscriptionKey");
    
        // Creates a speech recognizer
        auto recognizer = SpeechRecognizer::FromConfig(config);
        cout << "Say something...\n";
    
        // Performs recognition. RecognizeOnceAsync() returns when the first utterance has been recognized,
        // so it is suitable only for single shot recognition like command or query. For long-running
        // recognition, use StartContinuousRecognitionAsync() instead.
        auto result = recognizer->RecognizeOnceAsync().get();
    
        // Checks result.
        if (result->Reason == ResultReason::RecognizedSpeech) {
            cout << "We recognized: " << result->Text << std::endl;
        }
        else if (result->Reason == ResultReason::NoMatch) {
            cout << "NOMATCH: Speech could not be recognized." << std::endl;
        }
        else if (result->Reason == ResultReason::Canceled) {
            auto cancellation = CancellationDetails::FromResult(result);
            cout << "CANCELED: Reason=" << (int)cancellation->Reason << std::endl;
    
            if (cancellation->Reason == CancellationReason::Error) {
                cout << "CANCELED: ErrorCode= " << (int)cancellation->ErrorCode << std::endl;
                cout << "CANCELED: ErrorDetails=" << cancellation->ErrorDetails << std::endl;
                cout << "CANCELED: Did you update the subscription info?" << std::endl;
            }
        }
    }
    
    int main(int argc, char **argv) {
        setlocale(LC_ALL, "");
        recognizeSpeech();
        return 0;
    }
    ```

1. 在此新文件中，将字符串 `YourSubscriptionKey` 替换为你的语音服务订阅密钥。

1. 将字符串 `YourServiceRegion` 替换为与订阅关联的[区域](~/articles/cognitive-services/Speech-Service/regions.md)的“区域标识符”（例如，对于试用版订阅，为 `chinaeast2`）。 

> [!NOTE]
> 语音 SDK 将默认使用 en-us 作为语言进行识别。若要了解如何选择源语言，请参阅[指定语音转文本的源语言](../../../../how-to-specify-source-language.md)。

## <a name="build-the-app"></a>生成应用

> [!NOTE]
> 请确保将以下命令输入在单个命令行上。  执行该操作的最简单方法是使用每个命令旁边的“复制按钮”来复制命令，然后将其粘贴到 shell 提示符下。 

* 运行以下命令以生成应用程序。

  ```sh
  g++ helloworld.cpp -o helloworld --std=c++14 -F${SPEECHSDK_ROOT} -framework MicrosoftCognitiveServicesSpeech
  ```

## <a name="run-the-app"></a>运行应用程序

1. 将加载程序的库路径配置为指向语音 SDK 库。

    ```sh
    export DYLD_FRAMEWORK_PATH="$DYLD_FRAMEWORK_PATH:$SPEECHSDK_ROOT"
    ```

1. 运行应用程序。

   ```sh
   ./helloworld
   ```

1. 在控制台窗口中，会出现一个提示，请求你说点什么。 说一个英语短语或句子。 你的语音将传输到语音服务并转录为文本，该文本将显示在同一窗口中。

   ```text
   Say something...
   We recognized: What's the weather like?
   ```

## <a name="next-steps"></a>后续步骤

[!INCLUDE [footer](./footer.md)]