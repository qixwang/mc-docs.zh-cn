---
title: include 文件
description: include 文件
services: virtual-machines
author: Johnnytechn
ms.service: virtual-machines
ms.topic: include
ms.date: 04/13/2020
ms.author: v-johya
ms.custom: include file
ms.openlocfilehash: 50e958675801a286a5b7ebdd21bf88f0f7eedc0a
ms.sourcegitcommit: ebedf9e489f5218d4dda7468b669a601b3c02ae5
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/26/2020
ms.locfileid: "82159035"
---
本文介绍如何使用 AzCopy 将 VHD 从本地计算机上传到 Azure 托管磁盘，或将托管磁盘复制到其他区域。 此过程（直接上传）还允许你将最大大小为 32 TiB 的 VHD 直接上传到托管磁盘。 目前，标准 HDD、标准 SSD 和高级 SSD 托管磁盘支持直接上传。 超级磁盘尚不支持此功能。

若要为 Azure 中的 IaaS VM 提供备份解决方案，建议使用直接上传方法将客户备份还原到托管磁盘。 从 Azure 外部的源上传 VHD 时，速度取决于本地带宽。 从 Azure VM 上传或复制时，带宽将与标准 HDD 相同。
