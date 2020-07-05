---
title: Azure Key Vault 安全建议
description: Azure Key Vault 安全建议。 实施本文中的指导可帮助你履行我们的责任分担模型中所述的安全义务
services: key-vault
author: barclayn
manager: rkarlin
ms.service: key-vault
ms.subservice: general
ms.topic: article
origin.date: 09/30/2019
ms.date: 07/01/2020
ms.author: v-tawe
ms.custom: security-recommendations
ms.openlocfilehash: 97761ec6dc17d6e436afa25be6eba3327e08992f
ms.sourcegitcommit: 4f84bba7e509a321b6f68a2da475027c539b8fd3
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/01/2020
ms.locfileid: "85796296"
---
# <a name="security-recommendations-for-azure-key-vault"></a>Azure Key Vault 安全建议

本文包含针对 Azure Key Vault 的安全建议。 实施这些建议可帮助你履行我们的责任分担模型中所述的安全义务。 若要详细了解 Microsoft 采取哪些措施来履行服务提供商责任，请参阅[云计算的分担责任](https://gallery.technet.microsoft.com/Shared-Responsibilities-81d0ff91)。

Azure 安全中心可以自动监视本文所述的某些建议。 Azure 安全中心是保护 Azure 中的资源的第一道防线。 它会定期分析 Azure 资源的安全状态，以识别潜在的安全漏洞。 然后，它会建议如何解决这些漏洞。

- 有关 Azure 安全中心建议的详细信息，请参阅 [Azure 安全中心内的安全建议](../../security-center/security-center-recommendations.md)。
- 有关 Azure 安全中心的信息，请参阅[什么是 Azure 安全中心？](../../security-center/security-center-intro.md)

## <a name="data-protection"></a>数据保护

| 建议 | 注释 | 安全中心 |
|-|----|--|
|启用软删除 | 启用[软删除](overview-soft-delete.md)后，你可以恢复已删除的保管库和保管库对象 |  - |
| 限制对保管库数据的访问  | 遵循最低权限原则，且仅授权组织的部分成员访问保管库数据 |  - |

## <a name="identity-and-access-management"></a>标识和访问管理

| 建议 | 注释 | 安全中心 |
|-|----|--|
| 限制拥有参与者访问权限的用户数 | 如果某个用户对 Key Vault 管理平面拥有参与者权限，则该用户可以通过设置 Key Vault 访问策略来授予自己对数据平面的访问权限。 应严格控制对 Key Vault 拥有“参与者”角色访问权限的用户。 确保只有已获授权且有必要进行访问的人员可以访问和管理你的保管库。 可以阅读[保护对密钥保管库的访问](secure-your-key-vault.md) | - |

## <a name="monitoring"></a>监视

| 建议 | 注释 | 安全中心 |
|-|----|--|
| 应在 Key Vault 中启用诊断日志 | 启用日志并将其保留长达一年。 这样便可以在发生安全事件或网络遭泄露时，重新创建活动线索用于调查目的。 | - |
| 限制可以访问 Azure Key Vault 日志的用户 | [Key Vault 日志](logging.md)保存针对保管库执行的活动（例如创建或删除保管库、密钥、机密）的相关信息，这些信息可以在调查过程中使用。 |  - |

## <a name="networking"></a>网络

| 建议 | 注释 | 安全中心 |
|-|----|--|
|限制网络曝光 | 网络访问应该仅限需要进行保管库访问的解决方案所使用的虚拟网络。 查看有关 [Azure Key Vault 虚拟网络服务终结点](overview-vnet-service-endpoints.md)的信息 | - |

<!-- ## Next steps -->

<!-- Check with your application provider to see if there are additional security requirements. For more information on developing secure applications, see [Secure Development Documentation](../security/fundamentals/abstract-develop-secure-apps.md). -->
