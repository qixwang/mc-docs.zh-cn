---
title: 内容感知编码的试验性预设 - Azure | Microsoft Docs
description: 本文介绍 Microsoft Azure 媒体服务 v3 中的内容感知编码。
services: media-services
documentationcenter: ''
author: WenJason
manager: digimobile
editor: ''
ms.service: media-services
ms.workload: ''
ms.topic: article
origin.date: 04/05/2019
ms.date: 01/20/2020
ms.author: v-jay
ms.custom: ''
ms.openlocfilehash: 69384d888a7872884fc750c1f20536f2ba151fe7
ms.sourcegitcommit: 779d674e865b23ae417eb492efca7508675b8ba6
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/14/2020
ms.locfileid: "75939816"
---
# <a name="experimental-preset-for-content-aware-encoding"></a>内容感知编码的试验性预设

若要准备通过[自适应比特率流式处理](https://en.wikipedia.org/wiki/Adaptive_bitrate_streaming)传送的内容，需以多个比特率（从高到低）编码视频。 确保质量的平稳降级，因为比特率会降低，而视频的分辨率也会降低。 这会造成所谓的编码梯度 – 分辨率和比特率的表；请参阅媒体服务的[内置编码预设](https://docs.microsoft.com/rest/api/media/transforms/createorupdate#encodernamedpreset)。

## <a name="overview"></a>概述

Netflix 在 2015 年 12 月发布其[博客](https://medium.com/netflix-techblog/per-title-encode-optimization-7e99442b62a2)后，人们对超越“一个预设符合所有视频”方法的热情有所增长。 从那以后，市场中已经发布了多个内容感知编码解决方案；有关概述，请参阅[此文](https://www.streamingmedia.com/Articles/Editorial/Featured-Articles/Buyers-Guide-to-Per-Title-Encoding-130676.aspx)。 思路就是感知内容 - 根据单个视频的复杂性自定义或优化编码梯度。 在每种分辨率下有一个比特率，超过该比特率会感知不到任何质量提升 – 编码器将以此最佳比特率运行。 下一级优化是基于内容选择分辨率 – 例如，降到 720p 以下不会使 PowerPoint 演示文稿的视频受益。 接下来，可以进一步在编码器中优化视频中每个拍摄画面的设置。 Netflix 在 2018 年介绍了[这种方法](https://medium.com/netflix-techblog/optimized-shot-based-encodes-now-streaming-4b9464204830)。

在 2017 年上旬，Microsoft 已发布[自适应流式处理](autogen-bitrate-ladder.md)预设，以解决源视频的质量和分辨率可变性的问题。 我们的客户混合使用不同的内容，其中某些内容的分辨率为 1080p，有些为 720p，还有少部分为标清或更低分辨率。 此外，并非所有源内容都是电影或电视演播室提供的高质量夹层文件。 自适应流式处理预设解决了这些问题，它可以确保比特率梯度永不超过输入夹层文件的分辨率或平均比特率。

试验性内容感知编码预设扩展了该机制，它可以整合自定义逻辑，使编码器能够查找给定分辨率的最佳比特率值，但不需要进行大量的计算分析。 最终结果是，此新预设将会生成一个比特率低于自适应流式处理预设、但质量更高的输出。 请参阅以下示例图形，其中显示了使用 [PSNR](https://en.wikipedia.org/wiki/Peak_signal-to-noise_ratio) 和 [VMAF](https://en.wikipedia.org/wiki/Video_Multimethod_Assessment_Fusion) 等质量指标做出的比较。 源的创建方式是将电影和电视节目中的高复杂性拍摄画面的简短剪辑相连接，旨在给编码器提供压力。 按照定义，此预设产生的结果因内容而异 – 这也意味着，对于某些内容，比特率可能不会显著降低，或者质量不会显著提高。

![使用 PSNR 的速率失真 (RD) 曲线](media/cae-experimental/msrv1.png)

图 1：**使用高复杂性源的 PSNR 指标的速率失真 (RD) 曲线**

![使用 VMAF 的速率失真 (RD) 曲线](media/cae-experimental/msrv2.png)

图 2：**使用高复杂性源的 VMAF 指标的速率失真 (RD) 曲线**

该预先目前已针对高复杂性、高质量源视频（电影、电视节目）进行优化。 我们目前正在努力针对低复杂性内容（例如 PowerPoint 演示文稿）和质量较差的视频进行优化。 此预设也使用与自适应流式处理预设相同的一组分辨率。 Azure 正在开发相应的方法，以根据内容选择最少量的一组分辨率。 下面是另一个源内容类别的结果，其中，编码器可以确定输入质量较差（由于比特率较低，因此生成了很多压缩项目）。 请注意，使用试验性预设时，编码器确定只生成一个输出层 – 该层的比特率足够低，使大多数客户端能够播放流，而不会出现停顿。

![使用 PSNR 的 RD 曲线](media/cae-experimental/msrv3.png)

图 3：**使用低质量输入的 PSNR 的 RD 曲线（分辨率为 1080p）**

![使用 VMAF 的 RD 曲线](media/cae-experimental/msrv4.png)

图 4：**使用低质量输入的 VMAF 的 RD 曲线（分辨率为 1080p）**

## <a name="use-the-experimental-preset"></a>使用试验性预设

可按如下所示创建使用此预设的转换。 如果使用了[此类](stream-files-tutorial-with-api.md)教程，可按如下所示更新代码：

```csharp
TransformOutput[] output = new TransformOutput[]
{
   new TransformOutput
   {
      // The preset for the Transform is set to one of Media Services built-in sample presets.
      // You can customize the encoding settings by changing this to use "StandardEncoderPreset" class.
      Preset = new BuiltInStandardEncoderPreset()
      {
         // This sample uses the new experimental preset for content-aware encoding
         PresetName = EncoderNamedPreset.ContentAwareEncodingExperimental
      }
   }
};
```

> [!NOTE]
> 此处使用的前置词“试验性”表示底层算法仍在演进。 随着时间的推移，用于生成比特率梯度的逻辑势必会发生变化，目的是融合某种可靠的算法，并适应各种输入条件。 使用此预设的编码作业仍根据输出分钟数计费，输出资产可以从 DASH 和 HLS 等协议中的流式处理终结点传送。

