---
title: 验证情绪分析容器实例
titleSuffix: Azure Cognitive Services
description: 了解如何验证情绪分析容器实例。
services: cognitive-services
author: lingliw
manager: nitinme
ms.service: cognitive-services
ms.topic: include
origin.date: 09/12/2019
ms.date: 02/25/2020
ms.author: v-lingwu
ms.openlocfilehash: 1eadad45c95ba5ae344f9a186d8b7b37f0310bda
ms.sourcegitcommit: c1ba5a62f30ac0a3acb337fb77431de6493e6096
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2020
ms.locfileid: "78155325"
---
### <a name="verify-the-sentiment-analysis-container-instance"></a>验证情绪分析容器实例

1. 选择“概述”  选项卡，并复制 IP 地址。
1. 打开新的浏览器选项卡，并输入 IP 地址。 例如，输入 `http://<IP-address>:5000 (http://55.55.55.55:5000`)。 此时将显示容器的主页，让你知道该容器正在运行。

    ![查看容器主页，以验证它是否处于运行状态](../media/how-tos/container-instance/swagger-docs-on-container.png)

1. 选择“服务 API 说明”  链接，以转到该容器的 Swagger 页。

1. 选择任意一个 **POST** API，然后选择“试用”  。此时将显示参数，其中包括以下示例输入：

    ```json
    {
      "documents": [
        {
          "language": "en",
          "id": "1",
          "text": "Hello world. This is some input text that I love."
        },
        {
          "language": "fr",
          "id": "2",
          "text": "Bonjour tout le monde"
        },
        {
          "language": "es",
          "id": "3",
          "text": "La carretera estaba atascada. Había mucho tráfico el día de ayer."
        }
      ]
    }
    ```

1. 将该输入替换为以下 JSON 内容：

    ```json
    {
      "documents": [
        {
          "language": "en",
          "id": "7",
          "text": "I was fortunate to attend the KubeCon Conference in Barcelona, it is one of the best conferences I have ever attended. Great people, great sessions and I thoroughly enjoyed it!"
        }
      ]
    }
    ```

1. 将“showStats”  设置为 `true`。

1. 选择“执行”  以确定文本的情绪。

    在容器中打包的模型会生成范围从 0 到 1 的分数，其中 0 是负面情绪，而 1 是正面情绪。

    返回的 JSON 响应包括更新的文本输入的情绪：

    ```json
    {
      "documents": [
      {
        "id": "7",
        "score": 0.9826303720474243,
        "statistics": {
          "charactersCount": 176,
            "transactionsCount": 1
          }
        }
      ],
      "errors": [],
      "statistics": {
        "documentsCount": 1,
        "validDocumentsCount": 1,
        "erroneousDocumentsCount": 0,
        "transactionsCount": 1
      }
    }
    ```

我们现在可以将响应有效负载的 JSON 数据的文档 `id` 关联到原始请求有效负载文档 `id`。 超过 `0.98` 的分数表示非常积极的情绪。

<!-- Update_Description: wording update -->