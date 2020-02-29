---
title: Azure 托管磁盘的服务器端加密 - Azure CLI
description: Azure 存储在将数据保存到存储群集之前会对其进行静态加密，以此保护数据。 可以依赖于 Azure 托管的密钥来加密托管磁盘，也可以使用客户管理的密钥通过自己的密钥来管理加密。
author: rockboyfor
origin.date: 01/13/2020
ms.date: 02/10/2020
ms.topic: conceptual
ms.author: v-yeche
ms.service: virtual-machines-linux
ms.subservice: disks
ms.openlocfilehash: e960c9ea909f05774b74f0b139a103f5871dbafe
ms.sourcegitcommit: ada94ca4685855f58616e4bf1dd5ca757878dfdc
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/18/2020
ms.locfileid: "77428479"
---
# <a name="server-side-encryption-of-azure-managed-disks"></a>Azure 托管磁盘的服务器端加密

默认情况下，在将数据保存到云时，Azure 托管磁盘会自动加密数据。 服务器端加密可保护数据，并帮助组织履行在安全性与合规性方面做出的承诺。 Azure 托管磁盘中的数据将使用 256 位 [AES 加密法](https://en.wikipedia.org/wiki/Advanced_Encryption_Standard)（可用的最强大块加密法之一）以透明方式进行加密，并符合 FIPS 140-2 规范。   

加密不会影响托管磁盘的性能。 加密不会产生额外的费用。

有关 Azure 托管磁盘底层加密模块的详细信息，请参见[加密 API：下一代](https://docs.microsoft.com/windows/desktop/seccng/cng-portal)

## <a name="about-encryption-key-management"></a>关于加密密钥管理

可以依赖于平台托管的密钥来加密托管磁盘，也可以使用自己的密钥来管理加密。 如果选择使用自己的密钥来管理加密，则可以指定*客户管理的密钥*，用于加密和解密托管磁盘中的所有数据。 

以下部分更详细地介绍了每个密钥管理选项。

## <a name="platform-managed-keys"></a>平台托管的密钥

默认情况下，托管磁盘使用平台托管的加密密钥。 自 2017 年 6 月 10 日起，所有写入现有托管磁盘的新托管磁盘、快照、映像和新数据都会自动使用平台托管密钥进行静态加密。 

<!--Not Avaialble on ## Customer-managed keys-->

## <a name="server-side-encryption-versus-azure-disk-encryption"></a>服务器端加密与 Azure 磁盘加密

针对虚拟机和虚拟机规模集的 Azure 磁盘加密利用 Windows 的 [BitLocker](https://docs.microsoft.com/windows/security/information-protection/bitlocker/bitlocker-overview) 功能和 Linux 的 [DM-Crypt](https://en.wikipedia.org/wiki/Dm-crypt) 功能，在来宾 VM 中使用客户托管密钥来加密托管磁盘。  使用客户托管密钥的服务器端加密通过加密存储服务中的数据，使你能够将任何 OS 类型和映像用于 VM，从而改进了 ADE。

<!--Not Available on [Azure Disk Encryption for virtual machines and virtual machine scale sets](../../security/fundamentals/azure-disk-encryption-vms-vmss.md)-->

## <a name="next-steps"></a>后续步骤

- [探索 Azure 资源管理器模板以使用客户管理密钥创建加密磁盘](https://github.com/ramankumarlive/manageddiskscmkpreview)
- [什么是 Azure 密钥保管库？](../../key-vault/key-vault-overview.md)
- [复制具有已启用客户管理密钥的磁盘的计算机](../../site-recovery/azure-to-azure-how-to-enable-replication-cmk-disks.md)
- [使用 PowerShell 设置 VMware VM 到 Azure 的灾难恢复](../../site-recovery/vmware-azure-disaster-recovery-powershell.md#replicate-vmware-vms)
- [使用 PowerShell 和 Azure 资源管理器为 Hyper-V VM 设置到 Azure 的灾难恢复](../../site-recovery/hyper-v-azure-powershell-resource-manager.md#step-7-enable-vm-protection)

<!-- Update_Description: update meta properties, wording update, update link -->