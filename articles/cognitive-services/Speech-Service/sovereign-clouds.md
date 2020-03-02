---
title: 主权云 - 语音服务
titleSuffix: Azure Cognitive Services
description: 了解如何使用主权云
services: cognitive-services
author: cbasoglu
manager: xdh
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
origin.date: 01/14/2020
ms.date: 02/17/2020
ms.author: v-tawe
ms.openlocfilehash: f16701613accef6058f8e9f8538c024bfaaaf634
ms.sourcegitcommit: 892137d117bcaf9d88aec0eb7ca756fe39613344
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/28/2020
ms.locfileid: "78042305"
---
# <a name="speech-services-with-sovereign-clouds"></a>将语音服务与主权云配合使用

## <a name="microsoft-azure-china"></a>Microsoft Azure 中国

一个位于中国的 Azure 数据中心，可直接访问中国移动、中国电信、中国联通和其他主要承运商主干网络，为中国用户提供高速且稳定的本地网络访问体验。
- 区域：中国东部 2（上海）
- SpeechSDK 中的 SR：*config.FromHost("wss://chinaeast2.stt.speech.azure.cn", "\<your-key\>");*
- SpeechSDK 中的 TTS：*config.FromHost("https[]()://chinaeast2.tts.speech.azure.cn", "\<your-key\>");*
- 身份验证令牌：https[]()://chinaeast2.api.cognitive.azure.cn/sts/v1.0/issueToken
- Azure 门户： https://portal.azure.cn
- 自定义语音识别门户： https://speech.azure.cn/customspeech
- 可用的 SKU：S0
- 支持的功能：
  - 语音转文本
  - 文本转语音
  - 用于文本转语音的神经语音（预览版，免费）
  - 语音翻译器
- 不支持的功能
  - 自定义语音识别（声学/语言调整）
  - 自定义语音
- 支持的区域设置：以下语言的区域设置受支持。
  - 阿拉伯语 (ar-*)
  - 中文 (zh-*)
  - 英语 (en-*)
  - 法语 (fr-*)
  - 德语 (de-*)
  - Hindi
  - 朝鲜语
  - 俄语
  - 西班牙语 (es-*)

