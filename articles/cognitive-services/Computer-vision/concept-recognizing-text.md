---
title: 光学字符识别 (OCR) - 计算机视觉
titleSuffix: Azure Cognitive Services
description: 与使用计算机视觉 API 从具有印刷和手写文本的图像和文档中进行光学字符识别 (OCR) 相关的概念。
services: cognitive-services
author: Johnnytechn
manager: netahw
ms.service: cognitive-services
ms.subservice: computer-vision
ms.topic: conceptual
origin.date: 04/17/2019
ms.date: 08/04/2020
ms.author: v-johya
ms.custom: seodec18
ms.openlocfilehash: 04b42ac162ae3973c53052e5c4b025e76a33429c
ms.sourcegitcommit: caa18677adb51b5321ad32ae62afcf92ac00b40b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/08/2020
ms.locfileid: "88023401"
---
# <a name="optical-character-recognition-ocr"></a>光学字符识别 (OCR)

Microsoft 的计算机视觉 API 包含用于从图像和 PDF 文档中提取印刷文本或手写文本的光学字符识别 (OCR) 功能。 OCR API 从模拟文档（图像、扫描的文档）和数字化文档中提取文本。 可以从自然环境中的图像（例如牌照或带有序列号的容器的照片）中，以及文档（发票、账单、财务报表、文章等）中提取文本。 新的读取 OCR API 作为云中或本地（容器）托管服务的一部分提供。 此外，它还支持虚拟网络和专用终结点，以满足企业级合规性和隐私需求。

## <a name="read-api"></a>读取 API 

