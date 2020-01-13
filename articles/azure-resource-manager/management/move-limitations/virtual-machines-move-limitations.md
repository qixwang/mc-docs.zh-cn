---
title: 将 Azure VM 移到新的订阅或资源组
description: 使用 Azure 资源管理器将虚拟机移到新的资源组或订阅。
ms.topic: conceptual
origin.date: 10/10/2019
ms.author: v-yeche
ms.date: 01/06/2020
ms.openlocfilehash: 837907e36fb8aa0be216199cebb5c5e998d8b9e7
ms.sourcegitcommit: 6fb55092f9e99cf7b27324c61f5fab7f579c37dc
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/03/2020
ms.locfileid: "75631534"
---
# <a name="move-guidance-for-virtual-machines"></a>针对虚拟机的移动指南

本文介绍当前不支持的方案以及移动使用备份的虚拟机的步骤。

## <a name="scenarios-not-supported"></a>不支持的方案

以下方案尚不受支持：

<!--Not Available on Availability Zones-->

* 无法移动具有标准 SKU 负载均衡器或标准 SKU 公共 IP 的虚拟机规模集。
* 无法跨资源组或订阅移动基于附加了计划的市场资源创建的虚拟机。 在当前订阅中取消预配虚拟机，并在新的订阅中重新部署虚拟机。
* 如果没有移动虚拟网络中的所有资源，则无法将现有虚拟网络中的虚拟机移到新订阅。
    
    <!--MOONCAKE: Not Available on Low priority-->
    
* 可用性集中的虚拟机不能单独移动。

## <a name="virtual-machines-with-azure-backup"></a>使用 Azure 备份的虚拟机

若要移动使用 Azure 备份配置的虚拟机，请使用以下解决方法：

* 找到虚拟机的位置。
* 找到含有以下命名模式的资源组：`AzureBackupRG_<location of your VM>_1` 例如，AzureBackupRG_chinanorth2_1
* 如果在 Azure 门户中，则查看“显示隐藏的类型”
* 如果在 PowerShell 中，则使用 `Get-AzResource -ResourceGroupName AzureBackupRG_<location of your VM>_1` cmdlet
* 如果在 CLI 中，则使用 `az resource list -g AzureBackupRG_<location of your VM>_1`
* 使用类型 `Microsoft.Compute/restorePointCollections` 找到具有命名模式 `AzureBackup_<name of your VM that you're trying to move>_###########` 的资源
* 删除此资源。 此操作仅删除即时恢复点，不删除保管库中的备份数据。
* 删除完成后，可以将虚拟机移到目标订阅。

    <!--MOONCAKE: CUSTOMIZED ON Not Available on  You can move the vault and virtual machine to the target subscription. After the move, you can continue backups with no loss in data.-->

* 若要了解如何移动恢复服务保管库以完成备份，请参阅[恢复服务限制](../../../backup/backup-azure-move-recovery-services-vault.md?toc=/azure-resource-manager/toc.json)。

## <a name="next-steps"></a>后续步骤

有关用于移动资源的命令，请参阅[将资源移到新资源组或订阅](../move-resource-group-and-subscription.md)。

<!-- Update_Description: update meta properties, wording update, update link -->
