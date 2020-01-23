---
title: 参考：DSVM 映像的弃用
description: 有关影响 Azure Data Science Virtual Machine (DSVM) 的弃用的详细信息
author: gvashishtha
ms.service: machine-learning
ms.subservice: data-science-vm
ms.author: gopalv
ms.date: 10/14/2019
ms.topic: reference
ms.openlocfilehash: 4b48e09674fc81cbc04f8a2cbcd4d395062fd1ab
ms.sourcegitcommit: 623d64ef33e80d5f84b6dcf6d1ef4120fe4b8c08
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/02/2020
ms.locfileid: "75597992"
---
# <a name="reference-deprecation-of-dsvm-images"></a>参考：DSVM 映像的弃用

下文将讨论有关如何处理 Azure Data Science Virtual Machine 上即将发布的弃用的建议。

## <a name="windows-2012-migrating-data-disks"></a>Windows 2012：迁移数据磁盘

我们将于 2019 年 12 月 31 日停止支持 Windows 2012 DSVM 映像。 若要将数据磁盘从现有的 Windows 2012 DSVM 迁移到 Windows 2016 DSVM，请执行以下步骤：

1. 按照[此处](./provision-vm.md#create-your-dsvm)显示的说明，创建新的 Windows 2016 DSVM。
1. 根据[这些说明](../../virtual-machines/windows/detach-disk.md)，将现有数据磁盘从 Windows 2012 映像中拆离。
1. 根据[这些说明](../../virtual-machines/windows/attach-disk-ps.md#attach-an-existing-data-disk-to-a-vm)，将上一步骤中的磁盘附加到 Windows 2016 映像。
