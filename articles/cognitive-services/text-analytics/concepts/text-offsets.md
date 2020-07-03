---
title: 文本分析 API 中的文本偏移量
titleSuffix: Azure Cognitive Services
description: 了解多语言和表情符号编码导致的偏移量。
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: text-analytics
ms.topic: article
ms.date: 06/24/2020
origin.date: 03/09/2020
ms.author: v-tawe
ms.reviewer: jdesousa
ms.openlocfilehash: 78b923f2bb04dcd267b00692868a2952ae4822a3
ms.sourcegitcommit: f5484e21fa7c95305af535d5a9722b5ab416683f
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/24/2020
ms.locfileid: "85323643"
---
# <a name="text-offsets-in-the-text-analytics-api-output"></a>文本分析 API 输出中的文本偏移量

多语言和表情符号支持已导致 Unicode 编码，该编码使用多个码位来表示单个显示的字符（称为字形）。 例如，🌷 和 👍 之类的表情符号可以使用几个字符将形状与其他字符组合起来，以提供视觉对象特性（例如肤色）。 同样，印地语文字 `अनुच्छेद` 将编码为五个字母和三个组合标记。

由于可能的多语言和表情符号编码的长度不同，文本分析 API 可能会在响应中返回偏移量。

## <a name="offsets-in-the-api-response"></a>API 响应中的偏移量。 

每当在 API 响应（如[命名实体识别](../how-tos/text-analytics-how-to-entity-linking.md)或[情绪分析](../how-tos/text-analytics-how-to-sentiment-analysis.md)）中返回偏移量时，请记住以下事项：

* 响应中的元素可能会特定于所调用的终结点。 
* HTTP POST/GET 有效负载以 [UTF-8](https://www.w3schools.com/charsets/ref_html_utf8.asp) 格式进行编码，该编码不一定是客户端编译器或操作系统上的默认字符编码。
* 偏移量是指基于 [Unicode 8.0.0](https://unicode.org/versions/Unicode8.0.0) 标准的字形计数，而不是字符计数。

## <a name="extracting-substrings-from-text-with-offsets"></a>从具有偏移量的文本中提取子字符串

使用基于字符的子字符串方法（例如 .NET [substring()](https://docs.microsoft.com/dotnet/api/system.string.substring?view=netframework-4.8) 方法）时，偏移量可能会导致问题。 一个问题是，偏移量可能导致子字符串方法在多字符字形编码的中间而不是结尾处结束。

在 .NET 中，考虑使用 [StringInfo](https://docs.microsoft.com/dotnet/api/system.globalization.stringinfo?view=netframework-4.8) 类，该类使你可以将字符串作为一系列文本元素（而不是单个字符对象）来处理。 也可以在首选软件环境中查找字形拆分器库。 

为方便起见，文本分析 API 也会返回这些文本元素。

## <a name="see-also"></a>另请参阅

* [文本分析概述](../overview.md)
* [情绪分析](../how-tos/text-analytics-how-to-sentiment-analysis.md)
* [实体识别](../how-tos/text-analytics-how-to-entity-linking.md)
* [检测语言](../how-tos/text-analytics-how-to-keyword-extraction.md)
* [语言识别](../how-tos/text-analytics-how-to-language-detection.md)
