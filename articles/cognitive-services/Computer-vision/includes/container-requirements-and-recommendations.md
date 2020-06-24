---
title: 容器要求和建议
titleSuffix: Azure Cognitive Services
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.topic: include
ms.date: 06/05/2020
ms.author: v-tawe
origin.date: 04/01/2020
ms.openlocfilehash: 5f0e24248e9fccb6861672e0d1dd5cf177df1663
ms.sourcegitcommit: 8dae792aefbe44e8388f961b813e3da6564423ec
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/10/2020
ms.locfileid: "84655015"
---
> [!NOTE]
> 这些要求和建议基于这样的基准：每秒一个请求，使用包含 29 行和总共 803 个字符的经过扫描的业务信函的 8 MB。

下表显示了每个读取容器的最小和建议的资源分配。

| 容器 | 最小值 | 建议 |TPS<br>(最小值, 最大值)|
|-----------|---------|-------------|--|
| 读取 | 单核, 8-GB 内存, 0.24 TPS | 8 核, 16-GB 内存, 1.17 TPS | 0.24, 1.17 |

* 每个核心必须至少为 2.6 千兆赫 (GHz) 或更快。
* TPS - 每秒事务数。

核心和内存对应于 `--cpus` 和 `--memory` 设置，用作 `docker run` 命令的一部分。
