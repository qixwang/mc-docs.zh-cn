---
title: 语言检测容器 docker 示例
titleSuffix: Azure Cognitive Services
description: 语言检测容器 docker 示例
services: cognitive-services
author: lingliw
manager: nitinme
ms.service: cognitive-services
ms.topic: include
origin.date: 09/19/2019
ms.date: 02/25/2020
ms.author: v-lingwu
ms.openlocfilehash: f5613fb7c6fda520d6e61d4bf0ecefccf297046e
ms.sourcegitcommit: d202f6fe068455461c8756b50e52acd4caf2d095
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/28/2020
ms.locfileid: "78155465"
---
### <a name="language-detection-container-docker-examples"></a>语言检测容器 docker 示例

以下 docker 示例适用于语言检测容器。

#### <a name="basic-example"></a>基本示例 

  ```
  docker run --rm -it -p 5000:5000 --memory 4g --cpus 1 \
mcr.microsoft.com/azure-cognitive-services/language \
Eula=accept \
Billing={ENDPOINT_URI} \
ApiKey={API_KEY} 
  ```

#### <a name="logging-example"></a>日志记录示例 

  ```
  docker run --rm -it -p 5000:5000 --memory 4g --cpus 1 \
mcr.microsoft.com/azure-cognitive-services/language \
Eula=accept \
Billing={ENDPOINT_URI} \
ApiKey={API_KEY} \
Logging:Console:LogLevel:Default=Information
  ```
