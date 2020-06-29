---
author: trevorbye
ms.service: cognitive-services
ms.topic: include
origin.date: 04/02/2020
ms.date: 04/20/2020
ms.author: v-tawe
ms.openlocfilehash: 887d50b39eeccc45ae7093957c999e08b245806a
ms.sourcegitcommit: 304d3ef3c9e65c3e85977b3afb9985fbc0f908d6
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/19/2020
ms.locfileid: "85095911"
---
## <a name="prerequisites"></a>先决条件

准备工作：

> [!div class="checklist"]
> * <a href="https://portal.azure.cn/#create/Microsoft.CognitiveServicesSpeechServices" target="_blank">创建 Azure 语音资源<span class="docon docon-navigate-external x-hidden-focus"></span></a>
> * [设置开发环境并创建空项目](../../../../quickstarts/setup-platform.md?tabs=jre&pivots=programming-language-java)
> * 请确保你有权访问麦克风，以便进行音频捕获
> * 需要语音 SDK 1.11.0 或更高版本。

## <a name="source-code"></a>源代码

若要向 Java 项目添加新的空类，请选择“文件” > “新建” > “类”。**** **** **** 在“新建 Java 类”窗口中，在“包”字段内输入 **speechsdk.quickstart**，在“名称”字段内输入 **Main**。**** **** ****

![“新建 Java 类”窗口的屏幕截图](~/articles/cognitive-services/Speech-Service/media/sdk/qs-java-jre-06-create-main-java.png)

将 *Main.java* 文件的内容替换为以下代码片段：

    ```java
    package speechsdk.quickstart;
    
    import java.net.URI;
    import java.util.concurrent.Future;
    import com.microsoft.cognitiveservices.speech.*;
    
    /**
     * Quickstart: recognize speech using the Speech SDK for Java.
     */
    public class Main {
    
        /**
         * @param args Arguments are ignored in this sample.
         */
        public static void main(String[] args) {
            try {
    
                int exitCode = 1;
                SpeechConfig config = SpeechConfig.fromSubscription("YourSubscriptionKey", "YourServiceRegion");
                assert(config != null);
    
                SpeechRecognizer reco = new SpeechRecognizer(config);
                assert(reco != null);
    
                System.out.println("Say something...");
    
                Future<SpeechRecognitionResult> task = reco.recognizeOnceAsync();
                assert(task != null);
    
                SpeechRecognitionResult result = task.get();
                assert(result != null);
    
                if (result.getReason() == ResultReason.RecognizedSpeech) {
                    System.out.println("We recognized: " + result.getText());
                    exitCode = 0;
                }
                else if (result.getReason() == ResultReason.NoMatch) {
                    System.out.println("NOMATCH: Speech could not be recognized.");
                }
                else if (result.getReason() == ResultReason.Canceled) {
                    CancellationDetails cancellation = CancellationDetails.fromResult(result);
                    System.out.println("CANCELED: Reason=" + cancellation.getReason());
    
                    if (cancellation.getReason() == CancellationReason.Error) {
                        System.out.println("CANCELED: ErrorCode=" + cancellation.getErrorCode());
                        System.out.println("CANCELED: ErrorDetails=" + cancellation.getErrorDetails());
                        System.out.println("CANCELED: Did you update the subscription info?");
                    }
                }
    
                reco.close();
    
                System.exit(exitCode);
            } catch (Exception ex) {
                System.out.println("Unexpected exception: " + ex.getMessage());
    
                assert(false);
                System.exit(1);
            }
        }
    }
    ```

[!INCLUDE [replace key and region](../replace-key-and-region.md)]

## <a name="code-explanation"></a>代码说明

[!INCLUDE [code explanation](../code-explanation.md)]

## <a name="build-and-run-app"></a>生成并运行应用

按 <kbd>F11</kbd>，或选择“运行”**** > “调试”****。
接下来的 15 秒，通过麦克风提供的语音输入将被识别并记录到控制台窗口中。

![成功识别后的控制台输出的屏幕截图](~/articles/cognitive-services/Speech-Service/media/sdk/qs-java-jre-07-console-output.png)

## <a name="next-steps"></a>后续步骤

[!INCLUDE [Speech recognition basics](../../speech-to-text-next-steps.md)]

