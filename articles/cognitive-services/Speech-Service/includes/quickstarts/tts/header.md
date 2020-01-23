---
title: 快速入门：合成语音 - 语音服务
titleSuffix: Azure Cognitive Services
description: 了解如何使用语音 SDK 来合成语音
services: cognitive-services
author: yulin-li
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: include
origin.date: 12/09/2019
ms.date: 01/27/2020
ms.author: v-tawe
ms.openlocfilehash: 7de90941b316e37e3eb812d466aa42bf1e6a7ea6
ms.sourcegitcommit: 94e1c9621b8f81a7078f1412b3a73281d0a8668b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/16/2020
ms.locfileid: "76123532"
---
在本快速入门中，我们将使用[语音 SDK](~/articles/cognitive-services/speech-service/speech-sdk.md) 将文本转换为合成语音。 满足几个先决条件后，将合成语音呈现到默认扬声器只需四个步骤：
> [!div class="checklist"]
> * 通过订阅密钥和区域创建 ````SpeechConfig```` 对象。
> * 使用以上的 ````SpeechConfig```` 对象创建 ````SpeechSynthesizer```` 对象。
> * 使用 ````SpeechSynthesizer```` 对象来朗读文本。
> * 检查返回的 ````SpeechSynthesisResult```` 中是否有错误。
