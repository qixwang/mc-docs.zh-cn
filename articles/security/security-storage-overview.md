---
title: 可与 Azure 存储配合使用的安全功能 | Azure
description: 本文概述可与 Azure 存储配合使用的核心 Azure 安全功能。
services: security
documentationcenter: na
author: lingliw
manager: digimobile
editor: TomSh
ms.assetid: 521180dc-2cc9-43f1-ae87-2701de7ca6b8
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
origin.date: 03/26/2019
ms.date: 09/29/2019
ms.author: v-lingwu
ms.openlocfilehash: 1440bc4913b2a3ee4ebba046ba25c030ea20a524
ms.sourcegitcommit: c1ba5a62f30ac0a3acb337fb77431de6493e6096
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2020
ms.locfileid: "71674699"
---
# <a name="azure-storage-security-overview"></a>Azure 存储安全概述

Azure 存储是依赖于持续性、可用性和伸缩性来满足客户需求的现代应用程序的云存储解决方案。 Azure 存储提供全面的安全功能。 可以：

* 使用基于角色的访问控制 (RBAC) 和 Azure Active Directory 对存储帐户进行安全保护。
* 使用客户端加密、HTTPS 或 SMB 3.0 对应用程序和 Azure 之间传输的数据进行安全保护。
* 可将数据设置为在写入 Azure 存储时使用存储服务加密自动进行加密。
* 将虚拟机 (VM) 使用的 OS 和数据磁盘设置为使用 Azure 磁盘加密进行加密。
* 使用共享访问签名 (SAS) 授予对 Azure 存储中数据对象的委派访问权限。
* 使用分析来跟踪某人访问存储时使用的身份验证方法。

有关 Azure 存储中安全性的详细信息，请参阅 [Azure 存储安全指南](../storage/common/storage-security-guide.md)。 本指南深入介绍了 Azure 存储的安全功能。 这些功能包括存储帐户密钥、传输中和静态中的数据加密以及存储分析。

## <a name="role-based-access-control"></a>基于角色的访问控制

可使用基于角色的访问控制来帮助保护存储帐户。 对于想要实施数据访问安全策略的组织而言，必须根据[需要知道](https://en.wikipedia.org/wiki/Need_to_know)和[最低权限](https://en.wikipedia.org/wiki/Principle_of_least_privilege)安全策略限制访问权限。 这些访问权限是通过将相应的 RBAC 角色分配给特定范围内的组和应用程序来授予的。 可以使用[内置 RBAC 角色](../role-based-access-control/built-in-roles.md)（例如存储帐户参与者）将权限分配给用户。

了解详细信息：

* [Azure Active Directory 基于角色的访问控制](../role-based-access-control/role-assignments-portal.md)

## <a name="delegated-access-to-storage-objects"></a>存储对象的委托访问权限

共享访问签名对存储帐户中的资源提供委托访问。 使用 SAS，意味着可以授权客户端在指定时间段内，以一组指定权限有限访问存储帐户中的对象。 可以授予这些有限的权限，而不必共享帐户访问密钥。 

SAS 是一个 URI，在其查询参数中包含对存储资源已验证访问所需的所有信息。 要使用 SAS 访问存储资源，客户端只需将 SAS 提供给相应的构造函数或方法。

了解详细信息：

* [了解 SAS 模型](../storage/common/storage-dotnet-shared-access-signature-part-1.md)
* [创建 SAS 并将其用于 Blob 存储](../storage/common/storage-dotnet-shared-access-signature-part-1.md)

## <a name="encryption-in-transit"></a>传输中加密

传输中加密是通过网络传输数据时保护数据的一种机制。 在 Azure 存储中，可使用以下功能保护数据：

* [传输级别加密](../storage/common/storage-security-guide.md#encryption-in-transit)，例如将数据传入或传出 Azure 存储时使用的 HTTPS。
* [线路加密](../storage/common/storage-security-guide.md#using-encryption-during-transit-with-azure-file-shares)，例如 Azure 文件共享的 SMB 3.0 加密。
* [客户端加密](../storage/common/storage-security-guide.md#using-client-side-encryption-to-secure-data-that-you-send-to-storage)，在将数据传输到存储之前加密数据，以及从存储传出数据后解密数据。

了解有关客户端加密的详细信息：

* [适用于世纪互联 Azure 存储的客户端加密](https://blogs.msdn.microsoft.com/windowsazurestorage/2015/04/28/client-side-encryption-for-microsoft-azure-storage-preview/)
* [云安全控制系列：加密传输中的数据](https://cloudblogs.microsoft.com/microsoftsecure/2015/08/10/cloud-security-controls-series-encrypting-data-in-transit/)

## <a name="encryption-at-rest"></a>静态加密

对许多组织而言，[静态数据加密](https://cloudblogs.microsoft.com/microsoftsecure/2015/09/10/cloud-security-controls-series-encrypting-data-at-rest/)是实现数据隐私性、符合性和数据所有权的必要措施。 可通过三种 Azure 功能进行静态数据加密：

* [存储服务加密](../storage/common/storage-security-guide.md#encryption-at-rest)可请求存储服务在将数据写入 Azure 存储时自动加密数据。
* [客户端加密](../storage/common/storage-security-guide.md#client-side-encryption)也提供静态加密功能。
* [Azure 磁盘加密](../storage/common/storage-security-guide.md#using-azure-disk-encryption-to-encrypt-disks-used-by-your-virtual-machines)允许加密 IaaS 虚拟机使用的 OS 磁盘和数据磁盘。

了解有关存储服务加密的详细信息：

* [Azure 存储服务加密](/storage/)适用于 [Azure Blob 存储](/storage/blobs/)。 有关其他 Azure 存储类型的详细信息，请参阅 [Azure 文件](/storage/files/)、[磁盘（高级存储）](/storage/premium-storage/)、[表存储](/storage/tables/)和[队列存储](/storage/queues/)。
* [静态数据的 Azure 存储服务加密](../storage/common/storage-service-encryption.md)

## <a name="azure-disk-encryption"></a>Azure 磁盘加密

适用于虚拟机的 Azure 磁盘加密有助于解决组织安全性和符合性要求。 它使用 [Azure Key Vault](/key-vault/) 中控制的密钥和策略来加密 VM 磁盘（包括启动盘和数据磁盘）。

适用于 VM 的磁盘加密可用于 Linux 与 Windows 操作系统。 它也使用密钥保管库帮助你保护、管理和审核磁盘加密密钥的使用。 在 Azure 存储帐户中使用行业标准加密技术，对 VM 磁盘中的所有数据进行静态加密。 适用于 Windows 的磁盘加密解决方案是基于 [Microsoft BitLocker 驱动器加密](https://technet.microsoft.com/library/cc732774.aspx)技术，Linux 解决方案基于 [dm-crypt](https://en.wikipedia.org/wiki/Dm-crypt)。

## <a name="azure-key-vault"></a>Azure Key Vault

Azure 磁盘加密使用 [Azure 密钥保管库](https://www.azure.cn/home/features/key-vault/)，可帮助你控制和管理密钥保管库订阅中的磁盘加密密钥和机密，同时确保虚拟机磁盘中的所有数据可在 Azure 存储中静态加密。 应使用密钥保管库来审核密钥和策略的使用。

了解详细信息

* [什么是 Azure 密钥保管库？](../key-vault/key-vault-overview.md)
