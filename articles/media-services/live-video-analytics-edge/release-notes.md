---
title: IoT Edge 上的实时视频分析发行说明 - Azure
description: 本主题提供 IoT Edge 上的实时视频分析版本的发行说明、改进、bug 修复和已知问题。
ms.topic: conceptual
origin.date: 04/27/2020
ms.date: 07/27/2020
ms.openlocfilehash: 119d83eb3dc5183c8a89972f0a2f6c1820610fa9
ms.sourcegitcommit: 091c672fa448b556f4c2c3979e006102d423e9d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/24/2020
ms.locfileid: "87162768"
---
# <a name="live-video-analytics-on-iot-edge-release-notes"></a>IoT Edge 上的实时视频分析发行说明

本文提供以下事项的信息：

* 最新版本
* 已知问题
* Bug 修复
* 已弃用的功能

## <a name="june-1-2020"></a>2020 年 6 月 1 日

此版本是 IoT Edge 上的实时视频分析的第一个公共预览版本。 发行标记为

```
     mcr.microsoft.com/media/live-video-analytics:1.0.0
```

### <a name="supported-functionalities"></a>支持的功能
* 使用所选的 AI 模块分析实时视频流，还可以选择在边缘设备上或云中录制视频
* 在 IoT Edge [支持](/iot-edge/support)的 Linux AMD64 操作系统上使用
* 使用 Azure 门户或 Visual Studio Code 通过 IoT 中心部署和配置模块
* 通过以下直接方法调用来远程或本地管理[图形拓扑和图形实例](media-graph-concept.md#media-graph-topologies-and-instances)

    *   GraphTopologyGet
    *   GraphTopologySet
    *   GraphTopologyDelete
    *   GraphTopologyList
    *   GraphInstanceGet
    *   GraphInstanceSet
    *   GraphInstanceDelete
    *   GraphInstanceList


## <a name="next-steps"></a>后续步骤

[概述](overview.md)
