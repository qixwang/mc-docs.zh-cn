---
title: 快速入门：识别存储在 Blob 存储中的语音 - 语音服务
titleSuffix: Azure Cognitive Services
description: TBD
services: cognitive-services
author: erhopf
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: include
origin.date: 10/28/2019
ms.date: 01/13/2020
ms.author: v-tawe
zone_pivot_groups: programming-languages-set-two
ms.openlocfilehash: 08d2816974d9dc3d8670042c6e688ccf34b66b1a
ms.sourcegitcommit: 94e1c9621b8f81a7078f1412b3a73281d0a8668b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/16/2020
ms.locfileid: "76123138"
---
在本快速入门中，将使用 REST API 在批处理过程中识别文件中的语音。 批处理过程无需任何用户交互即可执行语音听录。 它为你提供了一个简单的编程模型，而无需管理并发性、自定义语音模型或其他详细信息。 它需要高级控制选项，同时可以有效利用 Azure 语音服务资源。

[批量听录概述](../../../batch-transcription.md)介绍了使用此功能的详细信息。 详细的 API 以 [Swagger 文档](https://chinaeast2.cris.azure.cn/swagger/ui/index#/Custom%20Speech%20transcriptions%3A)的形式在标题 `Custom Speech transcriptins` 下提供。 

以下快速入门将指导你完成使用示例。
