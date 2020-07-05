---
author: v-demjoh
ms.service: cognitive-services
ms.topic: include
origin.date: 05/13/2020
ms.date: 06/19/2019
ms.author: v-tawe
ms.openlocfilehash: 67cb836372c9826cbd1602be076fb061952b68a7
ms.sourcegitcommit: d24e12d49708bbe78db450466eb4fccbc2eb5f99
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/01/2020
ms.locfileid: "85613356"
---
## <a name="find-a-file-that-contains-speech"></a>查找包含语音的文件

语音 CLI 可以识别多种文件格式和自然语言的语音。 对于本快速入门，可以使用包含英语语音的 WAV 文件（16kHz 或 8kHz，16 位，mono PCM）。

1. 下载 <a href="https://github.com/Azure-Samples/cognitive-services-speech-sdk/blob/master/samples/csharp/sharedcontent/console/whatstheweatherlike.wav" download="whatstheweatherlike" target="_blank">whatstheweatherlike.wav <span class="docon docon-download x-hidden-focus"></span></a>。
2. 将 `whatstheweatherlike.wav` 文件复制到语音 CLI 二进制文件所在的目录中。

## <a name="run-the-speech-cli"></a>运行语音 CLI

现在，可以运行语音 CLI 来识别声音文件中的语音。

在命令行中，更改为包含语音 CLI 二进制文件的目录，然后键入：

```bash
spx recognize --file whatstheweatherlike.wav
```

> [!NOTE]
> 语音 CLI 默认为英语。 你可以[从“语音转文本”表中](../../../../language-support.md)选择不同语言。
> 例如，添加 `--source de-DE` 以识别德语语音。

语音 CLI 将在屏幕上显示语音的文本转录。 然后，语音 CLI 将关闭。
