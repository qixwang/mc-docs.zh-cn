---
title: 阻止翻译内容 - 文本翻译 API
titleSuffix: Azure Cognitive Services
description: 使用文本翻译 API 阻止翻译内容。 通过文本翻译 API 可以标记内容，以便不对其进行翻译。
services: cognitive-services
author: rajdeep-in
manager: nitinme
ms.service: cognitive-services
ms.subservice: translator-text
ms.topic: conceptual
origin.date: 03/20/2020
ms.date: 03/26/2020
ms.author: v-lingwu
ms.openlocfilehash: d8993ff98e5501a5f62ac0b79ccd14d1ba9641ff
ms.sourcegitcommit: 303a16c7117b6f3495ef0493b4ae8ccb67d7dbba
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/27/2020
ms.locfileid: "80342410"
---
# <a name="how-to-prevent-translation-of-content-with-the-translator-text-api"></a>如何使用文本翻译 API 阻止翻译内容

通过文本翻译 API 可以标记内容，以便不对其进行翻译。 例如，你可能想要标记本地化后没有意义的代码、品牌名称或单词/短语。

## <a name="methods-for-preventing-translation"></a>阻止翻译的方法

1. 使用 `notranslate` 标记内容。 根据设计，仅当输入 textType 设置为 HTML 时，这才起作用

   示例：

   ```html
   <span class="notranslate">This will not be translated.</span>
   <span>This will be translated. </span>
   ```
   
   ```html
   <div class="notranslate">This will not be translated.</div>
   <div>This will be translated. </div>
   ```

2. 使用[动态词典](dynamic-dictionary.md)给出特定翻译。

3. 不要将字符串传递到文本翻译 API 进行翻译。


## <a name="next-steps"></a>后续步骤
> [!div class="nextstepaction"]
> [避免在 Translator API 调用中进行翻译](reference/v3-0-translate.md)

<!-- Update_Description: update metedata properties -->