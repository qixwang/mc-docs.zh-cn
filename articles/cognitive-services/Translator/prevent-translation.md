---
title: 防止翻译内容 - 翻译器
titleSuffix: Azure Cognitive Services
description: 使用翻译器防止翻译内容。 使用翻译器可以标记内容，以便不对其进行翻译。
services: cognitive-services
author: swmachan
manager: nitinme
ms.service: cognitive-services
ms.subservice: translator-text
ms.topic: conceptual
origin.date: 05/26/2020
ms.date: 06/22/2020
ms.author: v-tawe
ms.openlocfilehash: 89b0ef40eca1ccb64e717aef22899aee8da7d21a
ms.sourcegitcommit: 43db4001be01262959400663abf8219e27e5cb8b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/23/2020
ms.locfileid: "85241528"
---
# <a name="how-to-prevent-translation-of-content-with-the-translator"></a>如何使用翻译器防止翻译内容

使用翻译器可以标记内容，以便不对其进行翻译。 例如，你可能想要标记本地化后没有意义的代码、品牌名称或单词/短语。

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

3. 不要将字符串传递到翻译器进行翻译。



## <a name="next-steps"></a>后续步骤
> [!div class="nextstepaction"]
> [避免在翻译器调用中进行翻译](reference/v3-0-translate.md)

<!-- Update_Description: update metedata properties -->