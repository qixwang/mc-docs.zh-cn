---
title: include 文件
description: include 文件
services: digital-twins
ms.service: digital-twins
ms.topic: include
ms.date: 04/29/2020
author: deepakpalled
ms.author: v-junlch
manager: cshankar
ms.custom: include file
ms.openlocfilehash: 91a0306d96d85ef7a9c336efd78c306b714fb44d
ms.sourcegitcommit: 95efd248f5ee3701f671dbd5cfe0aec9c9959a24
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/29/2020
ms.locfileid: "82516024"
---
### <a name="general-availability-and-preview-comparison"></a>正式版和预览版比较

下表总结了 Azure 时序见解正式版 (GA) 和预览版实例之间的几个主要区别。

| | GA | 预览 |
| --- | --- | ---|
| 第一类公民 | 以事件为中心 | 以时序为中心 |
| 语义推理 | 低级（参考数据） | 高级（模型） |
| 数据上下文化 | 非设备级别 | 设备和非设备级别 |
| 计算逻辑存储 | 否 | 存储在模型的类型变量中 |
| 存储和访问控制 | 否 | 已通过模型启用 |
| 聚合/采样 | 否 | 事件加权和时间加权 |
| 信号重建 | 否 | 内插 |
| 生成衍生时序 | 否 | 是，合并和联接 |
| 语言灵活性 | 不可组合 | 可组合 |
| 表达式语言 | 谓词字符串 | 时序表达式（谓词字符串、值、表达式和函数） |

### <a name="property-limits"></a>属性限制

在正式版中，时序见解的属性限制已从最大上限 800 增加到 1,000。 提供的事件属性具有相应的 JSON、CSV 和图表列，可以在[时序见解预览版资源管理器](/time-series-insights/time-series-insights-update-quickstart)中查看。

| SKU | 最大属性数 |
| --- | --- |
| 预览 PAYG | 1,000 属性（列） |
| GA S1 | 600 属性（列） |
| GA S2 | 800 属性（列） |

### <a name="event-sources"></a>事件源


默认情况下，[预览版环境支持的入口速率](/time-series-insights/time-series-insights-update-storage-ingress#ingress-scale-and-preview-limitations)最大为每环境每秒 1 兆字节（MB/秒）  。 如果需要，客户可以将其预览版环境扩展到 16 MB/秒  吞吐量。 还存在每个分区 0.5 MB/秒  的限制。 

### <a name="api-limits"></a>API 限制

[REST API 参考文档](https://docs.microsoft.com/rest/api/time-series-insights/preview-query#limits)中指定了时序见解预览版的 REST API 限制。

