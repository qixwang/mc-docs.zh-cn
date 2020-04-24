---
title: 如何使用 Azure CLI 为虚拟机设置 Key Vault | Azure
description: 如何使用 Azure CLI 设置用于 Azure 资源管理器虚拟机的 Key Vault。
services: virtual-machines-linux
documentationcenter: ''
author: rockboyfor
manager: digimobile
editor: ''
tags: azure-resource-manager
ms.assetid: bccdd5ab-5ccf-4760-9039-92c6eafb15bd
ms.service: virtual-machines-linux
ms.devlang: azurecli
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.topic: article
origin.date: 02/24/2017
ms.date: 10/14/2019
ms.author: v-yeche
ms.openlocfilehash: d8207274ee2bf3fbec40731d4a31b7e5ac955c8e
ms.sourcegitcommit: c1ba5a62f30ac0a3acb337fb77431de6493e6096
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2020
ms.locfileid: "72272427"
---
# <a name="how-to-set-up-key-vault-for-virtual-machines-with-the-azure-cli"></a>如何使用 Azure CLI 为虚拟机设置 Key Vault

在 Azure 资源管理器堆栈中，密码/证书被建模为 Key Vault 所提供的资源。 若要了解有关 Azure 密钥保管库的详细信息，请参阅[什么是 Azure 密钥保管库？](../../key-vault/key-vault-overview.md) 为了让 Key Vault 能与 Azure 资源管理器 VM 搭配使用，必须将 Key Vault 上的 *EnabledForDeployment* 属性设置为 true。 本文说明如何通过 Azure CLI 设置用于 Azure 虚拟机 (VM) 的 Key Vault。 

若要执行这些步骤，需要安装最新的 [Azure CLI](https://docs.azure.cn/cli/install-az-cli2?view=azure-cli-latest)，并使用 [az login](https://docs.azure.cn/cli/reference-index?view=azure-cli-latest#az-login) 登录到 Azure 帐户。

[!INCLUDE [azure-cli-2-azurechinacloud-environment-parameter](../../../includes/azure-cli-2-azurechinacloud-environment-parameter.md)]

## <a name="create-a-key-vault"></a>创建密钥保管库
使用 [az keyvault create](https://docs.azure.cn/cli/keyvault?view=azure-cli-latest#az-keyvault-create) 创建密钥保管库并分配部署策略。 以下示例在 `myKeyVault` 资源组中创建名为 `myResourceGroup` 的密钥保管库：

```azurecli
az keyvault create -l chinanorth -n myKeyVault -g myResourceGroup --enabled-for-deployment true
```

## <a name="update-a-key-vault-for-use-with-vms"></a>更新用于 VM 的 Key Vault
使用 [az keyvault update](https://docs.azure.cn/cli/keyvault?view=azure-cli-latest#az-keyvault-update) 在现有的密钥保管库上设置部署策略。 以下命令在 `myKeyVault` 资源组中更新名为 `myResourceGroup` 的密钥保管库：

```azurecli
az keyvault update -n myKeyVault -g myResourceGroup --set properties.enabledForDeployment=true
```

## <a name="use-templates-to-set-up-key-vault"></a>使用模板设置密钥保管库
使用模板时，必须将 Key Vault 资源的 `enabledForDeployment` 属性设置为 `true`，如下所示：

```json
{
    "type": "Microsoft.KeyVault/vaults",
    "name": "ContosoKeyVault",
    "apiVersion": "2015-06-01",
    "location": "<location-of-key-vault>",
    "properties": {
    "enabledForDeployment": "true",
    ....
    ....
    }
}
```

## <a name="next-steps"></a>后续步骤
有关使用模板创建 Key Vault 时可以配置的其他选项，请参阅[创建密钥保管库](https://github.com/Azure/azure-quickstart-templates/tree/master/101-key-vault-create/)。

<!-- Update_Description: wording update -->