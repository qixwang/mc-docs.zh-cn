---
title: include 文件
description: include 文件
services: virtual-machines
author: rockboyfor
ms.service: virtual-machines
ms.topic: include
origin.date: 03/28/2020
ms.date: 05/18/2020
ms.author: v-yeche
ms.custom: include file
ms.openlocfilehash: 86f012a034be2a9962280c98832b43c3a1fb3b41
ms.sourcegitcommit: 981a75a78f8cf74ab5a76f9e6b0dc5978387be4b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/22/2020
ms.locfileid: "83801116"
---
<!-- RELEASE CAREFULLY BEFORE CONFIGMING THE P1/P2/P3 IS AVAILABLE-->

| 高级 SSD 大小 | P1 | P2 | P3 | P4 | P6 | P10 | P15 | P20 | P30 | P40 | P50 | P60 | P70 | P80 |
|-------------------|----|----|----|----|----|-----|-----|-----|-----|-----|-----|------|------|------|
| 磁盘大小 (GiB) | 4 | 8 | 16 | 32 | 64 | 128 | 256 | 512 | 1,024 | 2,048 | 4,096 | 8,192 | 16,384 | 32,767 |
| 每个磁盘预配的 IOPS | 120 | 120 | 120 | 120 | 240 | 500 | 1,100 | 2,300 | 5,000 | 7,500 | 7,500 | 16,000 | 18,000 | 20,000 |
| 每个磁盘预配的吞吐量 | 25 MiB/秒 | 25 MiB/秒 | 25 MiB/秒 | 25 MiB/秒 | 50 MiB/秒 | 100 MiB/秒 | 125 MiB/秒 | 150 MiB/秒 | 200 MiB/秒 | 250 MiB/秒 | 250 MiB/秒| 500 MiB/秒 | 750 MiB/秒 | 900 MiB/秒 |
| 每个磁盘最大的突发 IOPS | 3,500 | 3,500 | 3,500 | 3,500 | 3,500 | 3,500 | 3,500 | 3,500 |
| 每个磁盘最大的突发吞吐量 | 170 MiB/秒 | 170 MiB/秒 | 170 MiB/秒 | 170 MiB/秒 | 170 MiB/秒 | 170 MiB/秒 | 170 MiB/秒 | 170 MiB/秒 |
| 最大突发持续时间 | 30 分钟  | 30 分钟  | 30 分钟  | 30 分钟  | 30 分钟  | 30 分钟  | 30 分钟  | 30 分钟  |
| 符合预留条件 | 否  | 否  | 否  | 否  | 否  | 否  | 否  | 否  | 是，最多一年 | 是，最多一年 | 是，最多一年 | 是，最多一年 | 是，最多一年 | 是，最多一年 |

<!--Not Available on P1,P2,P3-->