计算机视觉的[读取 API](https://dev.cognitive.azure.cn/docs/services/computer-vision-v3-ga/operations/5d986960601faab4bf452005) 是 Microsoft 最新的 OCR 技术，可从图像和多页 PDF 文档中提取多种语言的印刷文本、手写文本（仅英语）、数字和货币符号。 该技术经过优化，可以从自然环境中的多文本图像中提取文本，也可从包含混合语言的多页 PDF 文档中提取文本。 它支持检测同一图像或文档中的印刷文本和手写文本（仅英语）。 请参阅 [OCR 支持的语言](/cognitive-services/computer-vision/language-support#optical-character-recognition-ocr)页的完整列表。

### <a name="how-ocr-works"></a>OCR 的工作方式

[读取 API](https://dev.cognitive.azure.cn/docs/services/computer-vision-v3-ga/operations/5d986960601faab4bf452005) 支持最多 2000 页的多文本文档，因此以异步方式执行。 第一步是调用“读取”操作。 “读取”操作将图像或 PDF 文档作为输入，并返回操作 ID。 

第二步是调用[获取结果](https://dev.cognitive.azure.cn/docs/services/computer-vision-v3-ga/operations/5d9869604be85dee480c8750)操作。 此操作采用“读取”操作创建的操作 ID。 然后，它以 JSON 格式返回从图像或文档中提取的文本内容。 JSON 响应会维护已识别单词的原始分组。 其中包括提取的文本行及其边界框坐标。 每个文本行都包含所有提取的单词及其坐标和可信度分数。

如有必要，“读取”会更正已识别文本的旋转，方法是以度为单位返回有关水平图像轴的旋转偏移量，如下图所示。

![OCR 如何将图像和文档转换为带有提取文本的结构化输出](./Images/how-ocr-works.svg)

### <a name="sample-ocr-output"></a>示例 OCR 输出

成功的响应会以 JSON 格式返回，如以下示例所示：

```json
{
  "status": "succeeded",
  "createdDateTime": "2020-05-28T05:13:21Z",
  "lastUpdatedDateTime": "2020-05-28T05:13:22Z",
  "analyzeResult": {
    "version": "3.0.0",
    "readResults": [
      {
        "page": 1,
        "language": "en",
        "angle": 0.8551,
        "width": 2661,
        "height": 1901,
        "unit": "pixel",
        "lines": [
          {
            "boundingBox": [
              67,
              646,
              2582,
              713,
              2580,
              876,
              67,
              821
            ],
            "text": "The quick brown fox jumps",
            "words": [
              {
                "boundingBox": [
                  143,
                  650,
                  435,
                  661,
                  436,
                  823,
                  144,
                  824
                ],
                "text": "The",
                "confidence": 0.958
              }
            ]
          }
        ]
      }
    ]
  }
}
```

### <a name="input-requirements"></a>输入要求

读取 API 采用以下输入：
* 支持的文件格式：JPEG、PNG、BMP、PDF 和 TIFF
* 对于 PDF 和 TIFF，最多处理 2000 页。 对于免费层订阅服务器，只处理前两页。
* 文件大小必须小于 50 MB，且尺寸介于 50 x 50 和 10000 x 10000 像素之间。
* PDF 尺寸必须为 17 x 17 英寸或更小，对应于 Legal 或 A3 纸张尺寸及更小。

### <a name="text-from-images"></a>图像中的文本

以下读取 API 输出显示了从图像中提取的文本行和单词，其中文本具有不同的角度、颜色和字体

![正在旋转的图像及其正在读取和画出的文本](./Images/text-from-images-example.png)

### <a name="text-from-documents"></a>文档中的文本

除了图像，读取 API 还会将 PDF 文档作为输入。

![正在旋转的图像及其正在读取和画出的文本](./Images/text-from-documents-example.png)


### <a name="handwritten-text-in-english"></a>英语手写文本

目前，“读取”操作仅支持提取英语手写文本。

![正在旋转的图像及其正在读取和画出的文本](./Images/handwritten-example.png)

### <a name="printed-text-in-supported-languages"></a>支持语言的印刷文本

读取 3.0 API 支持提取英语、西班牙语、德语、法语、意大利语、葡萄牙语和荷兰语的印刷文本。 [读取 3.1-preview. 1 API 公共预览版](https://dev.cognitive.azure.cn/docs/services/computer-vision-v3-1-preview-1/operations/5d986960601faab4bf452005)增加了对简体中文的支持。 如果你的方案需要支持更多语言，请参阅本文档中的 OCR API 概述。 请参阅所有 [OCR 支持的语言](/cognitive-services/computer-vision/language-support#optical-character-recognition-ocr)的列表

![正在旋转的图像及其正在读取和画出的文本](./Images/supported-languages-example.png)

### <a name="mixed-languages-support"></a>混合语言支持

读取 API 支持包含多种语言的图像和文档，通常称之为混合语言文档。 在提取文本内容之前，通过将文档中的每个文本行分类为检测到的语言来实现此目的。

![正在旋转的图像及其正在读取和画出的文本](./Images/mixed-language-example.png)

### <a name="data-privacy-and-security"></a>数据隐私和安全性

与所有认知服务一样，使用“读取”服务的开发人员应了解针对客户数据的 Microsoft 政策。 有关详细信息，请参阅 [Microsoft 信任中心](https://www.microsoft.com/en-us/trust-center/product-overview)中的“认知服务”页。

### <a name="containers-for-on-premise-deployment"></a>用于本地部署的容器

“读取”还可以作为 Docker 容器（预览版）使用，以使你能够在自己的环境中部署新的 OCR 功能。 容器非常适合用于满足特定的安全性和数据管理要求。 请参阅[如何安装和运行“读取”容器](/cognitive-services/computer-vision/computer-vision-how-to-install-containers)。


## <a name="ocr-api"></a>OCR API

[OCR API](https://dev.cognitive.azure.cn/docs/services/56f91f2d778daf23d8ec6739/operations/56f91f2e778daf14a499e1fc) 使用较旧的识别模式，仅支持图像，且以同步方式执行，并立即返回检测到的文本。 请参阅 [OCR 支持的语言](/cognitive-services/computer-vision/language-support#optical-character-recognition-ocr)而不是读取 API。

## <a name="next-steps"></a>后续步骤

- 了解[读取 3.0 REST API](https://dev.cognitive.azure.cn/docs/services/computer-vision-v3-ga/operations/5d986960601faab4bf452005)。
- 了解[读取 3.1-preview. 1 REST API](https://dev.cognitive.azure.cn/docs/services/computer-vision-v3-1-preview-1/operations/5d986960601faab4bf452005)，其中增加了对简体中文的支持。

