---
title: 快速入门：合成语音，C# (.NET Core) - 语音服务
titleSuffix: Azure Cognitive Services
description: 了解如何在 Windows 上使用语音 SDK 通过 .NET Core 下的 C# 合成语音
services: cognitive-services
author: yinhew
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: include
origin.date: 04/04/2020
ms.date: 04/20/2020
ms.author: v-tawe
ms.openlocfilehash: c4a3b50f6155ffeb77047eb6fa2180c0af927cee
ms.sourcegitcommit: f9c242ce5df12e1cd85471adae52530c4de4c7d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/24/2020
ms.locfileid: "82150975"
---
> [!NOTE]
> .NET Core 是一个实现了 [.NET Standard](https://docs.microsoft.com/dotnet/standard/net-standard) 规范的开源跨平台 .NET 平台。

## <a name="prerequisites"></a>先决条件

在开始之前，请务必：

> [!div class="checklist"]
> * [创建一个 Azure 搜索资源](../../../../get-started.md)
> * [设置开发环境并创建空项目](../../../../quickstarts/setup-platform.md?tabs=dotnetcore&pivots=programming-language-csharp)
> * 需要语音 SDK 版本 1.10.0 或更高版本。

## <a name="add-sample-code"></a>添加示例代码

1. 打开 `Program.cs` 并将其中的所有代码替换为以下内容。

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
                }
            }
    
            static void Main()
            {
                SynthesisToSpeakerAsync().Wait();
                Console.WriteLine("Press any key to exit...");
                Console.ReadKey();
            }
        }
    }
    ```

1. 在同一文件中，将字符串 `YourSubscriptionKey` 替换为你的订阅密钥。

1. 还将字符串 `YourServiceRegion` 替换为与订阅关联的[区域](~/articles/cognitive-services/Speech-Service/regions.md)（例如，对于试用订阅，为 `chinaeast2`）。

1. 保存对项目的更改。

## <a name="build-and-run-the-app"></a>生成并运行应用

1. 构建应用程序。 从菜单栏中，选择“构建” > “构建解决方案”   。 编译代码时应不会出错。

    ![Visual Studio 应用程序的屏幕截图，其中突出显示了“生成解决方案”选项](~/articles/cognitive-services/Speech-Service/media/sdk/qs-csharp-dotnetcore-windows-05-build.png "成功的生成")

1. 启动应用程序。 在菜单栏中，选择“调试” > “开始调试”，或按 F5    。

    ![Visual Studio 应用程序的屏幕截图，其中突出显示了“启动调试”选项](~/articles/cognitive-services/Speech-Service/media/sdk/qs-csharp-dotnetcore-windows-06-start-debugging.png "启动应用进行调试")

1. 此时会显示控制台窗口，提示你键入一些文本。 键入几个单词或一个句子。 键入的文本将传输到语音服务，并合成为语音，在扬声器上播放。

    ![成功合成后的控制台输出的屏幕截图](~/articles/cognitive-services/Speech-Service/media/sdk/qs-tts-csharp-dotnet-windows-console-output.png "成功合成后的控制台输出")

## <a name="next-steps"></a>后续步骤

[!INCLUDE [Speech synthesis basics](../../text-to-speech-next-steps.md)]

<!-- ## See also -->

<!-- - [Create a Custom Voice](~/articles/cognitive-services/Speech-Service/how-to-custom-voice-create-voice.md) -->
<!-- - [Record custom voice samples](~/articles/cognitive-services/Speech-Service/record-custom-voice-samples.md) -->
