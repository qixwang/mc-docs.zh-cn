---
title: include 文件
description: include 文件
services: virtual-machines
author: Johnnytechn
ms.service: virtual-machines
ms.topic: include
ms.date: 05/18/2020
ms.author: v-johya
ms.custom: include file
ms.openlocfilehash: 5a1d0eabe0f7ce1d145ae76644389b9337a6a0f6
ms.sourcegitcommit: 8d56bc6baeb42d675695ecef1909d76f5c4a6ae3
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/14/2020
ms.locfileid: "83406192"
---
本文介绍如何使用 AzCopy 将 VHD 从本地计算机上传到 Azure 托管磁盘，或将托管磁盘复制到其他区域。 此过程（直接上传）还允许你将最大大小为 32 TiB 的 VHD 直接上传到托管磁盘。 目前，标准 HDD、标准 SSD 和高级 SSD 托管磁盘支持直接上传。

<!--Not Available on  It isn't supported for ultra disks, yet.-->

若要为 Azure 中的 IaaS VM 提供备份解决方案，建议使用直接上传方法将客户备份还原到托管磁盘。 从 Azure 外部的源上传 VHD 时，速度取决于本地带宽。 从 Azure VM 上传或复制时，带宽将与标准 HDD 相同。
