---
author: trevorbye
ms.service: cognitive-services
ms.topic: include
origin.date: 04/03/2019
ms.date: 04/20/2020
ms.author: v-tawe
ms.openlocfilehash: 1acfb349089b6aff99d74ae7943969a716f3f895
ms.sourcegitcommit: 304d3ef3c9e65c3e85977b3afb9985fbc0f908d6
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/19/2020
ms.locfileid: "85095906"
---
## <a name="prerequisites"></a>先决条件

准备工作：

> [!div class="checklist"]
> * <a href="https://portal.azure.cn/#create/Microsoft.CognitiveServicesSpeechServices" target="_blank">创建 Azure 语音资源<span class="docon docon-navigate-external x-hidden-focus"></span></a>
> * [设置开发环境并创建空项目](../../../../quickstarts/setup-platform.md?tabs=macos&pivots=programming-language-cpp)
> * 请确保你有权访问麦克风，以便进行音频捕获
> * 需要语音 SDK 1.11.0 或更高版本。

## <a name="source-code"></a>源代码

创建一个名为 *helloworld.cpp* 的 C++ 源文件，并将以下代码粘贴到其中。

   ```cpp
   #include <iostream> // cin, cout
    #include <MicrosoftCognitiveServicesSpeech/speechapi_cxx.h>
    
    using namespace std;
    using namespace Microsoft::CognitiveServices::Speech;
    
    void recognizeSpeech() {
        // Creates an instance of a speech config with specified subscription key and service region.
        // Replace with your own subscription key and service region (e.g., "chinaeast2").
        auto config = SpeechConfig::FromSubscription("YourSubscriptionKey", "YourServiceRegion");

    
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

[!INCLUDE [replace key and region](../replace-key-and-region.md)]

## Code explanation

[!INCLUDE [code explanation](../code-explanation.md)]

## Build the app

> [!NOTE]
> Make sure to enter the commands below as a _single command line_. The easiest way to do that is to copy the command by using the **Copy** button next to each command, and then paste it at your shell prompt.

* Run the following command to build the application.

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

[!INCLUDE [Speech recognition basics](../../speech-to-text-next-steps.md)]
