---
title: 托管磁盘突发
description: 了解 Azure 磁盘的磁盘突发和 Azure 虚拟机的磁盘突发
author: rockboyfor
origin.date: 04/27/2020
ms.date: 05/18/2020
ms.author: v-yeche
ms.topic: conceptual
ms.service: virtual-machines
ms.subservice: disks
ms.openlocfilehash: 65c1624a206b6275fc02f02ae2e5add9d6746c6a
ms.sourcegitcommit: f6d0bd7958e0720367022e68dc1824448c866882
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/15/2020
ms.locfileid: "83417250"
---
<!--CHECK THE BURSTING FEATHER BEFORE RELEASE-->
<!--Verified successfully-->
# <a name="disk-bursting"></a>磁盘突发
[!INCLUDE [managed-disks-bursting](../../../includes/managed-disks-bursting.md)]

## <a name="virtual-machine-level-bursting"></a>虚拟机级别突发

<!--Lsv2 is available on two regions(China East 2 and China North 2)-->

目前，在 Azure 中国云的两个区域（中国东部 2 和中国北部 2）中，对下述受支持的大小启用了 VM 级别突发支持： 
- [Lsv2 系列](../lsv2-series.md)

默认情况下，对支持突发的虚拟机启用突发。

## <a name="disk-level-bursting"></a>磁盘级别突发
对于所有区域中大小为 P20 和更小的磁盘，在我们的[高级 SSD](disks-types.md#premium-ssd) 上也提供了突发功能。 在支持磁盘突发的磁盘大小的新部署上，默认已启用磁盘突发。 支持磁盘突发的现有磁盘大小可以通过以下任一方法启用突发： 
- **重启 VM** 
- **在分离磁盘后重新连接磁盘**

[!INCLUDE [managed-disks-bursting](../../../includes/managed-disks-bursting-2.md)]

<!-- Update_Description: update meta properties, wording update, update link -->