---
title: 品牌检测 - 计算机视觉
titleSuffix: Azure Cognitive Services
description: 本文讨论一种特殊的对象检测模式，即借助计算机视觉 API 进行品牌和/或徽标检测。
services: cognitive-services
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: computer-vision
ms.topic: conceptual
ms.date: 06/08/2020
ms.author: v-tawe
origin.date: 08/08/2019
ms.openlocfilehash: 9a64b76d270fc27e7f87aa4f57597b8cc8273bc3
ms.sourcegitcommit: 8dae792aefbe44e8388f961b813e3da6564423ec
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/10/2020
ms.locfileid: "84655002"
---
# <a name="detect-popular-brands-in-images"></a>检测图像中的流行品牌

品牌检测是[对象检测](concept-object-detection.md)的一种专用模式，使用包含全球数千个徽标的数据库来标识图像或视频中的商业品牌。 可以使用此功能来执行特定的操作，例如，发现哪些品牌在社交媒体上最受欢迎，或者哪些品牌在社交产品排名上最靠前。

计算机视觉服务检测给定图像中是否存在品牌徽标；如果存在，则返回品牌名称、置信度分数以及徽标周围边框的坐标。

<!--Custom vision is not available-->

## <a name="brand-detection-example"></a>品牌检测示例

以下 JSON 响应表明计算机视觉在示例图像中检测品牌时所返回的内容。

![上面有 Microsoft 标签的徽标的红色衬衫](./Images/red-shirt-logo.jpg)

```json
"brands":[  
   {  
      "name":"Microsoft",
      "rectangle":{  
         "x":20,
         "y":97,
         "w":62,
         "h":52
      }
   }
]
```

在某些情况下，品牌检测程序会挑选徽标图像和风格化品牌名称作为两个单独徽标。

![上面有 Microsoft 标签和徽标的灰色运动衫](./Images/gray-shirt-logo.jpg)

```json
"brands":[  
   {  
      "name":"Microsoft",
      "rectangle":{  
         "x":58,
         "y":106,
         "w":55,
         "h":46
      }
   },
   {  
      "name":"Microsoft",
      "rectangle":{  
         "x":58,
         "y":86,
         "w":202,
         "h":63
      }
   }
]
```

## <a name="use-the-api"></a>使用 API

品牌检测功能属于[分析图像](https://dev.cognitive.azure.cn/docs/services/5adf991815e1060e6355ad44/operations/56f91f2e778daf14a499e1fa) API。 可以通过本机 SDK 或 REST 调用来调用此 API。 将 `Brands` 包括在 **visualFeatures** 查询参数中。 然后，在获取完整 JSON 响应时，就只需分析 `"brands"` 部分内容的字符串。

* [快速入门：计算机视觉 .NET SDK](./quickstarts-sdk/client-library.md?pivots=programming-language-csharp)
* [快速入门：分析图像 (REST API)](./quickstarts/csharp-analyze.md)
