---
title: Azure 媒体服务高可用性流式处理
description: 了解在发生区域性数据中心中断或故障时，如何故障转移到辅助媒体服务帐户。
services: media-services
documentationcenter: ''
author: WenJason
manager: digimobile
editor: ''
ms.service: media-services
ms.subservice: ''
ms.workload: ''
ms.topic: article
ms.custom: ''
origin.date: 02/24/2020
ms.date: 07/27/2020
ms.author: v-jay
ms.openlocfilehash: 64850dfa2e61c0bede763e75fd94981bf0b35fd9
ms.sourcegitcommit: 091c672fa448b556f4c2c3979e006102d423e9d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/24/2020
ms.locfileid: "87162750"
---
# <a name="media-services-high-availability-streaming"></a>媒体服务高可用性流式处理

如果出现区域性数据中心中断，或者基础组件或相关服务出现故障，Azure 媒体服务当前不提供服务的即时故障转移。 本文指导如何构建点播视频跨区域流式处理。

## <a name="prerequisites"></a>先决条件

查看[如何构建跨区域编码系统](media-services-high-availability-encoding.md)

## <a name="how-to-build-video-on-demand-cross-region-streaming"></a>如何构建点播视频跨区域流式处理 

* 点播视频跨区域流式处理包含复制[资产](assets-concept.md)、[内容密钥策略](content-key-policy-concept.md)（如果使用）、[流式处理策略](streaming-policy-concept.md)和[流式处理定位符](streaming-locators-concept.md)。 
* 需要在两个区域中创建策略，并使其保持最新状态。 
* 创建流式处理定位符时，需要使用相同的定位符 ID 值、ContentKey ID 值和 ContentKey 值。  
* 如果要对内容进行编码，建议对区域 A 中的内容进行编码并将其发布，然后将编码后的内容复制到区域 B 并使用与区域 A 相同的值发布该内容。
* 可在源服务器和密钥传递服务的主机名上使用流量管理器（在媒体服务中配置将类似于自定义密钥服务器 URL）。

## <a name="next-steps"></a>后续步骤

查看：

* [教程：基于 URL 对远程文件进行编码并流式传输视频](stream-files-dotnet-quickstart.md)
* [代码示例](https://docs.microsoft.com/samples/browse/?products=azure-media-services)
