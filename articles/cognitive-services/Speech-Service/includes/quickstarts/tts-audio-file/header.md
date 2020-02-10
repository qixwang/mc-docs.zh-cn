---
author: IEvangelist
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: include
origin.date: 01/31/2020
ms.date: 02/17/2020
ms.author: v-tawe
ms.openlocfilehash: 0c9c342b89371ce233001248f33f180c0f438156
ms.sourcegitcommit: 888cbc10f2348de401d4839a732586cf266883bf
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/05/2020
ms.locfileid: "77028228"
---
在本快速入门中，你将使用[语音 SDK](~/articles/cognitive-services/speech-service/speech-sdk.md) 将文本转换为音频文件中的合成语音。 在[文本转语音语言支持](../../../language-support.md#text-to-speech)下，文本转语音服务为合成语音提供了多种选项。 在满足几项先决条件后，将语音合成到文件只需五个步骤：
> [!div class="checklist"]
> * 通过订阅密钥和区域创建 `SpeechConfig` 对象。
> * 创建指定 .WAV 文件名的“音频配置”对象。
> * 使用上面的配置对象创建 `SpeechSynthesizer` 对象。
> * 使用 `SpeechSynthesizer` 对象，将文本转换为合成语音，并将其保存到指定的音频文件中。
> * 检查返回的 `SpeechSynthesizer` 中是否有错误。
