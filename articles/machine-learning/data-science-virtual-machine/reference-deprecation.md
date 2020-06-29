---
title: 参考：Data Science Virtual Machine 映像弃用
titleSuffix: Azure Data Science Virtual Machine
description: 有关影响 Azure Data Science Virtual Machine 的弃用的详细信息
author: lobrien
ms.service: machine-learning
ms.subservice: data-science-vm
ms.author: gopalv
origin.date: 04/03/2020
ms.date: 06/29/2020
ms.topic: reference
ms.openlocfilehash: ca312eee906720c5e0d749c8f0596f6c8571bf27
ms.sourcegitcommit: 1c01c98a2a42a7555d756569101a85e3245732fd
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/19/2020
ms.locfileid: "85097244"
---
# <a name="reference-deprecation-of-dsvm-images"></a>参考：DSVM 映像的弃用

下文将讨论有关如何处理 Azure Data Science Virtual Machine 上即将发布的弃用的建议。

## <a name="windows-2012-migrating-data-disks"></a>Windows 2012：迁移数据磁盘

我们将于 2019 年 12 月 31 日停止支持 Windows 2012 DSVM 映像。 若要将数据磁盘从现有的 Windows 2012 DSVM 迁移到 Windows 2016 DSVM，请执行以下步骤：

1. 按照[此处](./provision-vm.md#create-your-dsvm)显示的说明，创建新的 Windows 2016 DSVM。
1. 根据[这些说明](../../virtual-machines/windows/detach-disk.md)，将现有数据磁盘从 Windows 2012 映像中拆离。
1. 根据[这些说明](../../virtual-machines/windows/attach-disk-ps.md#attach-an-existing-data-disk-to-a-vm)，将上一步骤中的磁盘附加到 Windows 2016 映像。

## <a name="centos"></a>CentOS

新用户应使用最新的 Ubuntu 或 Windows 映像。 CentOS 仍可与现有解决方案模板一起使用。