---
title: 在 Azure 中保护 Azure 数据资源管理器群集
description: 了解如何保护 Azure 数据资源管理器中的群集。
author: saguiitay
ms.author: v-tawe
ms.reviewer: orspodek
ms.service: data-explorer
ms.topic: conceptual
origin.date: 01/06/2020
ms.date: 03/16/2020
ms.openlocfilehash: d44199e0a14918c16d3c5d06deea39a1b5cce205
ms.sourcegitcommit: 44d3fe59952847e5394bbe6c05bd6f333bb56345
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/01/2020
ms.locfileid: "80522055"
---
# <a name="secure-azure-data-explorer-clusters-in-azure"></a>在 Azure 中保护 Azure 数据资源管理器群集

本文介绍 Azure 数据资源管理器中的安全性，以帮助你保护云中的数据和资源，并满足企业的安全需求。 确保群集安全非常重要。 可通过一项或多项 Azure 功能来保护群集，包括安全访问和存储。 本文提供的信息可帮助你确保群集安全。

## <a name="managed-identities-for-azure-resources"></a>Azure 资源的托管标识

构建云应用程序时面临的一个常见难题是，如何管理代码中用于云服务身份验证的凭据。 保护这些凭据是一项重要任务。 不应将这些凭据存储在开发人员工作站中，或将其签入源代码管理。 虽然 Azure Key Vault 可用于安全存储凭据、机密以及其他密钥，但代码需要通过 Key Vault 的身份验证才能检索它们。

Azure 资源的 Azure Active Directory (Azure AD) 托管标识功能可以解决此问题。 此功能为 Azure 服务提供了 Azure AD 中的自动托管标识。 可以使用此标识向支持 Azure AD 身份验证的任何服务（包括 Key Vault）证明身份，无需在代码中放入任何凭据。 有关此服务的详细信息，请参阅 [Azure 资源的托管标识](/active-directory/managed-identities-azure-resources/overview)概述页。

## <a name="data-encryption"></a>数据加密

### <a name="azure-disk-encryption"></a>Azure 磁盘加密

[Azure 磁盘加密](/security/azure-security-disk-encryption-overview)有助于保护数据，使组织能够信守在安全性与合规性方面作出的承诺。 它为群集虚拟机的 OS 和数据磁盘提供卷加密。 Azure 磁盘加密还与 [Azure Key Vault](/key-vault/) 集成，使我们可以控制和管理磁盘加密密钥和机密，并确保 VM 磁盘上的所有数据已加密。 

<!--
### Customer-managed keys with Azure Key Vault

By default, data is encrypted with Microsoft-managed keys. For additional control over encryption keys, you can supply customer-managed keys to use for data encryption. You can manage encryption of your data at the storage level with your own keys. A customer-managed key is used to protect and control access to the root encryption key, which is used to encrypt and decrypt all data. Customer-managed keys offer greater flexibility to create, rotate, disable, and revoke access controls. You can also audit the encryption keys used to protect your data.

Use Azure Key Vault to store your customer-managed keys. You can create your own keys and store them in a key vault, or you can use an Azure Key Vault API to generate keys. The Azure Data Explorer cluster and the Azure Key Vault must be in the same region, but they can be in different subscriptions. For more information about Azure Key Vault, see [What is Azure Key Vault?](/key-vault/key-vault-overview). For a detailed explanation on customer-managed keys, see [Customer-managed keys with Azure Key Vault](/storage/common/storage-service-encryption). Configure customer-managed keys in your Azure Data Explorer cluster using [C#](/data-explorer/customer-managed-keys-csharp) or the [Azure Resource Manager template](/data-explorer/customer-managed-keys-resource-manager)

> [!Note]
> Customer-managed keys rely on managed identities for Azure resources, a feature of Azure Active Directory (Azure AD). To configure customer-managed keys in the Azure portal, you need to configure a **SystemAssigned** managed identity to your cluster.
-->

#### <a name="store-customer-managed-keys-in-azure-key-vault"></a>将客户管理的密钥存储在 Azure 密钥保管库

若要在群集中启用客户管理的密钥，请使用 Azure Key Vault 来存储密钥。 必须同时启用密钥保管库上的“软删除”和“不清除”属性   。 Key Vault 必须与群集位于同一订阅中。 Azure 数据资源管理器使用 Azure 资源的托管标识向 Key Vault 进行身份验证，以执行加密和解密操作。 托管标识不支持跨目录方案。

#### <a name="rotate-customer-managed-keys"></a>轮换客户管理的密钥

可以根据自己的合规性策略，在 Azure 密钥保管库中轮换客户管理的密钥。 轮换密钥后，必须更新群集才能使用新的密钥 URI。 轮换密钥不会触发群集中数据的重新加密。 

#### <a name="revoke-access-to-customer-managed-keys"></a>撤消对客户管理的密钥的访问权限

若要撤消对客户管理的密钥的访问权限，请使用 PowerShell 或 Azure CLI。 有关详细信息，请参阅 [Azure Key Vault PowerShell](https://docs.microsoft.com/powershell/module/az.keyvault/) 或 [Azure 密钥保管库 CLI](/cli/keyvault)。 撤消访问权限会阻止访问群集的存储级别中的所有数据，因为在这种情况下，Azure 数据资源管理器无法访问加密密钥。

> [!Note]
> 当 Azure 数据资源管理器识别到对客户管理的密钥的访问权限被撤消时，它会自动挂起群集，以删除所有缓存的数据。 重新授予对密钥的访问权限后，需要手动恢复群集。

## <a name="role-based-access-control"></a>基于角色的访问控制

使用[基于角色的访问控制 (RBAC)](/role-based-access-control/overview) 可以在团队中分离职责，并只向群集用户授予所需的访问权限。 可以仅允许某些操作，而不是向群集上的每个人授予不受限制的权限。 可以使用 [Azure 门户](/role-based-access-control/role-assignments-portal)、[Azure CLI](/role-based-access-control/role-assignments-cli) 或 [Azure PowerShell](/role-based-access-control/role-assignments-powershell) [针对数据库配置访问控制](/data-explorer/manage-database-permissions)。

## <a name="next-steps"></a>后续步骤

* 通过启用静态加密[保护 Azure 数据资源管理器中的群集 - 门户](manage-cluster-security.md)。
* [配置 Azure 数据资源管理器群集的托管标识](managed-identities.md)

<!-- * [Configure customer-managed-keys using the Azure Resource Manager template](customer-managed-keys-resource-manager.md) -->
<!-- * [Configure customer-managed-keys using C#](customer-managed-keys-csharp.md) -->