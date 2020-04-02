---
title: 不雅内容筛选 - 翻译文本 API
titleSuffix: Azure Cognitive Services
description: 在 Azure 认知服务文本翻译 API 中，使用不雅内容筛选来确定在你的文本中翻译的不雅内容级别。
services: cognitive-services
author: rajdeep-in
manager: nitinme
ms.service: cognitive-services
ms.subservice: translator-text
ms.topic: conceptual
origin.date: 06/04/2019
ms.date: 03/26/2020
ms.author: v-lingwu
ms.openlocfilehash: c57cf4cab787a19b705c0832c0e3a6bc1083aa38
ms.sourcegitcommit: 303a16c7117b6f3495ef0493b4ae8ccb67d7dbba
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/27/2020
ms.locfileid: "80342409"
---
# <a name="add-profanity-filtering-with-the-translator-text-api"></a>使用翻译文本 API 添加不雅内容筛选

通常，Translator 服务在翻译中会保留源中存在的不雅内容。 不雅程度和使词语不雅的语境在不同的文化之间有所不同。 因此，在目标语言中的不雅程度可能会被放大或降低。

如果希望避免在翻译中看到不雅内容（即使源文本中存在不雅内容），可以使用 Translate() 方法中提供的不雅内容筛选选项。 使用该选项可以选择是要看到不雅内容被删除、标有相应标记还是不被执行任何操作。

Translate() 方法采用“options”参数，该参数包含新元素“ProfanityAction”。 可接受的 ProfanityAction 值为“NoAction”、“Marked”和“Deleted”。

## <a name="accepted-values-of-profanityaction-and-examples"></a>可接受的 ProfanityAction 值和示例
|ProfanityAction 值 | 操作 | 示例：源 - 日语 | 示例：目标 - 英语|
| :---|:---|:---|:---|
| NoAction | 默认。 与不设置此选项等效。 不雅内容从源传递到目标。 | 彼は変態です。 | He is a jerk. |
| Marked | 将不雅词语括在 XML 标记 \<profanity> ... \</profanity> 内。 | 彼は変態です。 | He is a \<profanity>jerk\</profanity>. |
| Deleted | 将不雅词语从输出中删除且不替换。 | 彼は。 | 他是一个。 |

## <a name="next-steps"></a>后续步骤
> [!div class="nextstepaction"]
> [通过 Translator API 调用应用不雅内容筛选](reference/v3-0-translate.md)

<!-- Update_Description: update metedata properties -->