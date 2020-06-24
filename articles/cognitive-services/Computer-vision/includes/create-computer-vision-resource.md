---
title: 容器支持
titleSuffix: Azure Cognitive Services
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.topic: include
ms.date: 06/08/2020
ms.author: v-tawe
origin.date: 04/01/2020
ms.openlocfilehash: 9e695bf28d0697f276fd8d55d9899a5ab64c1f3e
ms.sourcegitcommit: 8dae792aefbe44e8388f961b813e3da6564423ec
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/10/2020
ms.locfileid: "84655022"
---
## <a name="create-an-computer-vision-resource"></a>创建计算机视觉资源

1. 登录到 [Azure 门户](https://portal.azure.cn)。
1. 单击[创建“计算机视觉”](https://portal.azure.cn/#create/Microsoft.CognitiveServicesComputerVision)资源****。
1. 输入所有必需的设置：

    |设置|Value|
    |--|--|
    |名称|所需名称（2-64 个字符）|
    |订阅|选择相应的订阅|
    |位置|选择附近任何可用的位置|
    |定价层|`F0` - 最小定价层|
    |资源组|选择可用的资源组|

1. 单击“创建”**** 并等待创建资源。 创建资源后，导航到资源页。
1. 收集配置的 `{ENDPOINT_URI}` 和 `{API_KEY}`，请参阅[收集所需参数](../computer-vision-how-to-install-containers.md#gathering-required-parameters)获取详细信息。
