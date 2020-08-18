---
title: 导出和删除数据 - LUIS
titleSuffix: Azure Cognitive Services
description: 你对于查看、导出和删除其数据具有完全控制权。 删除客户数据以确保隐私和合规性。
services: cognitive-services
author: lingliw
manager: digimobile
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: conceptual
origin.date: 04/02/2019
ms.date: 07/24/2019
ms.author: v-lingwu
ms.openlocfilehash: e7a598604147f6f8ae9aad41832a8760b2532514
ms.sourcegitcommit: 9d9795f8a5b50cd5ccc19d3a2773817836446912
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/14/2020
ms.locfileid: "88228349"
---
# <a name="export-and-delete-your-customer-data-in-language-understanding-luis-in-cognitive-services"></a>在认知服务中的语言理解 (LUIS) 中导出和删除客户数据

删除客户数据以确保隐私和合规性。 

## <a name="summary-of-customer-data-request-features"></a>客户数据请求功能摘要
语言理解智能服务 (LUIS) 保存客户内容来使服务运转，但 LUIS 用户拥有完全控制权限来查看、导出以及删除其数据。 这可以通过 LUIS Web [门户](luis-reference-regions.md)或 [LUIS 创作（也称为“编程”）API](https://{region}.dev.cognitive.azure.cn/docs/services/5890b47c39e2bb17b84a55ff/operations/5890b47c39e2bb052c5b9c2f) 来完成。

[!INCLUDE [GDPR-related guidance](../../../includes/gdpr-intro-sentence.md)]

客户内容以加密方式存储在 Microsoft 区域性 Azure 存储中，并且包括：

- 在注册时收集的用户帐户内容
- 构建模型时所需的训练数据
- [主动学习](luis-concept-review-endpoint-utterances.md)使用记录的用户查询来帮助改进模型
  - 用户可以通过将 `&log=false` 追加到请求来关闭查询日志记录，[此处](troubleshooting.md#how-can-i-disable-the-logging-of-utterances)提供了详细信息

## <a name="deleting-customer-data"></a>删除客户数据
LUIS 用户具有完全控制权限，可通过 LUIS Web 门户或 LUIS 创作（也称“编程”）API 来删除任何用户内容。 下表显示了对这两种方式有帮助的链接：

| | **用户帐户** | **应用程序** | **示例话语** | **最终用户查询** |
| --- | --- | --- | --- | --- |
| **门户** | [链接](luis-concept-data-storage.md#delete-an-account) | 链接| [链接](luis-concept-data-storage.md#utterances-in-an-intent) | 主动学习话语<br>[记录的话语](luis-concept-data-storage.md#disable-logging-utterances) |
| **API** | [链接](https://{region}.dev.cognitive.azure.cn/docs/services/5890b47c39e2bb17b84a55ff/operations/5890b47c39e2bb052c5b9c4c) | [链接](https://{region}.dev.cognitive.azure.cn/docs/services/5890b47c39e2bb17b84a55ff/operations/5890b47c39e2bb052c5b9c39) | [链接](https://{region}.dev.cognitive.azure.cn/docs/services/5890b47c39e2bb17b84a55ff/operations/5890b47c39e2bb052c5b9c0b) | [链接](https://{region}.dev.cognitive.azure.cn/docs/services/5890b47c39e2bb17b84a55ff/operations/58b6f32139e2bb139ce823c9) |


## <a name="exporting-customer-data"></a>导出客户数据
LUIS 用户具有完全控制权限可在门户上查看数据，但是，若要导出数据，必须通过 LUIS 创作（也称为“编程”）API 进行。 下表显示了对通过 LUIS 创作（也称为“编程”）API 导出数据有帮助的链接：

| | **用户帐户** | **应用程序** | **陈述** | **最终用户查询** |
| --- | --- | --- | --- | --- |
| **API** | [链接](https://{region}.dev.cognitive.azure.cn/docs/services/5890b47c39e2bb17b84a55ff/operations/5890b47c39e2bb052c5b9c48) | [链接](https://{region}.dev.cognitive.azure.cn/docs/services/5890b47c39e2bb17b84a55ff/operations/5890b47c39e2bb052c5b9c40) | [链接](https://{region}.dev.cognitive.azure.cn/docs/services/5890b47c39e2bb17b84a55ff/operations/5890b47c39e2bb052c5b9c0a) | [链接](https://{region}.dev.cognitive.azure.cn/docs/services/5890b47c39e2bb17b84a55ff/operations/5890b47c39e2bb052c5b9c36) |

## <a name="location-of-active-learning"></a>主动学习的位置

为了启用主动学习，在已发布的 LUIS 终结点收到的用户记录的话语将存储在以下 Azure 地理区域中：

* [中国](#china)

除了主动学习数据（详见下文）外，LUIS 遵循[区域服务的数据存储做法](http://azuredatacentermap.chinacloudsites.cn/)。 


### <a name="china"></a>中国

[luis.azure.cn](https://luis.azure.cn) 门户和中国创作（也称为编程 API）托管在 Azure 的中国地理区域中。 https://luis.azure.cn 门户和中国创作（也称为编程 API）支持将终结点部署到以下 Azure 地理区域：

* 中国

部署到这些 Azure 地理区域时，终结点从应用的最终用户收到的话语将存储在 Azure 的中国地理区域中，以便进行主动学习。

## <a name="next-steps"></a>后续步骤

> [!div class="nextstepaction"]
> [LUIS 区域参考](./luis-reference-regions.md)




