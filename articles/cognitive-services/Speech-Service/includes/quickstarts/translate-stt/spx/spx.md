---
author: v-demjoh
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: include
origin.date: 05/18/2020
ms.date: 06/19/2019
ms.author: v-tawe
ms.openlocfilehash: 26c717575240a16149e6da353c77c8b88076e5e0
ms.sourcegitcommit: d24e12d49708bbe78db450466eb4fccbc2eb5f99
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/01/2020
ms.locfileid: "85613312"
---
## <a name="run-the-speech-cli"></a>运行语音 CLI

现在，可以运行语音 CLI，将语音转换为采用不同语言的文本。

在命令行中，更改为包含语音 CLI 二进制文件的目录，然后键入：

```bash
spx translate --microphone --target de-DE
```

语音 CLI 会将自然语言英语口语转换为采用德语输出的文本。
按 ENTER 停止该工具。

> [!NOTE]
> 语音 CLI 默认为英语。 你可以[从“语音转文本”表中](../../../../language-support.md)选择不同语言。
> 例如，添加 `--source ja-JP` 以识别日语语音。
