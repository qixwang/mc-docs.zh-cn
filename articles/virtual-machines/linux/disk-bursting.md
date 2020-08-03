---
title: 托管磁盘突发
description: 了解 Azure 磁盘的磁盘突发和 Azure 虚拟机的磁盘突发
author: Johnnytechn
ms.author: v-johya
ms.date: 06/17/2020
ms.topic: conceptual
ms.service: virtual-machines
ms.subservice: disks
ms.openlocfilehash: 601945e491cc36e5cd8782a36823be999b76e779
ms.sourcegitcommit: 692b9bad6d8e4d3a8e81c73c49c8cf921e1955e7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/30/2020
ms.locfileid: "87426479"
---
<!--CHECK THE BURSTING FEATHER BEFORE RELEASE-->
<!--Verified successfully-->
# <a name="disk-bursting"></a>磁盘突发
[!INCLUDE [managed-disks-bursting](../../../includes/managed-disks-bursting.md)]

<!--Not Availble on ## Virtual Machine level bursting-->

## <a name="disk-level-bursting"></a>磁盘级别突发
对于所有区域中大小为 P20 和更小的磁盘，在我们的[高级 SSD](disks-types.md#premium-ssd) 上也提供了突发功能。 在支持磁盘突发的磁盘大小的所有新的和现有的部署上，默认已启用磁盘突发。

[!INCLUDE [managed-disks-bursting](../../../includes/managed-disks-bursting-2.md)]

<!-- Update_Description: update meta properties, wording update, update link -->