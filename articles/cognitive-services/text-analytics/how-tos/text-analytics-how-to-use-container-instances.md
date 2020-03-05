---
title: 运行 Azure 容器实例 - 文本分析
titleSuffix: Azure Cognitive Services
description: 将文本分析容器部署到 Azure 容器实例，并在 Web 浏览器中对其进行测试。
services: cognitive-services
author: IEvangelist
manager: nitinme
ms.service: cognitive-services
ms.subservice: text-analytics
ms.topic: conceptual
ms.date: 11/21/2019
ms.author: dapine
ms.openlocfilehash: a732ba1f0905d74d540e2fc17081e958043283ba
ms.sourcegitcommit: d202f6fe068455461c8756b50e52acd4caf2d095
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/28/2020
ms.locfileid: "78155326"
---
# <a name="deploy-a-text-analytics-container-to-azure-container-instances"></a>将文本分析容器部署到 Azure 容器实例

了解如何将认知服务[文本分析][install-and-run-containers]容器部署到 Azure [容器实例][container-instances]。 该过程演示了创建文本分析资源的方法、创建关联的情绪分析映像的方法，以及在浏览器中练习前两项的相关业务流程的功能。 使用容器可以将开发人员的注意力从管理基础结构转移到应用程序开发上。

## <a name="prerequisites"></a>先决条件

* 使用 Azure 订阅。 如果没有 Azure 订阅，可在开始前创建一个[试用帐户](https://wd.azure.cn/zh-cn/pricing/1rmb-trial-full)。

[!INCLUDE [Create a Cognitive Services Text Analytics resource](../includes/create-text-analytics-resource.md)]

#### <a name="sentiment-analysis"></a>[情绪分析](#tab/sentiment)

[!INCLUDE [Verify the Sentiment Analysis container instance](../includes/verify-sentiment-analysis-container.md)]

***

## <a name="next-steps"></a>后续步骤 

* 使用[文本分析连接服务](../vs-text-connected-service.md)

[install-and-run-containers]: ./text-analytics-how-to-install-containers.md
[container-instances]: https://docs.microsoft.com/azure/container-instances