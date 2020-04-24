---
title: API 参考 - 人脸 API
titleSuffix: Azure Cognitive Services
description: 此 API 参考提供了有关人员、LargePersonGroup/PersonGroup、LargeFaceList/FaceList 和人脸算法 API 的信息。
services: cognitive-services
author: SteveMSFT
manager: nitinme
ms.service: cognitive-services
ms.subservice: face-api
ms.topic: reference
origin.date: 03/01/2018
ms.date: 05/14/2019
ms.author: v-junlch
ms.openlocfilehash: 699efc1f1f1cf57755fb2f39ad9891e75c048f89
ms.sourcegitcommit: c1ba5a62f30ac0a3acb337fb77431de6493e6096
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2020
ms.locfileid: "65668971"
---
# <a name="api-reference"></a>API 参考

Azure 人脸 API 是基于云的 API，可提供用于人脸检测和识别的算法。 人脸 API 包含以下类别：

- 人脸算法 API：涵盖了诸如[检测](https://dev.cognitive.azure.cn/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236)、[查找相似](https://dev.cognitive.azure.cn/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395237)、[验证](https://dev.cognitive.azure.cn/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039523a)、[识别](https://dev.cognitive.azure.cn/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395239)和[分组](https://dev.cognitive.azure.cn/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395238)之类的核心功能。
- [FaceList API](https://dev.cognitive.azure.cn/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039524b)：用来管理用于[查找相似](https://dev.cognitive.azure.cn/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395237)的 FaceList。
- [LargePersonGroup 人员 API](https://dev.cognitive.azure.cn/docs/services/563879b61984550e40cbbe8d/operations/599adcba3a7b9412a4d53f40)：用来管理用于[识别](https://dev.cognitive.azure.cn/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395239)的 LargePersonGroup 人脸。
- [LargePersonGroup API](https://dev.cognitive.azure.cn/docs/services/563879b61984550e40cbbe8d/operations/599acdee6ac60f11b48b5a9d)：用来管理用于[识别](https://dev.cognitive.azure.cn/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395239)的 LargePersonGroup 数据集。
- [LargeFaceList API](https://dev.cognitive.azure.cn/docs/services/563879b61984550e40cbbe8d/operations/5a157b68d2de3616c086f2cc)：用来管理用于[查找相似](https://dev.cognitive.azure.cn/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395237)的 LargeFaceList。
- [PersonGroup 人员 API](https://dev.cognitive.azure.cn/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039523c)：用来管理用于[识别](https://dev.cognitive.azure.cn/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395239)的 PersonGroup 人脸。
- [PersonGroup API](https://dev.cognitive.azure.cn/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395244)：用来管理用于[识别](https://dev.cognitive.azure.cn/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395239)的 PersonGroup 数据集。
- [快照 API](https://dev.cognitive.azure.cn/docs/services/563879b61984550e40cbbe8d/operations/snapshot-take)：用于管理跨订阅的数据迁移的快照。

<!-- Update_Description: link update -->