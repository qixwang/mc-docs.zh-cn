---
title: 文本转语音常见问题
titleSuffix: Azure Cognitive Services
description: 获取有关“文本转语音”服务的常见问题的解答。
services: cognitive-services
author: PanosPeriorellis
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
origin.date: 07/05/2019
ms.date: 04/20/2020
ms.author: v-tawe
ms.openlocfilehash: 230b5d3b69c4dc595847f6d931619a9fb2a6327e
ms.sourcegitcommit: a4a2521da9b29714aa6b511fc6ba48279b5777c8
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/24/2020
ms.locfileid: "82126983"
---
# <a name="text-to-speech-frequently-asked-questions"></a>文本转语音常见问题

如果在本常见问题解答中找不到你的问题的解答，请检查[其他支持选项](support.md)。

## <a name="general"></a>常规

<!-- **Q: What is the difference between a standard voice model and a custom voice model?** -->

**问：如果想要使用标准语音模型，应从何处开始？**

**答**：通过 HTTP 请求可以使用超过 10 种语言的 50 多种标准语音模型。 首先，获取[订阅密钥](https://docs.azure.cn/cognitive-services/speech-service/get-started)。 若要对预部署的语音模型进行 REST 调用，请参阅 [REST API](https://docs.azure.cn/cognitive-services/speech-service/rest-apis)。

<!-- **Q: If I want to use a customized voice model, is the API the same as the one that's used for standard voices?** -->

<!-- **Q: Do I need to prepare the training data to create custom voice models on my own?** -->

<!-- **Q: What scripts should I use to record the speech data for custom voice training?** -->

<!-- **Q: What if I need higher concurrency than the default value or what is offered in the portal?** -->

**问：能否下载模型并在本地运行？**

**答**：无法下载模型并在本地执行。

**问：我的请求是否受到限制？**

**答**：REST API 将请求限制为每 5 秒 25 个。 可以在我们的[文本转语音](text-to-speech.md)页面中找到详细信息。

## <a name="next-steps"></a>后续步骤

- [故障排除](troubleshooting.md)
- [发行说明](releasenotes.md)
