---
title: include 文件
description: include 文件
services: virtual-machines
author: rockboyfor
ms.service: virtual-machines
ms.topic: include
origin.date: 03/05/2020
ms.date: 06/15/2020
ms.author: v-yeche
ms.custom: include file
ms.openlocfilehash: a47530bcc336a2a62f9b3cd9d8a27f49472e49a5
ms.sourcegitcommit: c4fc01b7451951ef7a9616fca494e1baf29db714
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/09/2020
ms.locfileid: "84574610"
---
<!--Verified successfully by PG team-->
增量快照是托管磁盘的时间点备份，拍摄后，这些备份仅包含自上次快照以来的所有更改。 尝试下载或以其他方式使用增量快照时，将使用完整的 VHD。 托管磁盘快照的这种新功能可能使它们更具成本效益，因为你不必在拍摄单个快照时存储整个磁盘，除非你选择这么做。 与常规快照一样，增量快照可用于创建完整的托管磁盘，也可用于制作常规快照。

增量快照和常规快照之间存在一些差异。 增量快照始终使用标准 HDD 存储，而不管磁盘的存储类型如何，而常规快照可以使用高级 SSD。 如果在高级存储上使用常规快照来横向扩展 VM 部署，建议在[共享映像库](../articles/virtual-machines/linux/shared-image-galleries.md)中的标准存储上使用自定义映像。 它将帮助你以更低的成本实现更大的规模。

<!--Not Available on [zone-redundant storage](../articles/storage/common/storage-redundancy-zrs.md)-->
<!--Not Available on  Additionally, incremental snapshots potentially offer better reliability with zone-redundant storage(ZRS). If ZRS is available in the selected region, an incremental snapshot will use ZRS automatically. If ZRS is not available in the region, then the snapshot will default to locally-redundant storage (LRS). You can override this behavior and select one manually but, we do not recommend that.-->

增量快照还提供差异功能，但仅适用于托管磁盘。 它们用于获取同一托管磁盘的两个增量快照之间的差异，可适用至块级别。 在跨区域复制快照时，可以使用此功能来减少数据占用空间。  例如，可以将第一个增量快照下载为另一个区域中的基本 Blob。 对于后续增量快照，只需要将自上次快照以来的更改复制到基本 Blob 中。 复制更改后，可以在基本 Blob 上拍摄表示另一个区域中磁盘的时间点备份的快照。 可以从基本 Blob 或从其他区域中的基本 Blob 上的快照还原磁盘。

:::image type="content" source="media/virtual-machines-disks-incremental-snapshots-description/incremental-snapshot-diagram.png" alt-text="描述跨区域复制的增量快照的图表。快照会执行各种 API 调用，直到最终形成每个快照的页 blob。":::

<!--Not Available on [Azure usage report](/billing/billing-understand-your-bill)-->

<!-- Update_Description: new article about virtual machines disks incremental snapshots description -->
<!--NEW.date: 06/15/2020-->