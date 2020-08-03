---
title: include 文件
description: include 文件
services: virtual-machines
author: rockboyfor
ms.service: virtual-machines
ms.topic: include
origin.date: 04/27/2020
ms.date: 07/27/2020
ms.testscope: no
ms.testdate: ''
ms.author: v-yeche
ms.custom: include file
ms.openlocfilehash: b6e323e81c1c87530f7652b038bd692a30135a2f
ms.sourcegitcommit: 2b78a930265d5f0335a55f5d857643d265a0f3ba
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/28/2020
ms.locfileid: "87260236"
---
<!--CONFIRM THE DEOPLOMENT REGIONS BEFORE RELEASEMENT-->
<!--Verified successfully-->
在 Azure 上，我们提供了在虚拟机和磁盘上提升磁盘存储 IOPS 和 MB/秒性能的功能（称为突发）。 突发在许多情况下很有用，例如处理意外的磁盘流量或处理批处理作业。 可以有效地利用 VM 和磁盘级别的突发，以在 VM 和磁盘上实现良好的基线性能和突发性能。 这样，你就可以在 VM 和磁盘上都获得良好的基线性能和突发性能。 

请注意，磁盘和 VM 上的突发相互独立。 如果有突发磁盘，则不需要使用突发 VM 来允许磁盘突发。 如果有突发 VM，则不需要使用突发磁盘来允许 VM 突发。

<!-- Update_Description: update meta properties, wording update, update link -->