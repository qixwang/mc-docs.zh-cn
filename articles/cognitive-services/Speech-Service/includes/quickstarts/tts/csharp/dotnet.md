---
title: 快速入门：合成语音，C# (Windows) - 语音服务
titleSuffix: Azure Cognitive Services
description: 按照本指南使用适用于 Windows 的 .NET framework 和语音 SDK 创建文本转语音控制台应用程序。 完成后，你可以从文本合成语音，并实时听到扬声器上的语音。
services: cognitive-services
author: yinhew
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: include
origin.date: 04/04/2020
ms.date: 04/20/2020
ms.author: v-tawe
ms.openlocfilehash: b6b27013b86137077982348dc6d81be9db1e6933
ms.sourcegitcommit: 304d3ef3c9e65c3e85977b3afb9985fbc0f908d6
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/19/2020
ms.locfileid: "85095986"
---
## <a name="prerequisites"></a>先决条件

在开始之前，请务必：

> [!div class="checklist"]
> * [创建一个 Azure 搜索资源](../../../../get-started.md)
> * [设置开发环境并创建空项目](../../../../quickstarts/setup-platform.md?tabs=dotnet&pivots=programming-language-csharp)
> * 需要语音 SDK 版本 1.11.0 或更高版本。

## <a name="add-sample-code"></a>添加示例代码

1. 打开 **Program.cs** 并使用此示例替换自动生成的代码：

    ```csharp
    using System;
    using System.Threading.Tasks;
    using Microsoft.CognitiveServices.Speech;
    
    namespace helloworld
    {
        class Program
        {
            public static async Task SynthesisToSpeakerAsync()
            {
                // Creates an instance of a speech config with specified subscription key and service region.
                // Replace with your own subscription key and service region (e.g., "chinaeast2").
                // The default language is "en-us".
                var config = SpeechConfig.FromSubscription("YourSubscriptionKey", "YourServiceRegion");
    
                // Creates a speech synthesizer using the default speaker as audio output.
                using (var synthesizer = new SpeechSynthesizer(config))
                {
                    // Receive a text from console input and synthesize it to speaker.
                    Console.WriteLine("Type some text that you want to speak...");
                    Console.Write("> ");
                    string text = Console.ReadLine();
    
                    using (var result = await synthesizer.SpeakTextAsync(text))
                    {
                        if (result.Reason == ResultReason.SynthesizingAudioCompleted)
                        {
                            Console.WriteLine($"Speech synthesized to speaker for text [{text}]");
                        }
                        else if (result.Reason == ResultReason.Canceled)
                        {
                            var cancellation = SpeechSynthesisCancellationDetails.FromResult(result);
                            Console.WriteLine($"CANCELED: Reason={cancellation.Reason}");
    
                            if (cancellation.Reason == CancellationReason.Error)
                            {
                                Console.WriteLine($"CANCELED: ErrorCode={cancellation.ErrorCode}");
                                Console.WriteLine($"CANCELED: ErrorDetails=[{cancellation.ErrorDetails}]");
                                Console.WriteLine($"CANCELED: Did you update the subscription info?");
                            }
                        }
                    }
    
                    // This is to give some time for the speaker to finish playing back the audio
                    Console.WriteLine("Press any key to exit...");
                    Console.ReadKey();
                }
            }
    
            static void Main()
            {
                SynthesisToSpeakerAsync().Wait();
            }
        }
    }
    ```

1. 找到字符串 `YourSubscriptionKey` 并将其替换为语音服务订阅密钥。

1. 查找字符串 `YourServiceRegion`，并将其替换为与订阅关联的[区域](~/articles/cognitive-services/Speech-Service/regions.md)。 例如，如果使用的是试用订阅，则区域是 `chinaeast2`。

1. 在菜单栏中，选择“文件” > “全部保存”。

## <a name="build-and-run-the-application"></a>生成并运行应用程序

1. 从菜单栏中，选择“构建” > “构建解决方案”以构建应用程序。 现在，编译代码时应不会提示错误。

1. 选择“调试” > “开始调试”（或选择 F5）以启动 **helloworld** 应用程序。

1. 输入一个英语短语或句子。 应用程序将你的文本传输到语音服务，该服务会将合成的语音发送到应用程序以在你的扬声器上播放。

   ![语音合成用户界面](~/articles/cognitive-services/Speech-Service/media/sdk/qs-tts-csharp-dotnet-windows-console-output.png)

## <a name="next-steps"></a>后续步骤

[!INCLUDE [Speech synthesis basics](../../text-to-speech-next-steps.md)]

<!-- ## See also -->

<!-- - [Create a Custom Voice](~/articles/cognitive-services/Speech-Service/how-to-custom-voice-create-voice.md) -->
<!-- - [Record custom voice samples](~/articles/cognitive-services/Speech-Service/record-custom-voice-samples.md) -->
