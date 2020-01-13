---
title: 快速入门：在 Android 上运行语音设备 SDK - 语音服务
titleSuffix: Azure Cognitive Services
description: Android 语音设备 SDK 入门的先决条件和说明。
services: cognitive-services
author: erhopf
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: quickstart
origin.date: 12/17/2019
ms.date: 01/13/2020
ms.author: v-tawe
ms.openlocfilehash: 8752299185013ef54e061ae580f64ce219d0dd8d
ms.sourcegitcommit: 6fb55092f9e99cf7b27324c61f5fab7f579c37dc
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/03/2020
ms.locfileid: "75630785"
---
# <a name="quickstart-run-the-speech-devices-sdk-sample-app-on-android"></a>快速入门：在 Android 上运行语音设备 SDK 示例应用

本快速入门介绍如何使用适用于 Android 的语音设备 SDK 来生成支持语音的产品。

本指南需要一个包含语音服务资源的 [Azure 认知服务](get-started.md)帐户。 如果没有帐户，可以使用[试用帐户](https://www.azure.cn/pricing/1rmb-trial-full/?form-type=identityauth)获取订阅密钥。

示例应用程序的源代码随附在语音设备 SDK 中， 也可在 [GitHub 上获取](https://github.com/Azure-Samples/Cognitive-Services-Speech-Devices-SDK)。

## <a name="prerequisites"></a>先决条件

开始使用语音设备 SDK 之前，需要：

- 按[开发工具包](get-speech-devices-sdk.md)中提供的说明启动设备。

- 下载[语音设备 SDK](https://aka.ms/sdsdk-download) 的最新版本，并将 .zip 提取到工作目录。

  > [!NOTE]
  > Android-Sample-Release.zip 文件包含 Android 示例应用，本快速入门假设该应用提取到 C:\SDSDK\Android-Sample-Release

- 获取[语音服务的 Azure 订阅密钥](get-started.md)

<!-- - If you plan to use the Conversation Transcription you must use a [circular microphone device](get-speech-devices-sdk.md) and this feature is currently only available for "en-US" and "zh-CN" in regions, “chinaeast2”. You must have a speech key in one of those regions to use Conversation Transcription. -->

<!-- - If you plan to use the Speech Services to identify intents (or actions) from user utterances, you'll need a [Language Understanding Service (LUIS)](https://docs.azure.cn/cognitive-services/luis/azureibizasubscription) subscription. To learn more about LUIS and intent recognition, see [Recognize speech intents with LUIS, C#](https://docs.azure.cn/cognitive-services/speech-service/how-to-recognize-intents-from-speech-csharp). -->

<!--  You can [create a simple LUIS model](https://docs.azure.cn/cognitive-services/luis/) or use the sample LUIS model, LUIS-example.json. The sample LUIS model is available from the [Speech Devices SDK download site](https://aka.ms/sdsdk-luis). To upload your model's JSON file to the [LUIS portal](https://luis.azure.cn), select **Import new app**, and then select the JSON file. -->

- 在电脑上安装 [Android Studio](https://developer.android.com/studio/) 和 [Vysor](https://vysor.io/download/)。

## <a name="set-up-the-device"></a>设置设备

1. 在计算机上启动 Vysor。

   ![Vysor](media/speech-devices-sdk/qsg-3.png)

1. 你的设备应列在“选择设备”下。  选择设备旁边的“视图”按钮。 

1. 选择文件夹图标，然后选择“设置” > “WLAN”连接到无线网络。  

   ![Vysor WLAN](media/speech-devices-sdk/qsg-4.png)

   > [!NOTE]
   > 如果你的公司有关于将设备连接到 Wi-Fi 系统的政策，则你需要获取 MAC 地址并联系 IT 部门来了解如何将它连接到公司的 Wi-Fi。
   >
   > 若要查找开发工具包的 MAC 地址，请在开发工具包的桌面上选择文件夹图标。
   >
   > ![Vysor 文件夹](media/speech-devices-sdk/qsg-10.png)
   >
   > Select <bpt id="p1">**</bpt>Settings<ept id="p1">**</ept>. 搜索“mac 地址”，然后选择“Mac 地址” > “高级 WLAN”。   记下对话框底部附近显示的 MAC 地址。
   >
   > ![Vysor MAC 地址](media/speech-devices-sdk/qsg-11.png)
   >
   > 某些公司可能会限制设备可以连接到其 Wi-Fi 系统的时长。 在一定的天数后，可能需要延长开发工具包在 Wi-Fi 系统中的注册。

## <a name="run-the-sample-application"></a>运行示例应用程序

若要验证开发工具包设置，请生成并安装示例应用程序：

1. 启动 Android Studio。

1. 选择“打开现有 Android Studio 项目”  。

   ![Android Studio - 打开现有项目](media/speech-devices-sdk/qsg-5.png)

1. 转到 C:\SDSDK\Android-Sample-Release\example。 选择“确定”打开示例项目。 

1. 将语音订阅密钥添加到源代码。 

   对于语音和 LUIS，你的信息会进入 MainActivity.java：

   ```java
    // Subscription
    private static String SpeechSubscriptionKey = "<enter your subscription info here>";
    private static String SpeechRegion = "chinaeast2";
    private static String LuisSubscriptionKey = "<enter your subscription info here>";
    private static String LuisRegion = "chinaeast2"; / you can change this, if your LUIS region is different.
    private static String LuisAppId = "<enter your LUIS AppId>";
   ```

   <!-- If you are using conversation transcription, your speech key and region information are also needed in conversation.java: -->

<!-- 1. The default keyword is "Computer". You can also try one of the other provided keywords, like "Machine" or "Assistant". The resource files for these alternate keywords are in the Speech Devices SDK, in the keyword folder. For example, C:\SDSDK\Android-Sample-Release\keyword\Computer contains the files used for the keyword "Computer". -->

1. 更新包含麦克风阵列几何设置的以下行：

   ```java
   private static final String DeviceGeometry = "Circular6+1";
   private static final String SelectedGeometry = "Circular6+1";
   ```

   此表列出了支持的值：

   | 变量 | 含义 | 可用值 |
   | -------- | ------- | ---------------- |
   | `DeviceGeometry` | 物理麦克风配置 | 环形开发工具包：`Circular6+1` |
   |          |         | 线性开发工具包：`Linear4` |
   | `SelectedGeometry` | 软件麦克风配置 | 使用所有麦克风的环形开发工具包：`Circular6+1` |
   |          |         | 使用四个麦克风的环形开发工具包：`Circular3+1` |
   |          |         | 使用所有麦克风的线性开发工具包：`Linear4` |
   |          |         | 使用两个麦克风的线性开发工具包：`Linear2` |

1. 若要生成应用程序，请在“运行”菜单中选择“运行‘应用’”。   此时会显示“选择部署目标”对话框。 

1. 选择设备，然后选择“确定”，将应用程序部署到设备。 

   ![“选择部署目标”对话框](media/speech-devices-sdk/qsg-7.png)

1. 语音设备 SDK 示例应用程序将会启动，并显示以下选项：

   ![示例语音设备 SDK 的示例应用程序和选项](media/speech-devices-sdk/qsg-8.png)

<!-- 1. Try the new Conversation Transcription demo. Start transcribing with 'Start Session'. By default everyone is a guest. However, if you have participant’s voice signatures they can be put into a file `/video/participants.properties` on the device. To generate the voice signature, look at [Transcribe conversations (SDK)](how-to-use-conversation-transcription-service.md). -->

   <!-- ![Demo Conversation Transcription application](media/speech-devices-sdk/qsg-15.png) -->

1. 尽情体验吧！

## <a name="troubleshooting"></a>故障排除

如果无法连接到语音设备。 在命令提示符窗口中键入以下命令。 这会返回设备列表：

```powershell
 adb devices
```

> [!NOTE]
> 此命令使用 Android Debug Bridge `adb.exe`，它是 Android Studio 安装的一部分。 此工具位于 C:\Users\[用户名]\AppData\Local\Android\Sdk\platform-tools 中。 可将该目录添加到你的路径，以便更轻松地调用 `adb`。 否则，必须在调用 `adb` 必须在每个调用 adb 的命令中指定到 adb.exe 的完整安装路径。
>
> 如果看到 `no devices/emulators found` 错误，请检查 USB 电缆是否已连接，并确保使用了高品质的电缆。

## <a name="next-steps"></a>后续步骤

> [!div class="nextstepaction"]
> [查看发行说明](devices-sdk-release-notes.md)