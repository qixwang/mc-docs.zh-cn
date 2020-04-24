---
title: 快速入门：将语音翻译为语音 - 语音服务
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
ms.openlocfilehash: 6f829f077bd85479f82a98b024b0cc4f8081d083
ms.sourcegitcommit: c1ba5a62f30ac0a3acb337fb77431de6493e6096
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2020
ms.locfileid: "77430093"
---
在本快速入门中，我们将使用[语音 SDK](~/articles/cognitive-services/speech-service/speech-sdk.md) 以交互方式将一种语言的语音翻译为另一种语言的语音。 在满足几项先决条件后，将语音翻译为语音只需六个步骤：
> [!div class="checklist"]
> * 通过语音主机和订阅密钥创建 ````SpeechTranslationConfig```` 对象。
> * 更新 ````SpeechTranslationConfig```` 对象，指定源语言和目标语言。
> * 更新 ````SpeechTranslationConfig```` 对象，指定语音输出语音名称。
> * 使用以上的 ````SpeechTranslationConfig```` 对象创建 ````TranslationRecognizer```` 对象。
> * 使用 ````TranslationRecognizer```` 对象，开始单个言语的识别过程。
> * 检查返回的 ````TranslationRecognitionResult````。
