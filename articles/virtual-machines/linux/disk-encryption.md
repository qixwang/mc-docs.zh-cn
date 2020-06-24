---
title: Azure 托管磁盘的服务器端加密
description: Azure 存储在将数据保存到存储群集之前会对其进行静态加密，以此保护数据。 可以使用 Microsoft 托管密钥加密你的托管磁盘，也可以使用客户托管密钥以通过自己的密钥管理所做的加密。
author: Johnnytechn
ms.date: 06/05/2020
ms.topic: conceptual
ms.author: v-johya
ms.service: virtual-machines-linux
ms.subservice: disks
ms.openlocfilehash: 6f19188b3b323aba4fc1214115cbe58af151f88c
ms.sourcegitcommit: 285649db9b21169f3136729c041e4d04d323229a
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/11/2020
ms.locfileid: "84684043"
---
# <a name="server-side-encryption-of-azure-managed-disks"></a>Azure 托管磁盘的服务器端加密

默认情况下，在将数据保存到云时，Azure 托管磁盘会自动加密数据。 服务器端加密 (SSE) 可保护数据，并帮助实现组织安全性和合规性承诺。 

Azure 托管磁盘中的数据将使用 256 位 [AES 加密法](https://en.wikipedia.org/wiki/Advanced_Encryption_Standard)（可用的最强大块加密法之一）以透明方式进行加密，并符合 FIPS 140-2 规范。 有关 Azure 托管磁盘底层加密模块的详细信息，请参见[加密 API：下一代](https://docs.microsoft.com/windows/desktop/seccng/cng-portal)

加密不会影响托管磁盘的性能，并且不会产生额外的费用。 

> [!NOTE]
> 临时磁盘不是托管磁盘，未由 SSE 加密；有关临时磁盘的详细信息，请参阅[托管磁盘概述：磁盘角色](managed-disks-overview.md#disk-roles)。

## <a name="about-encryption-key-management"></a>关于加密密钥管理

可以依赖于平台托管的密钥来加密托管磁盘，也可以使用自己的密钥来管理加密。 如果选择使用自己的密钥来管理加密，则可以指定*客户管理的密钥*，用于加密和解密托管磁盘中的所有数据。 

以下部分更详细地介绍了每个密钥管理选项。

## <a name="platform-managed-keys"></a>平台托管的密钥

默认情况下，托管磁盘使用平台托管的加密密钥。 自 2017 年 6 月 10 日起，所有写入现有托管磁盘的新托管磁盘、快照、映像和新数据都会自动使用平台托管密钥进行静态加密。

<!--Not Avaialble on ## Customer-managed keys-->

## <a name="server-side-encryption-versus-azure-disk-encryption"></a>服务器端加密与 Azure 磁盘加密

[虚拟机和虚拟机规模集的 Azure 磁盘加密](../../security/fundamentals/azure-disk-encryption-vms-vmss.md)利用 Linux 的 [DM-Crypt](https://en.wikipedia.org/wiki/Dm-crypt) 功能，通过来宾 VM 中的客户托管密钥来加密托管磁盘。  使用客户托管密钥的服务器端加密通过加密存储服务中的数据，使你能够将任何 OS 类型和映像用于 VM，从而改进了 ADE。

## <a name="next-steps"></a>后续步骤

- [探索 Azure 资源管理器模板以使用客户管理密钥创建加密磁盘](https://github.com/ramankumarlive/manageddiskscmkpreview)
- [什么是 Azure 密钥保管库？](../../key-vault/general/overview.md)
    <!--Not Available on - [Replicate machines with customer-managed keys enabled disks](../../site-recovery/azure-to-azure-how-to-enable-replication-cmk-disks.md)-->
- [使用 PowerShell 设置 VMware VM 到 Azure 的灾难恢复](../../site-recovery/vmware-azure-disaster-recovery-powershell.md#replicate-vmware-vms)
- [使用 PowerShell 和 Azure 资源管理器为 Hyper-V VM 设置到 Azure 的灾难恢复](../../site-recovery/hyper-v-azure-powershell-resource-manager.md#step-7-enable-vm-protection)

<!-- Update_Description: update meta properties, wording update, update link -->