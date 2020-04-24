---
title: include 文件
description: include 文件
services: virtual-machines
author: rockboyfor
ms.service: virtual-machines
ms.topic: include
origin.date: 08/15/2019
ms.date: 11/11/2019
ms.author: v-yeche
ms.custom: include file
ms.openlocfilehash: 3b09e0701e8cacc060e563a6c98be57d13f113c8
ms.sourcegitcommit: c1ba5a62f30ac0a3acb337fb77431de6493e6096
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2020
ms.locfileid: "73831370"
---
# <a name="what-disk-types-are-available-in-azure"></a>Azure 有哪些可用的磁盘类型？

<!--MOONCAKE: CURRENT NO Ultra SSD-->

Azure 托管磁盘目前提供三种磁盘类型，每种类型都针对特定的客户方案。

<!--MOONCAKE: CURRENT NO Ultra SSD-->

## <a name="disk-comparison"></a>磁盘比较

下表对托管磁盘的高级固态硬盘 (SSD)、标准 SSD 和标准硬盘驱动器 (HDD) 进行了比较，方便你确定使用哪一种。

<!--Not Available on ultra solid-state-drives (SSD) (preview)-->

|    | 高级·SSD   | 标准 SSD   | 标准 HDD   |
|---------|---------|---------|---------|
|磁盘类型   |SSD   |SSD   |HDD   |
|场景   |生产和性能敏感型工作负荷   |Web 服务器、不常使用的企业应用程序和开发/测试   |备份、非关键、不常访问   |
|最大磁盘大小   | 32,767 GiB    |32,767 GiB   |32,767 GiB   |
|最大吞吐量   |900 MiB/秒   |750 MiB/秒   |500 MiB/秒   |
|最大 IOPS   |20,000   |6,000   |2,000   |

<!--MOONCAKE: Disk size is less than 32,767 GiB-->
<!--Not Available on## Ultra SSD (preview)-->
