---
title: 在 Azure 数据资源管理器中保护群集
description: 本文介绍如何在 Azure 门户中通过 Azure 数据资源管理器保护群集。
author: orspod
ms.author: v-tawe
ms.reviewer: rkarlin
ms.service: data-explorer
ms.topic: conceptual
origin.date: 08/20/2019
ms.date: 03/09/2020
ms.openlocfilehash: 00e4d7401dd280bd3920e78a5c2b997f7d35cb8f
ms.sourcegitcommit: ced17aa58e800b9e4335276a1595b8045836b256
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/25/2020
ms.locfileid: "77590238"
---
# <a name="secure-your-cluster-in-azure-data-explorer---azure-portal"></a>在 Azure 数据资源管理器中保护群集 - Azure 门户

[Azure 磁盘加密](/security/azure-security-disk-encryption-overview)有助于保护数据，使组织能够信守在安全性与合规性方面作出的承诺。 它为群集虚拟机的 OS 和数据磁盘提供卷加密。 它还与 [Azure Key Vault](/key-vault/) 集成，让我们可以控制和管理磁盘加密密钥和机密，并确保 VM 磁盘上的所有数据在 Azure 存储中进行静态加密。 
  
## <a name="enable-encryption-at-rest-in-the-azure-portal"></a>在 Azure 门户中启用静态加密
  
通过群集安全设置可以在群集上启用磁盘加密。

1. 在 Azure 门户中，转到 Azure 数据资源管理器群集资源。 在“设置”标题下，选择“安全性”   。 

    ![启用静态加密](media/manage-cluster-security/security-encryption-at-rest.png)

1. 在“安全性”  窗口中，为“磁盘加密”  安全设置选择“打开”  。 

1. 选择“保存”  。
 
> [!NOTE]
> 启用加密后，选择“关闭”  可禁用加密。

## <a name="next-steps"></a>后续步骤

[检查群集运行状况](/data-explorer/check-cluster-health)
