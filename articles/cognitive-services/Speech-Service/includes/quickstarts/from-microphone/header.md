---
title: 快速入门：从麦克风中识别语音 - 语音服务
titleSuffix: Azure Cognitive Services
description: TBD
services: cognitive-services
author: erhopf
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: include
origin.date: 11/20/2019
ms.date: 01/13/2020
ms.author: v-tawe
ms.openlocfilehash: fa43c7e31f806e345d4ac6f72c08799f85b7117c
ms.sourcegitcommit: 0b07f1d36ac02da055874630d6edc31cb0a15269
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2020
ms.locfileid: "77112324"
---
本快速入门介绍如何使用[语音 SDK](~/articles/cognitive-services/speech-service/speech-sdk.md) 以交互方式识别来自麦克风输入的语音，以及从捕获的音频中获取听录文本。 可轻松地将此功能集成到应用或设备中，用于执行对话听录等常见识别任务。 它还可用于更加复杂的集成，例如配合使用 Bot Framework 和语音 SDK 来生成语音助手。

满足几个先决条件后，通过麦克风识别语音只需四个步骤：

> [!div class="checklist"]
> * 通过语音主机和订阅密钥创建 `SpeechConfig` 对象。
> * 使用以上的 `SpeechConfig` 对象创建 `SpeechRecognizer` 对象。
> * 使用 `SpeechRecognizer` 对象，开始单个言语的识别过程。
> * 检查返回的 `SpeechRecognitionResult`。
