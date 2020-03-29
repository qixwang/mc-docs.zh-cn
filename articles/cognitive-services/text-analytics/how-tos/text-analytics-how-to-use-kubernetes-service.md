---
title: 运行 Azure Kubernetes 服务 - 文本分析
titleSuffix: Azure Cognitive Services
description: 将文本分析容器映像部署到 Azure Kubernetes 服务，并在 Web 浏览器中对其进行测试。
services: cognitive-services
author: IEvangelist
manager: nitinme
ms.service: cognitive-services
ms.subservice: text-analytics
ms.topic: conceptual
ms.date: 11/21/2019
ms.author: dapine
ms.openlocfilehash: 33bbb5bbab49adaeb49a849f12be2f7ad0bde248
ms.sourcegitcommit: 303a16c7117b6f3495ef0493b4ae8ccb67d7dbba
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/27/2020
ms.locfileid: "80342377"
---
# <a name="deploy-a-text-analytics-container-to-azure-kubernetes-service"></a>将文本分析容器部署到 Azure Kubernetes 服务

了解如何将 Azure 认知服务[文本分析](/cognitive-services/text-analytics/how-tos/text-analytics-how-to-install-containers)容器映像部署到 Azure Kubernetes 服务 (AKS)。 该过程展示了创建文本分析资源的方法、创建关联的情绪分析映像的方法，以及在浏览器中练习前两项的相关业务流程的方法。 使用容器可以将你的关注点从管理基础结构转移到应用程序开发上。

## <a name="prerequisites"></a>先决条件

此过程要求必须在本地安装和运行多个工具。 请勿使用 Azure Cloud Shell。 需要满足以下条件：

* Azure 订阅。 如果没有 Azure 订阅，可在开始前创建一个[试用帐户](https://wd.azure.cn/zh-cn/pricing/1rmb-trial-full)。
* 文本编辑器，例如 [Visual Studio Code](https://code.visualstudio.com/download)。
* 已安装 [Azure CLI](/cli/install-azure-cli?view=azure-cli-latest)。
* 已安装 [Kubernetes CLI](https://kubernetes.io/docs/tasks/tools/install-kubectl/)。
* 具有适当定价层的 Azure 资源。 并非所有定价层都适用于此容器：
    * 仅具有 F0 或标准定价层的 Azure 文本分析资源  。
    * 具有 S0 定价层的 Azure 认知服务资源  。

[!INCLUDE [Create a Cognitive Services Text Analytics resource](../includes/create-text-analytics-resource.md)]

#### <a name="sentiment-analysis"></a>[情绪分析](#tab/sentiment)

[!INCLUDE [Verify the Sentiment Analysis container instance](../includes/verify-sentiment-analysis-container.md)]

***

## <a name="next-steps"></a>后续步骤

* 使用[文本分析连接服务](../vs-text-connected-service.md)
