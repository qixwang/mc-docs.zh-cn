---
title: 快速入门：将语音翻译为文本 - 语音服务
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
ms.openlocfilehash: 1509645d17d59a289ffb15dbb8a92a9c5bc0c22d
ms.sourcegitcommit: ada94ca4685855f58616e4bf1dd5ca757878dfdc
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/18/2020
ms.locfileid: "77430131"
---
在本快速入门中，我们将使用[语音 SDK](~/articles/cognitive-services/speech-service/speech-sdk.md) 以交互方式将一种语言的语音转换为另一种语言的文本。 在满足几项先决条件后，将语音转换为文本只需五个步骤：
> [!div class="checklist"]
> * 通过语音主机和订阅密钥创建 ````SpeechConfig```` 对象。
> * 更新 ````SpeechConfig```` 对象，指定源语言和目标语言。
> * 使用以上的 ````SpeechConfig```` 对象创建 ````TranslationRecognizer```` 对象。
> * 使用 ````TranslationRecognizer```` 对象，开始单个言语的识别过程。
> * 检查返回的 ````TranslationRecognitionResult````。
