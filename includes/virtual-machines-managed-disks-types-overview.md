---
title: include 文件
description: include 文件
services: virtual-machines
author: rockboyfor
ms.service: virtual-machines
ms.topic: include
origin.date: 06/03/2020
ms.date: 08/10/2020
ms.testscope: no
ms.testdate: ''
ms.author: v-yeche
ms.custom: include file
ms.openlocfilehash: 10d064b412eb7fcfd6d6626df112580448832734
ms.sourcegitcommit: ac70b12de243a9949bf86b81b2576e595e55b2a6
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/07/2020
ms.locfileid: "87919275"
---
<!--MOONCAKE: CURRENT NO Ultra SSD-->

Azure 托管磁盘目前提供三种磁盘类型，每种类型都针对特定的客户方案。

<!--MOONCAKE: CURRENT NO Ultra SSD-->

## <a name="disk-comparison"></a>磁盘比较

下表对托管磁盘的高级固态硬盘 (SSD)、标准 SSD 和标准硬盘驱动器 (HDD) 进行了比较，方便你确定使用哪一种。

<!--Not Available on ultra solid-state-drives (SSD) (preview)-->

| 详细信息 | 高级 SSD | 标准 SSD | 标准 HDD |
| ------ | ----------- | ------------ | ------------ |
|磁盘类型   |SSD   |SSD   |HDD   |
|方案   |生产和性能敏感型工作负荷   |Web 服务器、不常使用的企业应用程序和开发/测试   |备份、非关键、不常访问   |
|最大磁盘大小   |32,767 GiB    |32,767 GiB   |32,767 GiB   |
|最大吞吐量   |900 MB/秒   |750 MB/秒   |500 MB/秒   |
|最大 IOPS   |20,000   |6,000   |2,000   |

<!--MOONCAKE: Disk size is less than 32,767 GiB-->
<!--Not Available on## Ultra SSD (preview)-->
