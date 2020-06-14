---
title: 托管磁盘突发
description: 了解 Azure 磁盘的磁盘突发和 Azure 虚拟机的磁盘突发
author: Johnnytechn
ms.author: v-johya
ms.date: 06/05/2020
ms.topic: conceptual
ms.service: virtual-machines
ms.subservice: disks
ms.openlocfilehash: 6161d8c98be022a8edde986c925e4aa7b50645c4
ms.sourcegitcommit: 285649db9b21169f3136729c041e4d04d323229a
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/11/2020
ms.locfileid: "84683913"
---
<!--CHECK THE BURSTING FEATHER BEFORE RELEASE-->
<!--Verified successfully-->
# <a name="disk-bursting"></a>磁盘突发
[!INCLUDE [managed-disks-bursting](../../../includes/managed-disks-bursting.md)]

<!--Not Availble on ## Virtual Machine level bursting-->

<!--Lsv2 is available on two regions(China East 2 and China North 2)-->


## <a name="disk-level-bursting"></a>磁盘级别突发
对于所有区域中大小为 P20 和更小的磁盘，在我们的[高级 SSD](disks-types.md#premium-ssd) 上也提供了突发功能。 在支持磁盘突发的磁盘大小的新部署上，默认已启用磁盘突发。 支持磁盘突发的现有磁盘大小可以通过以下任一方法启用突发： 
- **重启 VM** 
- **在分离磁盘后重新连接磁盘**


[!INCLUDE [managed-disks-bursting](../../../includes/managed-disks-bursting-2.md)]

<!-- Update_Description: update meta properties, wording update, update link -->