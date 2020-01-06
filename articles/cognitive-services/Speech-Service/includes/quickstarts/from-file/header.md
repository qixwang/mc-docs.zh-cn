---
title: 快速入门：识别音频文件中的语音 - 语音服务
titleSuffix: Azure Cognitive Services
services: cognitive-services
author: erhopf
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: include
origin.date: 10/28/2019
ms.date: 01/13/2020
ms.author: v-tawe
ms.openlocfilehash: fd83db20f7a07445f5ad162eb23fc72d16c34092
ms.sourcegitcommit: 6fb55092f9e99cf7b27324c61f5fab7f579c37dc
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/03/2020
ms.locfileid: "75631067"
---
在本快速入门中，我们将使用[语音 SDK](~/articles/cognitive-services/speech-service/speech-sdk.md) 从音频文件识别语音。 满足几个先决条件后，从文件识别语音只需五个步骤：
> [!div class="checklist"]
> * 通过订阅密钥和区域创建 ````SpeechConfig```` 对象。
> * 创建指定 .WAV 文件名的 ````AudioConfig```` 对象。
> * 使用以上的 ````SpeechConfig```` 和 ````AudioConfig```` 对象创建 ````SpeechRecognizer```` 对象。
> * 使用 ````SpeechRecognizer```` 对象，开始单个言语的识别过程。
> * 检查返回的 ````SpeechRecognitionResult````。
