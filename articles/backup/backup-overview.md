---
title: 什么是 Azure 备份？
description: 概述 Azure 备份服务及其如何有助于实现业务连续性和灾难恢复 (BCDR) 策略。
ms.topic: overview
author: lingliw
origin.date: 04/24/2019
ms.date: 09/23/2019
ms.author: v-lingwu
ms.custom: mvc
ms.openlocfilehash: 8a4a329f64ee0ae6f57e9218804f51796dfe3d7d
ms.sourcegitcommit: 5c4141f30975f504afc85299e70dfa2abd92bea1
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/05/2020
ms.locfileid: "77028569"
---
# <a name="what-is-the-azure-backup-service"></a>什么是 Azure 备份服务？

Azure 备份服务提供简单、安全且经济高效的解决方案来备份数据，并从 Microsoft Azure 云恢复数据。

## <a name="why-use-azure-backup"></a>为何使用 Azure 备份？

Azure 备份具有以下主要优势：

- **卸载本地备份**：Azure 备份提供一个简单的解决方案，可以将本地资源备份到云。 获取短期和长期备份，不需部署复杂的本地备份解决方案。
- **备份 Azure IaaS VM**：Azure 备份提供独立且隔离的备份，可以防范原始数据的意外破坏。 备份存储在可以对恢复点进行内置托管的恢复服务保管库中。 配置和可伸缩性很简单，备份经过优化，可以轻松地根据需要还原。
- **轻松缩放** - Azure 备份利用 Azure 云的基础功能和无限缩放功能实现高可用性 - 无需维护，也无需监视开销。
- **无限数据传输**：Azure 备份不会限制传输的入站或出站数据量，不会对传输的数据收费。
  - 出站数据是指还原操作期间从恢复服务保管库传输的数据。
  - 如果使用 Azure 导入/导出服务执行脱机初始备份以导入大量数据，则入站数据将产生相关费用。  [了解详细信息](backup-azure-backup-import-export.md)。
- **保护数据安全**：Azure 备份为保护传输中数据和静态数据提供解决方案。
- **获取应用一致性备份**：应用程序一致性备份意味着恢复点包含还原备份副本所需的所有数据。 Azure 备份提供了应用程序一致性备份，确保了还原数据时无需额外的修补程序。 还原应用程序一致型数据可减少还原时间，因此可快速恢复到运行状态。
- **保留短期和长期数据**：可将恢复服务保管库用于短期和长期数据保留。 Azure 不会限制恢复服务保管库中数据的保留时间长度。 可将数据保留任意时间。 Azure 备份的限制为每个受保护实例仅限 9999 个恢复点。 
- **自动存储管理** - 混合环境常常需要异类存储（部分在本地，部分在云）。 通过 Azure 备份，使用本地存储设备时无需付费。 Azure 备份会自动分配和管理备份存储，且采用即用即付模型，因此，你只需为消耗的存储付费。 [详细了解](https://www.azure.cn/pricing/details/backup)定价情况。
- **多个存储选项** - Azure 备份提供两种类型的复制来保持存储/数据的高可用性。
    - [本地冗余存储 (LRS)](../storage/common/storage-redundancy-lrs.md) 将数据中心的存储缩放单元中的数据复制三次（创建三个数据副本）。 数据的所有副本存在于同一区域。 LRS 是一个低成本选项，可在本地硬件故障时保护数据。
    - [异地冗余存储 (GRS)](../storage/common/storage-redundancy-grs.md) 是默认的和推荐的复制选项。 GRS 将数据复制到离源数据主位置数英里之外的次要区域中。 GRS 的成本比 LRS 的高，但 GRS 可让数据更为持久，即使出现区域性中断也是如此。

## <a name="next-steps"></a>后续步骤

- [查看](backup-architecture.md)不同备份方案的体系结构和组件。
- [验证](backup-support-matrix.md)对备份和 [Azure VM 备份](backup-support-matrix-iaas.md)的支持要求和限制。

