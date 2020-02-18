---
title: 快速入门：从麦克风中识别语音，Python - 语音服务
titleSuffix: Azure Cognitive Services
description: 按照本指南使用适用于 Python 的语音 SDK 创建语音转文本控制台应用程序。 完成后，可以使用计算机的麦克风实时将语音转录为文本。
services: cognitive-services
author: chlandsi
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: include
origin.date: 07/05/2019
ms.date: 01/13/2020
ms.author: v-tawe
ms.openlocfilehash: 5d4f17250a51bfd7205ea19bebcf4716f837a06e
ms.sourcegitcommit: 0b07f1d36ac02da055874630d6edc31cb0a15269
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2020
ms.locfileid: "77112328"
---
## <a name="prerequisites"></a>必备条件

准备工作：

> [!div class="checklist"]
> * [创建一个 Azure 搜索资源](../../../../get-started.md)
> * [设置开发环境](../../../../quickstarts/setup-platform.md)
> * [创建空示例项目](../../../../quickstarts/create-project.md)
> * 请确保你有权访问麦克风，以便进行音频捕获

## <a name="support-and-updates"></a>支持和更新

语音 SDK Python 包的更新将通过 PyPI 分发，[发行说明](~/articles/cognitive-services/Speech-Service/releasenotes.md)中会发布相关通告。
如果有新版本可用，可以使用 `pip install --upgrade azure-cognitiveservices-speech` 命令进行更新。
通过查看 `azure.cognitiveservices.speech.__version__` 变量来检查当前安装的版本。

如果遇到问题或者缺少某项功能，请查看[支持和帮助选项](~/articles/cognitive-services/Speech-Service/support.md)。

## <a name="create-a-python-application-that-uses-the-speech-sdk"></a>使用语音 SDK 创建 Python 应用程序

### <a name="run-the-sample"></a>运行示例

可将本快速入门中的[示例代码](#sample-code)复制到源文件 `quickstart.py`，然后在 IDE 或控制台中运行该代码

```sh
python quickstart.py
```

或者，可以从[语音 SDK 示例存储库](https://github.com/Azure-Samples/cognitive-services-speech-sdk/)以 [Jupyter](https://jupyter.org) Notebook 的形式下载本快速入门教程，并将其作为 Notebook 运行。

### <a name="sample-code"></a>代码示例

> [!NOTE]
> 语音 SDK 将默认使用 en-us 作为语言进行识别。若要了解如何选择源语言，请参阅[指定语音转文本的源语言](../../../../how-to-specify-source-language.md)。

```python
import azure.cognitiveservices.speech as speechsdk

# Creates an instance of a speech config with specified host and subscription key.
# Replace with your own subscription key and service region (e.g., "chinaeast2").
speech_host, speech_key = "wss://YourServiceRegion.stt.speech.azure.cn/", "YourSubscriptionKey"
speech_config = speechsdk.SpeechConfig(host=speech_host, subscription=speech_key)

# Creates a recognizer with the given settings
speech_recognizer = speechsdk.SpeechRecognizer(speech_config=speech_config)

print("Say something...")


# Starts speech recognition, and returns after a single utterance is recognized. The end of a
# single utterance is determined by listening for silence at the end or until a maximum of 15
# seconds of audio is processed.  The task returns the recognition text as result. 
# Note: Since recognize_once() returns only a single utterance, it is suitable only for single
# shot recognition like command or query. 
# For long-running multi-utterance recognition, use start_continuous_recognition() instead.
result = speech_recognizer.recognize_once()

# Checks result.
if result.reason == speechsdk.ResultReason.RecognizedSpeech:
    print("Recognized: {}".format(result.text))
elif result.reason == speechsdk.ResultReason.NoMatch:
    print("No speech could be recognized: {}".format(result.no_match_details))
elif result.reason == speechsdk.ResultReason.Canceled:
    cancellation_details = result.cancellation_details
    print("Speech Recognition canceled: {}".format(cancellation_details.reason))
    if cancellation_details.reason == speechsdk.CancellationReason.Error:
        print("Error details: {}".format(cancellation_details.error_details))
```

### <a name="install-and-use-the-speech-sdk-with-visual-studio-code"></a>通过 Visual Studio Code 安装并使用语音 SDK

1. 在计算机上下载并安装 64 位版本的 [Python](https://www.python.org/downloads/)（3.5 或更高版本）。
1. 下载并安装 [Visual Studio Code](https://code.visualstudio.com/Download)。
1. 打开 Visual Studio Code 并安装 Python 扩展。 在菜单中选择“文件”   >   “首选项” >   “扩展”。 搜索 **Python**。

   ![安装 Python 扩展](~/articles/cognitive-services/Speech-Service/media/sdk/qs-python-vscode-python-extension.png)

1. 创建一个文件夹用于存储项目。 例如，使用 Windows 资源管理器。
1. 在 Visual Studio Code 中选择“文件”图标。  然后打开创建的文件夹。

   ![打开文件夹](~/articles/cognitive-services/Speech-Service/media/sdk/qs-python-vscode-python-open-folder.png)

1. 选择“新建文件”图标创建新的 Python 源文件 `speechsdk.py`。

   ![创建文件](~/articles/cognitive-services/Speech-Service/media/sdk/qs-python-vscode-python-newfile.png)

1. 复制 [Python 代码](#sample-code)并将其粘贴到新建的文件，然后保存文件。
1. 插入语音服务订阅信息。
1. 如果已选择 Python 解释器，窗口底部的状态栏左侧会显示它。
   否则，会显示可用 Python 解释器的列表。 打开命令面板 (Ctrl+Shift+P) 并输入 **Python:Select Interpreter**。 选择适当的解释器。
1. 如果尚未为所选的 Python 解释器安装， 可以从 Visual Studio Code 内部安装语音 SDK Python 包。
   若要安装语音 SDK 包，请打开终端。 再次启动命令面板 (Ctrl+Shift+P) 并输入 **Terminal:Create New Integrated Terminal** 来打开终端。
   在打开的终端中，输入命令 `python -m pip install azure-cognitiveservices-speech`，或者输入适用于系统的命令。
1. 若要运行示例代码，请在编辑器中的某个位置单击右键。 选择“在终端中运行 Python 文件”。 
   根据提示讲出几句话。 片刻之后，听录的文本将会显示。

   ![运行示例](~/articles/cognitive-services/Speech-Service/media/sdk/qs-python-vscode-python-run.png)

如果在遵照这些说明操作时遇到问题，请参阅内容更全面的 [Visual Studio Code Python 教程](https://code.visualstudio.com/docs/python/python-tutorial)。

## <a name="next-steps"></a>后续步骤

[!INCLUDE [footer](./footer.md)]
