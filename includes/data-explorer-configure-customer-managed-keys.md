---
author: orspod
ms.service: data-explorer
ms.topic: include
origin.date: 01/07/2020
ms.date: 02/17/2020
ms.author: v-tawe
ms.openlocfilehash: e100140700a1981ead30e7af6f4992ae8657e323
ms.sourcegitcommit: 7c80405a6b48380814b4b414e9f8a5756c007880
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/07/2020
ms.locfileid: "77067770"
---
Azure 数据资源管理器对静态存储帐户中的所有数据进行加密。 默认情况下，使用 Microsoft 管理的密钥对数据进行加密。 为了更进一步控制加密密钥，可以提供客户管理的密钥来用于对数据进行加密。 客户管理的密钥必须存储在 [Azure Key Vault](/key-vault/key-vault-overview) 中。 你可以创建自己的密钥并将其存储在密钥保管库中，或者使用 Azure Key Vault API 来生成密钥。 Azure 数据资源管理器群集和密钥保管库必须在同一个区域中，但可以在不同的订阅中。 有关客户管理的密钥的详细说明，请参阅[客户管理的密钥与 Azure Key Vault](/storage/common/storage-service-encryption)。 本文将介绍如何配置客户管理的密钥。

若要使用 Azure 数据资源管理器配置客户管理的密钥，必须[在密钥保管库上设置两个属性](/key-vault/key-vault-ovw-soft-delete)：“软删除”和“不清除”。   默认情况下未启用这些属性。 若要启用这些属性，请使用 [PowerShell](/key-vault/key-vault-soft-delete-powershell) 或 [Azure CLI](/key-vault/key-vault-soft-delete-cli)。 仅支持 RSA 密钥以及密钥大小 2048。

> [!NOTE]
> [先导和后继群集](/data-explorer/follower)上不支持使用客户管理的密钥进行数据加密。 

## <a name="assign-an-identity-to-the-cluster"></a>为群集分配标识

若要为群集启用客户管理的密钥，请先将一个系统分配的托管标识分配给该群集。 你将使用此托管标识授予群集访问密钥保管库的权限。 若要配置系统分配的托管标识，请参阅[托管标识](/data-explorer/managed-identities)。

## <a name="create-a-new-key-vault"></a>创建新的 Key Vault

若要使用 PowerShell 创建新的 Key Vault，请调用 [New-AzKeyVault](https://docs.microsoft.com/powershell/module/az.keyvault/new-azkeyvault)。 必须为用来存储客户管理的密钥（用于 Azure 数据资源管理器加密）的 Key Vault 启用两项密钥保护设置：“软删除”和“不要清除”。   请将以下示例中括号中的占位符值替换为你自己的值。

```azurepowershell-interactive
$keyVault = New-AzKeyVault -Name <key-vault> `
    -ResourceGroupName <resource_group> `
    -Location <location> `
    -EnableSoftDelete `
    -EnablePurgeProtection
```

## <a name="configure-the-key-vault-access-policy"></a>配置 Key Vault 访问策略

接下来，配置密钥保管库的访问策略，使群集有权访问密钥保管库。 在此步骤中，你将使用前面分配给群集的系统分配的托管标识。 若要设置 Key Vault 的访问策略，请调用 [Set-AzKeyVaultAccessPolicy](https://docs.microsoft.com/powershell/module/az.keyvault/set-azkeyvaultaccesspolicy)。 请将括号中的占位符值替换为你自己的值，并使用前面示例中定义的变量。

```azurepowershell-interactive
Set-AzKeyVaultAccessPolicy `
    -VaultName $keyVault.VaultName `
    -ObjectId $cluster.Identity.PrincipalId `
    -PermissionsToKeys wrapkey,unwrapkey,get,recover
```

## <a name="create-a-new-key"></a>新建密钥

接下来，在 Key Vault 中创建新密钥。 若要创建新密钥，请调用 [Add-AzKeyVaultKey](https://docs.microsoft.com/powershell/module/az.keyvault/add-azkeyvaultkey)。 请将括号中的占位符值替换为你自己的值，并使用前面示例中定义的变量。

```azurepowershell-interactive
$key = Add-AzKeyVaultKey -VaultName $keyVault.VaultName -Name <key> -Destination 'Software'
```
