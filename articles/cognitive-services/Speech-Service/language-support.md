---
title: 语言支持 - 语音服务
titleSuffix: Azure Cognitive Services
description: 语音服务支持多种语言进行语音到文本转换。 本文提供按服务功能分类的语言支持的完整列表。
services: cognitive-services
author: erhopf
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
origin.date: 11/21/2019
ms.date: 01/13/2020
ms.author: v-tawe
ms.custom: seodec18
ms.openlocfilehash: 4e88efd613fb24370d7821e650ce3806c0cbe131
ms.sourcegitcommit: 6fb55092f9e99cf7b27324c61f5fab7f579c37dc
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/03/2020
ms.locfileid: "75630879"
---
# <a name="language-and-region-support-for-the-speech-service"></a>语音服务的语言和区域支持

语言支持因语音服务功能而异。 下表汇总了对[语音转文本](#speech-to-text)服务产品的语言支持。

## <a name="speech-to-text"></a>语音转文本

Microsoft 语音 SDK 和 REST API 都支持以下语言（区域设置）。 若要提高准确性，可通过上传音频和人为标记的听录内容或相关文本，为语言子集提供自定义：句子。  发音自定义目前仅适用于 `en-US` 和 `de-DE`。 在[此处](how-to-custom-speech.md)详细了解自定义。

 Locale | 语言 | 支持 | 可自定义
------|------------|-----------|-------------
`ar-EG` | 阿拉伯语(埃及)，现代标准 | 是 | 是
`ar-SA` | 阿拉伯语(沙特阿拉伯) | 是 | 是
`ar-AE` | 阿拉伯语(阿拉伯联合酋长国) | 是 | 是
`ar-KW` | 阿拉伯语(科威特) | 是 | 是
`ar-QA` | 阿拉伯语(卡塔尔) | 是 | 是
`ca-ES` | 加泰罗尼亚语 | 是 | 否
`da-DK` | 丹麦语(丹麦) | 是 | 否
`de-DE` | 德语(德国) | 是 | 是
`en-AU` | 英语(澳大利亚) | 是 | 是
`en-CA` | 英语(加拿大) | 是 | 是
`en-GB` | 英语(英国) | 是 | 是
`en-IN` | 英语(印度) | 是 | 是
`en-NZ` | 英语(新西兰) | 是 | 是
`en-US` | 英语(美国) | 是 | 是
`es-ES` | 西班牙语(西班牙) | 是 | 是
`es-MX` | 西班牙语(墨西哥) | 是 | 是
`fi-FI` | 芬兰语(芬兰) | 是 | 否
`fr-CA` | 法语(加拿大) | 是 | 是
`fr-FR` | 法语(法国) | 是 | 是
`gu-IN` | 古吉拉特语(印度) | 是 | 是
`hi-IN` | 印地语(印度) | 是 | 是
`it-IT` | 意大利语(意大利) | 是 | 是
`ja-JP` | 日语(日本) | 是 | 是
`ko-KR` | 韩语(韩国) | 是 | 是
`mr-IN` | 马拉地语(印度) | 是 | 是
`nb-NO` | 书面挪威语(挪威) | 是 | 否
`nl-NL` | 荷兰语(荷兰) | 是 | 是
`pl-PL` | 波兰语(波兰) | 是 | 否
`pt-BR` | 葡萄牙语(巴西) | 是 | 是
`pt-PT` | 葡萄牙语(葡萄牙) | 是 | 是
`ru-RU` | 俄语(俄罗斯) | 是 | 是
`sv-SE` | 瑞典语(瑞典) | 是 | 否
`ta-IN` | 泰米尔语(印度) | 是 | 是
`te-IN` | 泰卢固语(印度) | 是 | 是
`zh-CN` | 中文(普通话，简体) | 是 | 是
`zh-HK` | 中文(粤语，繁体) | 是 | 是
`zh-TW` | 中文(台湾普通话) | 是 | 是
`th-TH` | 泰语(泰国) | 是 | 否
`tr-TR` | 土耳其 | 是 | 是

<!-- ## Text-to-speech -->

<!-- speech-translation -->

## <a name="next-steps"></a>后续步骤

* [获取语音服务试用订阅](https://www.azure.cn/home/features/cognitive-services/)
* [了解如何在 C# 中识别语音](~/articles/cognitive-services/Speech-Service/quickstarts/speech-to-text-from-microphone.md?pivots=programming-language-chsarp)
