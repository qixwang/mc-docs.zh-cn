---
title: 快速入门：识别音频文件中的语音 - 语音服务
titleSuffix: Azure Cognitive Services
services: cognitive-services
author: IEvangelist
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: include
origin.date: 01/14/2020
ms.date: 02/17/2020
ms.author: v-tawe
ms.openlocfilehash: 9b65b65f41df2780af73d45b85e2048f1d1daffc
ms.sourcegitcommit: ada94ca4685855f58616e4bf1dd5ca757878dfdc
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/18/2020
ms.locfileid: "77430057"
---
在本快速入门中，我们将使用[语音 SDK](~/articles/cognitive-services/speech-service/speech-sdk.md) 从音频文件识别语音。 满足几个先决条件后，从文件识别语音只需几个步骤：
> [!div class="checklist"]
> * 通过语音主机和订阅密钥创建 `SpeechConfig` 对象。
> * 创建指定 .WAV 文件名的 `AudioConfig` 对象。
> * 使用以上的 `SpeechConfig` 和 `AudioConfig` 对象创建 `SpeechRecognizer` 对象。
> * 使用 `SpeechRecognizer` 对象，开始单个言语的识别过程。
> * 检查返回的 `SpeechRecognitionResult`。
