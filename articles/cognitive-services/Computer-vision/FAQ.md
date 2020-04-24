---
title: 常见问题解答 - 计算机视觉
titlesuffix: Azure Cognitive Services
description: 获取 Azure 认知服务中计算机视觉 API 的常见问题解答。
services: cognitive-services
author: KellyDF
manager: nitinme
ms.service: cognitive-services
ms.subservice: computer-vision
ms.topic: conceptual
origin.date: 04/17/2019
ms.date: 07/09/2019
ms.author: v-junlch
ms.custom: seodec18
ms.openlocfilehash: 0ff260aaa5519b440f62caaf8a3e313343f8d4e7
ms.sourcegitcommit: c1ba5a62f30ac0a3acb337fb77431de6493e6096
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2020
ms.locfileid: "67844694"
---
# <a name="computer-vision-api-frequently-asked-questions"></a>计算机视觉 API 常见问题解答

> [!TIP]
> 如果在本“常见问题解答”中找不到问题的答案，请尝试在 [StackOverflow](https://stackoverflow.com/questions/tagged/project-oxford+or+microsoft-cognitive) 中向计算机视觉 API 社区提问，或联系 [UserVoice 上的帮助和支持人员](https://cognitive.uservoice.com/)

---

**问**：我能否将计算机视觉 API 定型为使用自定义标记？*例如，我想要馈送猫的种类图片以“训练”AI，然后在 AI 请求中接收种类值。*

**答**：此功能目前不可用。 但是，我们的工程师正在努力为计算机视觉开发此功能。

---

**问**：*能否在未连接 Internet 的情况下本地使用计算机视觉？*

**答**：我们目前未提供本地解决方案。

---

**问**：*计算机视觉能否用于读取牌照？*

**答**：视觉 API 可通过 OCR 有效检测文本，但暂不适合读取牌照。 我们正在不断尝试改进服务，并在功能请求列表中添加了用于自动识别牌照的 OCR。

---

**问**：*手写识别支持什么类型的书写表面？*

**答**：此技术支持不同种类的表面，包括白板、白纸和黄色便笺。

---

**问**：*手写识别操作需要多长时间才能完成？*

**答**：所需时间取决于文本长度。 对于较长的文本，可能要花费几秒钟时间。 因此，在完成“识别手写文本”操作后，可能需要等待一段时间，然后才能使用“获取手写文本操作结果”操作检索结果。

---

**问**：*手写识别技术如何处理使用插入点在一行中间插入的文本？*

**答**：手写识别操作将此类文本作为单独文本行返回。

---

**问**：*手写识别技术如何处理划掉的字词或文本行？*

**答**：如果字词是用多条线划掉，导致其无法识别，那么手写识别操作不会拾取这些字词。 但是，如果单词是使用单条删除线划掉的，则这种划线被视为干扰元素，手写识别操作仍会拾取这些单词。

---

**问**：*手写识别技术支持哪些文本方向？*

**答**：手写识别操作最多可拾取方向介于约 30 度到 40 度角度范围内的文本。

---

<!-- Update_Description: wording update -->