---
title: include 文件
description: include 文件
services: virtual-machines
author: rockboyfor
ms.service: virtual-machines
ms.topic: include
origin.date: 10/24/2019
ms.date: 11/11/2019
ms.author: v-yeche
ms.custom: include file
ms.openlocfilehash: dcb201963d9db6e8d3bdf228d9d5771c001bbe0f
ms.sourcegitcommit: c1ba5a62f30ac0a3acb337fb77431de6493e6096
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2020
ms.locfileid: "73730644"
---
默认情况下，在将数据保存到云时，Azure 托管磁盘会自动加密数据。 服务器端加密可保护数据，并帮助组织履行在安全性与合规性方面做出的承诺。 Azure 托管磁盘中的数据将使用 256 位 [AES 加密法](https://en.wikipedia.org/wiki/Advanced_Encryption_Standard)（可用的最强大块加密法之一）以透明方式进行加密，并符合 FIPS 140-2 规范。   

加密不会影响托管磁盘的性能。 加密不会产生额外的费用。

有关 Azure 托管磁盘底层加密模块的详细信息，请参见[加密 API：下一代](https://docs.microsoft.com/windows/desktop/seccng/cng-portal)

## <a name="about-encryption-key-management"></a>关于加密密钥管理

可以依赖于平台托管的密钥来加密托管磁盘，也可以使用自己的密钥来管理加密（公共预览版）。 如果选择使用自己的密钥来管理加密，则可以指定*客户管理的密钥*，用于加密和解密托管磁盘中的所有数据。 

以下部分更详细地介绍了每个密钥管理选项。

## <a name="platform-managed-keys"></a>平台托管的密钥

默认情况下，托管磁盘使用平台托管的加密密钥。 从 2017 年 6 月 10 日起，所有新托管磁盘、快照、映像和写入现有托管磁盘的新数据均会使用平台托管的密钥自动进行静态加密。 

<!--Not Available on ## Customer-managed keys (public preview)-->
<!--MOONCAK: ONLY VALID ON West Central US Region-->

## <a name="server-side-encryption-versus-azure-disk-encryption"></a>服务器端加密与 Azure 磁盘加密

Azure 磁盘加密利用 Windows 的 [BitLocker](https://docs.microsoft.com/windows/security/information-protection/bitlocker/bitlocker-overview) 功能和 Linux 的 [DM-Crypt](https://en.wikipedia.org/wiki/Dm-crypt) 功能，在来宾 VM 中使用客户托管密钥来加密托管磁盘。  使用客户托管密钥的服务器端加密通过加密存储服务中的数据，使你能够将任何 OS 类型和映像用于 VM，从而改进了 ADE。

<!--Not Available on [Azure Disk Encryption](../articles/security/fundamentals/azure-disk-encryption-vms-vmss.md)-->

## <a name="next-steps"></a>后续步骤

- [什么是 Azure 密钥保管库？](../articles/key-vault/key-vault-overview.md)

<!-- Update_Description: new article about disk storage encryption -->
<!--NEW.date: 11/11/2019-->