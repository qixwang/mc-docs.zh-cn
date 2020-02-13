---
title: 用于实现跨区域持续性的异地冗余存储 (GRS)
titleSuffix: Azure Storage
description: 异地冗余存储 (GRS) 在相距数百英里的两个区域之间复制数据。 GRS 针对数据中心内的硬件故障以及区域性灾难提供保护。
services: storage
author: WenJason
ms.service: storage
ms.topic: conceptual
origin.date: 01/02/2020
ms.date: 02/10/2020
ms.author: v-jay
ms.reviewer: artek
ms.subservice: common
ms.openlocfilehash: f4417a238b4478a423135e16cd633fd19cf371b1
ms.sourcegitcommit: 5c4141f30975f504afc85299e70dfa2abd92bea1
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/05/2020
ms.locfileid: "77029000"
---
# <a name="geo-redundant-storage-grs-cross-regional-replication-for-azure-storage"></a>异地冗余存储 (GRS)：Azure 存储的跨区域复制
[!INCLUDE [storage-common-redundancy-GRS](../../../includes/storage-common-redundancy-grs.md)]

## <a name="read-access-geo-redundant-storage"></a>读取访问异地冗余存储
读取访问异地冗余存储 (RA-GRS) 可最大程度提高存储帐户的可用性。 除了跨两个区域的异地复制之外，RA-GRS 提供对次要位置中的数据的只读访问。

当启用对次要区域中的数据的只读访问权限时，可以在存储帐户的辅助终结点以及主终结点上获取数据。 辅助终结点与主终结点类似，但会在帐户名称后面追加后缀 `-secondary` 。 例如，如果 Blob 服务的主终结点是 `myaccount.blob.core.chinacloudapi.cn`，则辅助终结点是 `myaccount-secondary.blob.core.chinacloudapi.cn`。 存储帐户的访问密钥对于主终结点和辅助终结点是相同的。

使用 RA-GRS 时需要注意一些注意事项：

- 应用程序必须管理在使用 RA-GRS 时要与哪些终结点进行交互。
- 由于异步复制涉及延迟，因此如果无法将数据从主要区域中恢复，则尚未复制到次要区域的更改可能会丢失。
- 可以检查存储帐户的“上次同步时间”  属性。 **上次同步时间**是 GMT 日期/时间值。 在**上次同步时间**之前进行的所有主要位置写入均已成功写入辅助位置，这意味着可以从辅助位置读取这些写入内容。 在**上次同步时间**之后进行的主要位置写入尚不一定可供读取。 可以使用 PowerShell、Azure CLI 或某个 Azure 存储客户端库查询此值。 有关详细信息，请参阅[使用读取访问异地冗余存储设计高度可用的应用程序](storage-designing-ha-apps-with-ragrs.md#getting-the-last-sync-time)中的**获取上次同步时间**。
- 如果 Azure 启动了到次要区域的故障转移，则在故障转移完成以后，将恢复你对该帐户的读写访问权限。 有关详细信息，请参阅[灾难恢复指南](storage-disaster-recovery-guidance.md)。
- 要实现高可用性时使用 RA-GRS。 有关可伸缩性的指南，请查看[性能清单](storage-performance-checklist.md)。
- 有关如何使用 RA-GRS 进行高可用性设计的建议，请参阅[使用 RA-GRS 存储设计高可用性应用程序](storage-designing-ha-apps-with-ragrs.md)。

## <a name="what-is-the-rpo-and-rto-with-grs"></a>什么是采用 GRS 的 RPO 和 RTO？

**恢复点目标 (RPO)：** 在 GRS 和 RA-GRS 中，存储服务以异步方式将数据从主位置异地复制到辅助位置。 在主要区域发生故障时，Azure 会故障转移到次要区域。 如果发生故障转移，则尚未进行异地复制的最近更改可能会丢失。 丢失的潜在数据的分钟数称为 RPO。 RPO 指示可以将数据恢复到的时间点。 Azure 存储的 RPO 通常小于 15 分钟，但目前没有 SLA 规定异地复制所用时长。

**恢复时间目标 (RTO)：** RTO 用于度量执行故障转移以及将存储帐户恢复联机所花费的时间。 执行故障转移的时间包括以下操作：

   * Azure 确定是否可以在主要位置恢复数据或是否需要故障转移所需的时间
   * Azure 通过将主要 DNS 条目更改为指向次要位置来执行故障转移所需的时间

## <a name="paired-regions"></a>配对区域 
创建存储帐户时，可以为帐户选择主要区域。 配对的次要区域是根据主要区域确定的且无法更改。

## <a name="see-also"></a>另请参阅
- [Azure 存储复制](storage-redundancy.md)
- [本地冗余存储 (LRS)：Azure 存储的低成本数据冗余](storage-redundancy-lrs.md)
