---
title: 什么是“翻译”？ - 翻译
titlesuffix: Azure Cognitive Services
description: 将“翻译”集成到应用程序、网站、工具和其他解决方案中，提供多语言用户体验。
services: cognitive-services
author: swmachan
manager: nitinme
ms.service: cognitive-services
ms.subservice: translator-text
ms.topic: overview
origin.date: 06/26/2020
ms.date: 06/22/2020
ms.author: v-tawe
ms.custom: seodec18
ms.openlocfilehash: 03e4e212f7060c1be256a7447030bea0da5fd3f1
ms.sourcegitcommit: 43db4001be01262959400663abf8219e27e5cb8b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/23/2020
ms.locfileid: "85241590"
---
# <a name="what-is-the-translator"></a>什么是“翻译”？

“翻译”可以轻松地集成到应用程序、网站、工具和解决方案中。 它允许你使用 [60 多种语言](language-support.md)添加多语言用户体验，可以在任何硬件平台上使用，以及在任何操作系统中使用，用于文本到文本语言翻译。

“翻译器”是云中机器学习和 AI 算法的 Azure [认知服务 API](/#pivot=products&panel=ai) 集合的一部分，可随时在开发项目中使用。

## <a name="about-microsoft-translator"></a>关于 Microsoft Translator

“翻译”是基于云的机器翻译服务。 核心服务是“翻译”，其为各种 Microsoft 产品和服务提供支持，并已在全球数千家企业的应用程序和工作流中使用，使他们的内容可传播到全球的受众。

语音翻译由“翻译”提供支持，也通过 [Microsoft 语音服务](/cognitive-services/speech-service/)提供。 它将语音翻译 API 和自定义语音服务的功能组合成了一项统一的可完全自定义的服务。 语音服务将替换语音翻译 API，后者自 2019 年 10 月 15 日起停用。

## <a name="language-support"></a>语言支持

Microsoft Translator 为翻译、直译、语言检测和字典提供多语言支持。 请参阅[语言支持](language-support.md)以获取完整的列表，或者通过 [REST API](/cognitive-services/translator/reference/v3-0-languages) 以编程方式访问列表。  

## <a name="microsoft-translator-neural-machine-translation"></a>Microsoft Translator 神经机器翻译

神经机器翻译 (NMT) 是采用 AI 的高质量机器翻译的新标准。 它代替的是旧式统计机器翻译 (SMT) 技术，该技术在 2010-2020 中期的几年中达到了质量顶峰。

与 SMT 相比，NMT 不仅能够从原始翻译质量评分的立场提供更好的翻译，而且听起来更流畅、更类似于人类。 这种流畅性的主要原因在于 NMT 使用一个句子的完整语境来翻译单词。 SMT 仅考虑每个单词前面和后面几个单词的直接语境。

NMT 模型是该 API 的核心，对最终用户不可见。 唯一明显的区别是改进的翻译质量，尤其是针对中文、日语和阿拉伯语等语言。

详细了解 [NMT 的工作原理](https://www.microsoft.com/translator/mt.aspx#nnt)

<!-- Language customization not support in Azure-->

## <a name="next-steps"></a>后续步骤

- [注册](translator-text-how-to-signup.md)访问密钥。
- [API 参考](/cognitive-services/Translator/reference/v3-0-reference)提供了 API 的技术文档。
- [定价详细信息](https://www.azure.cn/pricing/details/cognitive-services)

<!-- Update_Description: wording update -->