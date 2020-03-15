---
title: 区域 - 语音服务
titleSuffix: Azure Cognitive Services
description: 语音服务的可用区域和终结点列表，包括语音转文本、文本转语音和语音翻译。
services: cognitive-services
author: mahilleb-msft
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
origin.date: 11/05/2019
ms.date: 03/23/2020
ms.author: v-tawe
ms.custom: seodec18
ms.openlocfilehash: 93cc72bf2a93d99dbaec76e108148a5c906719ee
ms.sourcegitcommit: 3c98f52b6ccca469e598d327cd537caab2fde83f
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/13/2020
ms.locfileid: "79293300"
---
# <a name="speech-service-supported-regions"></a>语音服务支持的区域

通过语音服务，应用程序可将音频转换为文本、执行语音翻译以及将文本转换为语音。 多个区域中均提供该服务，这些区域为语音 SDK 和 REST API 使用唯一终结点。

此处提供了对所有区域的语音体验执行自定义配置的语音门户： https://speech.azure.cn

对于语音服务的调用，请确保调用与订阅的区域匹配。

## <a name="speech-sdk"></a>语音 SDK

在[语音 SDK](speech-sdk.md) 中，区域指定为字符串（例如，在 C# 语音 SDK 中用作 `SpeechConfig.FromHost` 的参数）。

### <a name="speech-to-text-text-to-speech-and-translation"></a>语音转文本、文本转语音和翻译

语音自定义门户在此处提供： https://speech.azure.cn

可以在以下区域使用语音服务，以进行**语音识别**、**文本转语音**和**翻译**：

[!INCLUDE [](../../../includes/cognitive-services-speech-service-region-identifier.md)]

如果使用[语音 SDK](speech-sdk.md)，则区域由**区域标识符**指定（例如，作为 `SpeechConfig.FromHost` 的参数）。 请确保该区域与订阅的区域匹配。

<!-- ### Intent recognition -->

<!-- ### Voice assistants -->

## <a name="rest-apis"></a>REST API

语音服务还为语音转文本和文本转语音请求公开 REST 终结点。

### <a name="speech-to-text"></a>语音转文本

有关语音转文本的参考文档，请参阅[语音转文本 REST API](rest-speech-to-text.md)。

REST API 的终结点具有以下格式：

```
https://<REGION_IDENTIFIER>.stt.speech.azure.cn/speech/recognition/conversation/cognitiveservices/v1
```

将 `<REGION_IDENTIFIER>` 替换为与下表中的订阅区域匹配的标识符：

[!INCLUDE [](../../../includes/cognitive-services-speech-service-region-identifier.md)]

> [!NOTE]
> 必须将语言参数追加到 URL 以避免收到 4xx HTTP 错误。 例如，使用“中国东部 2”终结点设置为美国英语的语言为：`https://chinaeast2.stt.speech.azure.cn/speech/recognition/conversation/cognitiveservices/v1?language=en-US`。

### <a name="text-to-speech"></a>文本转语音

有关文本转语音的参考文档，请参阅[文本转语音 REST API](rest-text-to-speech.md)。

[!INCLUDE [](../../../includes/cognitive-services-speech-service-endpoints-text-to-speech.md)]
