---
author: IEvangelist
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: include
origin.date: 01/31/2020
ms.date: 02/17/2020
ms.author: v-tawe
ms.openlocfilehash: 4b233018cb250ae31dd3dd6c4c4c4af35e616b8a
ms.sourcegitcommit: ada94ca4685855f58616e4bf1dd5ca757878dfdc
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/18/2020
ms.locfileid: "77430077"
---
在本快速入门中，你将使用[语音 SDK](~/articles/cognitive-services/speech-service/speech-sdk.md) 将文本转换为音频文件中的合成语音。 在[文本转语音语言支持](../../../language-support.md#text-to-speech)下，文本转语音服务为合成语音提供了多种选项。 在满足几项先决条件后，将语音合成到文件只需五个步骤：
> [!div class="checklist"]
> * 通过语音主机和订阅密钥创建 `SpeechConfig` 对象。
> * 创建指定 .WAV 文件名的“音频配置”对象。
> * 使用上面的配置对象创建 `SpeechSynthesizer` 对象。
> * 使用 `SpeechSynthesizer` 对象，将文本转换为合成语音，并将其保存到指定的音频文件中。
> * 检查返回的 `SpeechSynthesizer` 中是否有错误。
