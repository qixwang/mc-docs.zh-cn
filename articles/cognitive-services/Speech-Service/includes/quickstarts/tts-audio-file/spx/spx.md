---
author: v-demjoh
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: include
origin.date: 05/18/2020
ms.date: 06/19/2019
ms.author: v-tawe
ms.openlocfilehash: de2523203d36714d0ef1c0fc3d0aa61effbebea0
ms.sourcegitcommit: d24e12d49708bbe78db450466eb4fccbc2eb5f99
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/01/2020
ms.locfileid: "85611914"
---
## <a name="run-the-speech-cli"></a>运行语音 CLI

现在，可以运行语音 CLI，以将语音从文本合成到新的音频文件。

在命令行中，更改为包含语音 CLI 二进制文件的目录，然后键入：

```bash
spx synthesize --text "The speech synthesizer greets you!" --audio output greetings.wav
```

语音 CLI 将采用英文向 `greetings.wav` 音频文件生成自然语言。
在 Windows 中，输入 `start greetings.wav` 可以播放音频文件。
