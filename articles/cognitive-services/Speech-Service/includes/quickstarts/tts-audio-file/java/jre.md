---
author: erhopf
ms.service: cognitive-services
ms.topic: include
origin.date: 08/06/2019
ms.date: 03/09/2020
ms.author: v-tawe
ms.openlocfilehash: 7f5a556946ad331b81f7a43703f3d790db389756
ms.sourcegitcommit: ced17aa58e800b9e4335276a1595b8045836b256
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/25/2020
ms.locfileid: "77590251"
---
## <a name="prerequisites"></a>先决条件

> [!div class="checklist"]
> * [创建一个 Azure 搜索资源](../../../../get-started.md)
> * [设置开发环境](../../../../quickstarts/setup-platform.md?tabs=jre)
> * [创建空示例项目](../../../../quickstarts/create-project.md?tabs=jre)

## <a name="add-sample-code"></a>添加示例代码

1. 若要向 Java 项目添加新的空类，请选择“文件” > “新建” > “类”。   

1. 在“新建 Java 类”窗口中，在“包”字段内输入 **speechsdk.quickstart**，在“名称”字段内输入 **Main**。   

   ![“新建 Java 类”窗口的屏幕截图](~/articles/cognitive-services/Speech-Service/media/sdk/qs-java-jre-06-create-main-java.png)

1. 将 `Main.java` 中的所有代码替换为以下代码片段：

   ```Java
   package speechsdk.quickstart;

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
               // Replace below with your own subscription key
               String speechSubscriptionKey = "YourSubscriptionKey";
               // Replace below with your own filename.
               String audioFileName = "helloworld.wav";

               // Replace below with your own filename.
               String text = "Hello world!";

               int exitCode = 1;
               // Replace below with your own service region (e.g., "chinaeast2", use the one of SpeechSDKParameters
               // from here: https://docs.azure.cn/cognitive-services/speech-service/regions).
               SpeechConfig config = SpeechConfig.fromHost(new URI("https://YourServiceRegion.tts.speech.azure.cn/"), speechSubscriptionKey);
               assert(config != null);

               AudioConfig audioOutput = AudioConfig.fromWavFileInput(audioFileName);
               assert(audioOutput != null);

               SpeechSynthesizer synth = new SpeechSynthesizer(config, audioOutput);
               assert(synth != null);

               Future<SpeechSynthesisResult> task = synth.SpeakTextAsync(text);
               assert(task != null);

               SpeechSynthesisResult result = task.get();
               assert(result != null);

               if (result.getReason() == ResultReason.SynthesizingAudioCompleted) {
                   System.out.println("Speech synthesized to [" + audioFilename + "] for text [" + text + "]");
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

1. 将 `helloworld.wav` 字符串替换为你自己的文件名。

1. 将 `Hello world!` 字符串替换为你自己的文本。

1. 保存对项目的更改。

## <a name="build-and-run-the-app"></a>生成并运行应用

按 F11，或选择“运行” > “调试”。  
文本转换为语音，并保存在指定的音频数据中。

   ```text
   Speech synthesized to [helloworld.wav] for text [Hello world!]
   ```

## <a name="next-steps"></a>后续步骤

[!INCLUDE [footer](./footer.md)]

<!-- ## See also -->

<!-- - [Create a Custom Voice](~/articles/cognitive-services/Speech-Service/how-to-custom-voice-create-voice.md) -->
<!-- - [Record custom voice samples](~/articles/cognitive-services/Speech-Service/record-custom-voice-samples.md) -->
