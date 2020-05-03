---
title: 快速入门：合成语音，Java（Windows、Linux、macOS）- 语音服务
titleSuffix: Azure Cognitive Services
description: 本快速入门介绍如何创建一个简单的 Java 应用程序，它可以从文本捕获和合成语音，并使用默认扬声器播放它。
services: cognitive-services
author: yulin-li
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: include
origin.date: 04/04/2020
ms.date: 04/20/2020
ms.author: v-tawe
ms.openlocfilehash: 39f313c4b29237201c4ba111040a02758b0d469c
ms.sourcegitcommit: f9c242ce5df12e1cd85471adae52530c4de4c7d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/24/2020
ms.locfileid: "82150823"
---
## <a name="prerequisites"></a>先决条件

在开始之前，请务必：

> [!div class="checklist"]
> * [创建一个 Azure 搜索资源](../../../../get-started.md)
> * [设置开发环境并创建空项目](../../../../quickstarts/setup-platform.md?tabs=jre&pivots=programming-language-java)
> * 需要语音 SDK 版本 1.10.0 或更高版本。

## <a name="add-sample-code"></a>添加示例代码

1. 若要向 Java 项目添加新的空类，请选择“文件” > “新建” > “类”。   

1. 在“新建 Java 类”窗口中，在“包”字段内输入 **speechsdk.quickstart**，在“名称”字段内输入 **Main**。   

   ![“新建 Java 类”窗口的屏幕截图](~/articles/cognitive-services/Speech-Service/media/sdk/qs-java-jre-06-create-main-java.png)

1. 将 `Main.java` 中的所有代码替换为以下代码片段：

    ```java
    package speechsdk.quickstart;
    
    import java.util.Scanner;
    import java.util.concurrent.Future;
    import com.microsoft.cognitiveservices.speech.*;
    
    /**
     * Quickstart: synthesize speech using the Speech SDK for Java.
     */
    public class Main {
    
        /**
         * @param args Arguments are ignored in this sample.
         */
        public static void main(String[] args) {
            try {
                // Replace below with your own subscription key
                String speechSubscriptionKey = "YourSubscriptionKey";
    
                int exitCode = 1;
                // Replace below with your own service region (e.g., "chinaeast2").
                SpeechConfig config = SpeechConfig.fromSubscription("YourSubscriptionKey", "YourServiceRegion");
                assert(config != null);
    
                SpeechSynthesizer synth = new SpeechSynthesizer(config);
                assert(synth != null);
    
                System.out.println("Type some text that you want to speak...");
                System.out.print("> ");
                String text = new Scanner(System.in).nextLine();
    
                Future<SpeechSynthesisResult> task = synth.SpeakTextAsync(text);
                assert(task != null);
    
                SpeechSynthesisResult result = task.get();
                assert(result != null);
    
                if (result.getReason() == ResultReason.SynthesizingAudioCompleted) {
                    System.out.println("Speech synthesized to speaker for text [" + text + "]");
                    exitCode = 0;
                }
                else if (result.getReason() == ResultReason.Canceled) {
                    SpeechSynthesisCancellationDetails cancellation = SpeechSynthesisCancellationDetails.fromResult(result);
                    System.out.println("CANCELED: Reason=" + cancellation.getReason());
    
                    if (cancellation.getReason() == CancellationReason.Error) {
                        System.out.println("CANCELED: ErrorCode=" + cancellation.getErrorCode());
                        System.out.println("CANCELED: ErrorDetails=" + cancellation.getErrorDetails());
                        System.out.println("CANCELED: Did you update the subscription info?");
                    }
                }
    
                result.close();
                synth.close();
                
                System.exit(exitCode);
            } catch (Exception ex) {
                System.out.println("Unexpected exception: " + ex.getMessage());
    
                assert(false);
                System.exit(1);
            }
        }
    }
    ```

1. 将字符串 `YourSubscriptionKey` 替换为你的订阅密钥。

1. 将字符串 `YourServiceRegion` 替换为与订阅关联的[区域](~/articles/cognitive-services/Speech-Service/regions.md)（例如，对于试用订阅，为 `chinaeast2`）。

1. 保存对项目的更改。

## <a name="build-and-run-the-app"></a>生成并运行应用

按 F11，或选择“运行” > “调试”。  
出现提示时输入文本，你将听到从默认扬声器播放的合成音频。

## <a name="next-steps"></a>后续步骤

[!INCLUDE [Speech synthesis basics](../../text-to-speech-next-steps.md)]

<!-- ## See also -->

<!-- - [Create a Custom Voice](~/articles/cognitive-services/Speech-Service/how-to-custom-voice-create-voice.md) -->
<!-- - [Record custom voice samples](~/articles/cognitive-services/Speech-Service/record-custom-voice-samples.md) -->
