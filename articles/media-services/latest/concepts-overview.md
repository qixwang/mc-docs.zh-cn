---
title: Azure 媒体服务的术语和概念 - Azure | Microsoft Docs
description: 本主题简要概述 Azure 媒体服务的术语和概念，并提供更多详细信息的链接。
services: media-services
documentationcenter: ''
author: WenJason
manager: digimobile
editor: ''
ms.service: media-services
ms.workload: ''
ms.topic: article
origin.date: 09/10/2019
ms.date: 03/04/2020
ms.author: v-jay
ms.custom: seodec18
ms.openlocfilehash: cc96b5087f85c19f38fb11a5b1449fc44607ca85
ms.sourcegitcommit: fbc7584f403417d3af7bd6bbbaed7c13a78c57b9
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/06/2020
ms.locfileid: "78412686"
---
# <a name="media-services-concepts"></a>媒体服务的概念

> [!NOTE]
> Google Widevine 内容保护服务目前在 Azure 中国区域不可用。

本主题简要概述 Azure 媒体服务的术语和概念。 本文还会提供深入介绍媒体服务 v3 概念和功能的文章的链接。 

在开始开发之前，应该复习这些主题中所述的基本概念。

> [!NOTE]
> 目前，无法使用 Azure 门户来管理 v3 资源。 请使用 [REST API](https://aka.ms/ams-v3-rest-ref)、[CLI](/cli/ams?view=azure-cli-latest) 或受支持的 [SDK](media-services-apis-overview.md#sdks) 之一。

## <a name="terminology"></a>术语

本部分介绍某些常见行业术语如何对应于媒体服务 v3 API 中的术语。

### <a name="live-event"></a>实时事件

**实时事件**表示用于引入、转码（可选）以及打包视频、音频和实时元数据的管道。

对于从媒体服务 v2 API 迁移的客户，**实时事件**取代了 v2 中的**频道**实体。 有关详细信息，请参阅[从 v2 迁移到 v3](migrate-from-v2-to-v3.md)。

### <a name="streaming-endpoint-packaging-and-origin"></a>流式处理终结点（打包和来源）

**流式处理终结点**表示动态（实时）打包和源服务，该服务可使用一个常见流式处理媒体协议（HLS 或 DASH）直接将实时和按需内容发送到客户端播放器应用程序。 此外，**流式处理终结点**为行业领先的 DRM 提供动态（实时）加密。

在媒体流行业，此服务通常称为**打包器**或**来源**。  本行业对此功能使用的其他常见术语包括 JITP（实时打包器）或 JITE（实时加密）。 
 
## <a name="cloud-upload-and-storage"></a>云上传和存储

若要开始管理、加密、编码、分析和流式处理 Azure 中的媒体内容，需要创建一个媒体服务帐户，并将数字文件上传到**资产**中。

- [云上传和存储](storage-account-concept.md)
- [资产的概念](assets-concept.md)

## <a name="encoding"></a>编码

将优质数字媒体文件上传到资产中后，可将其编码为可在各种浏览器和设备上播放的格式。 

若要使用媒体服务 v3 进行编码，需要创建**转换**和**作业**。

![转换](./media/encoding/transforms-jobs.png)

- [转换和作业](transforms-jobs-concept.md)
- [使用媒体服务进行编码](encoding-concept.md)

## <a name="media-analytics"></a>媒体分析

若要分析视频和音频文件，也需要创建**转换**和**作业**。

- [分析视频和音频文件](analyzing-video-audio-files-concept.md)

## <a name="packaging-delivery-protection"></a>打包、传送、保护

将内容编码后，可以利用**动态打包**。 在媒体服务中，**流式处理终结点**/来源是用于将媒体内容传送到客户端播放器的动态打包服务。 若要使输出资产中的视频可供客户端进行播放，必须创建**流定位符**，然后生成流 URL。 

创建**流定位符**时，除了资产名称之外，还需要指定**流策略**。 使用**流策略**可为**流定位符**定义流式处理协议和加密选项（如果有）。

无论流式传输的是直播内容还是点播内容，都要使用动态打包。 下图演示了如何使用动态打包工作流流式传输点播内容。

![动态打包](./media/dynamic-packaging-overview/media-services-dynamic-packaging.svg)

借助媒体服务，可以传送使用高级加密标准 (AES-128) 或/和以下两个主要数字版权管理 (DRM) 系统中任意一个动态加密的直播和点播内容：Microsoft PlayReady 和 Apple FairPlay。 媒体服务还提供了用于向已授权客户端传送 AES 密钥和 DRM（PlayReady、Widevine 和 FairPlay）许可证的服务。

若要针对流指定加密选项，请创建**内容密钥策略**并将其与**流定位符**相关联。 使用**内容密钥策略**，可以配置如何将内容密钥传送到终端客户端。

下图阐释了媒体服务内容保护工作流： 

![保护内容](./media/content-protection/content-protection.svg)

&#42; 动态加密支持 AES-128“明文密钥”、CBCS 和 CENC。 

可以使用媒体服务**动态清单**来仅流式传输视频的特定再现内容或子剪辑。 以下示例使用编码器将夹层资产编码成七个 ISO MP4 视频再现内容（从 180p 到 1080p）。 编码的资产可动态打包成以下任一流协议：HLS、MPEG DASH 和平滑流式处理。  图表顶部显示了不包含筛选器的资产的 HLS 清单（包含全部七个再现内容）。  左下角显示名为“ott”的筛选器已应用到 HLS 清单。 “ott”筛选器指定要删除所有不低于 1 Mbps 的比特率，因此将最差的两个质量级别从响应中剥除。 右下角显示名为“mobile”的筛选器已应用到 HLS 清单。 “mobile”筛选器指定删除分辨率大于 720p 的再现内容，因此会剥除两个 1080p 再现内容。

![再现内容筛选](./media/filters-dynamic-manifest-overview/media-services-rendition-filter.png)

- [动态打包](dynamic-packaging-overview.md)
- [流式处理终结点](streaming-endpoint-concept.md)
- [流式处理定位符](streaming-locators-concept.md)
- [流式处理策略](streaming-policy-concept.md)
- [内容密钥策略](content-key-policy-concept.md)
- [内容保护](content-protection-overview.md)
- [动态清单](filters-dynamic-manifest-overview.md)
- [筛选器](filters-concept.md)

## <a name="live-streaming"></a>实时传送视频流

使用 Azure 媒体服务可将直播活动传送到 Azure 云中的客户。 **直播活动**负责引入和处理实时视频源。 创建**实时事件**时，会创建一个输入终结点，可以使用它来从远程编码器发送实时信号。 将流传输到**实时事件**后，可以通过创建**资产**、**实时输出**和**流定位符**来启动流事件。 **实时输出**会将流存档到**资产**中，使观看者可通过**流式处理终结点**使用该流。 **实时事件**可以是下述两种类型之一：**直通**和**实时编码**。

下图演示了直通类型的工作流：

![直通](./media/live-streaming/pass-through.svg)

- [实时传送视频流概述](live-streaming-overview.md)
- [直播活动和实时输出](live-events-outputs-concept.md)

## <a name="player-clients"></a>播放器客户端

可以在各种浏览器和设备上使用 Azure Media Player 播放媒体服务流式传输的媒体内容。 Azure Media Player 采用行业标准（如 HTML5、媒体源扩展 (MSE) 和加密媒体扩展插件 (EME)）来提供更丰富的自适应流式处理体验。 

- [Azure Media Player 概述](use-azure-media-player.md)

## <a name="next-steps"></a>后续步骤

* [对远程文件和流视频进行编码 - REST](stream-files-tutorial-with-rest.md)
* [对上传的文件和流视频进行编码 - .NET](stream-files-tutorial-with-api.md)
* [实时流 - .NET](stream-live-tutorial-with-api.md)
* [分析视频 - .NET](analyze-videos-tutorial-with-api.md)
* [AES-128 动态加密 - .NET](protect-with-aes128.md)
* [通过多重 DRM 进行动态加密 - .NET](protect-with-drm.md) 
