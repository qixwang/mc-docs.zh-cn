---
title: 将 Azure VM 迁移到托管磁盘
description: 迁移使用存储帐户中的非托管磁盘创建的 Azure 虚拟机以使用托管磁盘。
author: rockboyfor
ms.service: virtual-machines-windows
ms.topic: how-to
origin.date: 05/30/2019
ms.date: 07/06/2020
ms.author: v-yeche
ms.subservice: disks
ms.openlocfilehash: a6309276c2041a132fb9da327f22caaea59b4840
ms.sourcegitcommit: 89118b7c897e2d731b87e25641dc0c1bf32acbde
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/03/2020
ms.locfileid: "85945599"
---
# <a name="migrate-azure-vms-to-managed-disks-in-azure"></a>将 Azure VM 迁移到 Azure 中的托管磁盘

Azure 托管磁盘无需单独管理存储帐户，从而简化了存储管理。  还可以将现有的 Azure VM 迁移到托管磁盘，以便受益于可用性集中 VM 的更佳可靠性。 这可确保将可用性集中不同 VM 的磁盘最大限度地彼此独立，以避免单一故障点。 它会自动将可用性集中不同 VM 的磁盘置于不同的存储缩放单元（戳），限制由于硬件和软件故障引起的单个存储缩放单元故障影响。
可根据需要，从存储选项的四种类型中进行选择。 若要了解可用的磁盘类型，请参阅[选择磁盘类型](disks-types.md)一文

## <a name="migration-scenarios"></a>迁移方案

可以在以下方案中迁移到托管磁盘：

|方案  |文章  |
|---------|---------|
|将可用性集中的独立 VM 和多个 VM 转换为托管磁盘     |[转换 VM 以使用托管磁盘](convert-unmanaged-to-managed-disks.md)         |
|将托管磁盘上的单个 VM 从经典部署模型转换为资源管理器部署模型     |[从经典 VHD 创建 VM](create-vm-specialized-portal.md)         |
|将 vNet 中的所有 VM 从经典部署模型转换为托管磁盘上的资源管理器部署模型     |[将 IaaS 资源从经典迁移到 Resource Manager](migration-classic-resource-manager-ps.md)，然后[将 VM 从非托管磁盘转换为托管磁盘](convert-unmanaged-to-managed-disks.md)         |
|将具有标准非托管磁盘的 VM 升级到具有托管高级磁盘的 VM     | 首先，[将 Windows 虚拟机从非托管磁盘转换为托管磁盘](convert-unmanaged-to-managed-disks.md)。 然后[更新托管磁盘的存储类型](convert-disk-storage.md)。         |

[!INCLUDE [classic-vm-deprecation](../../../includes/classic-vm-deprecation.md)]

## <a name="next-steps"></a>后续步骤

- 详细了解[托管磁盘](managed-disks-overview.md)
- 查看[托管磁盘定价](https://www.azure.cn/pricing/details/storage/managed-disks/)。

<!-- Update_Description: update meta properties, wording update, update link -->