---
title: include 文件
description: include 文件
services: storage
author: WenJason
ms.service: storage
ms.topic: include
origin.date: 02/27/2020
ms.date: 03/09/2020
ms.author: v-jay
ms.custom: include file
ms.openlocfilehash: a9c1aa0c1d75a1cc8ad2005702a89d1a426b157f
ms.sourcegitcommit: c1ba5a62f30ac0a3acb337fb77431de6493e6096
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2020
ms.locfileid: "78411281"
---
下表介绍 Azure 常规用途 v1、v2 和 Blob 存储帐户的默认限制。 “传入”  限制是指请求中发送到存储帐户的所有数据。 “传出”  限制是指响应中从存储帐户接收的所有数据。

| 资源 | 默认限制 |
| --- | --- |
| 每个订阅每个区域的存储帐户数，包括标准帐户和高级帐户 | 250 |
| 最大存储帐户容量 | 5 PiB <sup>1</sup>|
| 每个存储帐户的 Blob 容器、Blob、文件共享、表、队列、实体或消息数目上限 | 无限制 |
| 每个存储帐户的最大请求速率<sup>1</sup> | 每秒 20,000 个请求 |
| 每个存储帐户的最大入口<sup>1</sup> | 如果已启用 RA-GRS/GRS，则为 5 Gbps；对于 LRS，为 10 Gbps<sup>2</sup> |
| 常规用途 v2 存储帐户和 Blob 存储帐户的最大流出量（所有区域） | 50 Gbps |
| 常规用途 v1 存储帐户的最大出口 | 如果已启用 RA-GRS/GRS，则为 10 Gbps；对于 LRS，为 15 Gbps<sup>2</sup> |
| 每个存储帐户的虚拟网络规则数目上限 | 200 |
| 每个存储帐户的 IP 地址规则数目上限 | 200 |

<sup>1</sup>Azure 存储标准帐户根据请求支持更高的容量上限和更高的流入量上限。 若要请求提高帐户入口上限，请与 [Azure 支持](https://support.azure.cn/zh-cn/support/contact)联系。

<sup>2</sup> 如果存储帐户启用了异地冗余存储 (RA-GRS) 的读取访问权限，则辅助位置的出口目标与主位置的出口目标相同。 [Azure 存储复制](/storage/common/storage-redundancy)选项包括：

[!INCLUDE [azure-storage-redundancy](azure-storage-redundancy.md)]

> [!NOTE]
> 大多数情况下，Azure 建议使用常规用途 v2 存储帐户。 可以轻松将常规用途 v1 或 Azure Blob 存储帐户升级到常规用途 v2 帐户，无需停机且无需复制数据。 有关详细信息，请参阅[升级到常规用途 v2 存储帐户](../articles/storage/common/storage-account-upgrade.md)。

如果应用程序的需求超过单个存储帐户的伸缩性目标，则可以构建使用多个存储帐户的应用程序。 然后，可以将数据对象分布到这些存储帐户中。 有关批量定价的信息，请参阅 [Azure 存储定价](https://azure.cn/pricing/details/storage/)。

所有存储帐户都在扁平网络拓扑上运行，无论它们创建于何时，都支持本文所述的可伸缩性和性能目标。 有关 Azure 存储的扁平网络体系结构和可伸缩性的详细信息，请参阅 [Azure Storage: A Highly Available Cloud Storage Service with Strong Consistency](https://blogs.msdn.com/b/windowsazurestorage/archive/2011/11/20/windows-azure-storage-a-highly-available-cloud-storage-service-with-strong-consistency.aspx)（Azure 存储：具有高度一致性的高可用云存储服务）。
