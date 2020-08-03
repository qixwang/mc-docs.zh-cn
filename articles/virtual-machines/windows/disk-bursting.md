---
title: 磁盘突发
description: 了解 Azure 磁盘的磁盘突发和 Azure 虚拟机的磁盘突发
author: rockboyfor
origin.date: 04/27/2020
ms.date: 08/03/2020
ms.author: v-yeche
ms.topic: conceptual
ms.service: virtual-machines
ms.subservice: disks
ms.openlocfilehash: e234ff204793568c798b683f66ba6398139e071f
ms.sourcegitcommit: 692b9bad6d8e4d3a8e81c73c49c8cf921e1955e7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/30/2020
ms.locfileid: "87426385"
---
# <a name="disk-bursting"></a>磁盘突发
[!INCLUDE [managed-disks-bursting](../../../includes/managed-disks-bursting.md)]

<!--Not Available on ## Virtual Machine level bursting-->

## <a name="disk-level-bursting"></a>磁盘级别突发
对于所有区域中大小为 P20 和更小的磁盘，在我们的[高级 SSD](disks-types.md#premium-ssd) 上也提供了突发功能。 在支持磁盘突发的磁盘大小的新部署上，默认已启用磁盘突发。 支持磁盘突发的现有磁盘大小可以通过以下任一方法启用突发： 
- **重启 VM** 
- **分离再重新附加磁盘**

[!INCLUDE [managed-disks-bursting](../../../includes/managed-disks-bursting-2.md)]

<!-- Update_Description: update meta properties, wording update, update link -->