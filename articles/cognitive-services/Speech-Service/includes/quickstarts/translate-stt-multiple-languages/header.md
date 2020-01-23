---
title: 快速入门：将语音翻译为多种语言 - 语音服务
titleSuffix: Azure Cognitive Services
description: TBD
services: cognitive-services
author: yulin-li
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: include
origin.date: 12/09/2019
ms.date: 01/27/2020
ms.author: v-tawe
ms.openlocfilehash: a7f39f0d451bb32c4eb5e9b7a82d17b413ea3bd7
ms.sourcegitcommit: 94e1c9621b8f81a7078f1412b3a73281d0a8668b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/16/2020
ms.locfileid: "76123502"
---
在本快速入门中，我们将使用[语音 SDK](~/articles/cognitive-services/speech-service/speech-sdk.md) 以交互方式将一种语言的语音翻译为另一种语言的语音。 在满足几项先决条件后，将语音翻译为多种语言的文本只需六个步骤：
> [!div class="checklist"]
> * 通过订阅密钥和区域创建 ````SpeechTranslationConfig```` 对象。
> * 更新 ````SpeechTranslationConfig```` 对象以指定语音识别源语言。
> * 更新 ````SpeechTranslationConfig```` 对象以指定多种翻译目标语言。
> * 使用以上的 ````SpeechTranslationConfig```` 对象创建 ````TranslationRecognizer```` 对象。
> * 使用 ````TranslationRecognizer```` 对象，开始单个言语的识别过程。
> * 检查返回的 ````TranslationRecognitionResult````。
