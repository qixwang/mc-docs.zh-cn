---
title: 在 Azure 资源管理器中为虚拟机设置密钥保管库
description: 如何设置与 Azure 资源管理器虚拟机搭配使用的密钥保管库。
services: virtual-machines-windows
documentationcenter: ''
author: rockboyfor
manager: digimobile
editor: ''
tags: azure-resource-manager
ms.assetid: 33a483e2-cfbc-4c62-a588-5d9fd52491e2
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.topic: article
origin.date: 01/24/2017
ms.date: 02/10/2020
ms.author: v-yeche
ms.openlocfilehash: 15ec9cc99896fdb1af9c12935a91ad301b97d7b1
ms.sourcegitcommit: c1ba5a62f30ac0a3acb337fb77431de6493e6096
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2020
ms.locfileid: "79293083"
---
# <a name="set-up-key-vault-for-virtual-machines-in-azure-resource-manager"></a>在 Azure 资源管理器中为虚拟机设置密钥保管库

[!INCLUDE [learn-about-deployment-models](../../../includes/learn-about-deployment-models-rm-include.md)]

在 Azure 资源管理器堆栈中，密码/证书被建模为密钥保管库资源提供程序所提供的资源。 若要了解有关密钥保管库的详细信息，请参阅[什么是 Azure 密钥保管库？](../../key-vault/key-vault-overview.md)

> [!NOTE]
> 1. 为了让密钥保管库能与 Azure 资源管理器虚拟机搭配使用，必须将密钥保管库上的 **EnabledForDeployment** 属性设置为 true。 可以在各种客户端中执行此操作。
> 2. 需要在与虚拟机相同的订阅和位置中创建密钥保管库。
>
>

## <a name="use-powershell-to-set-up-key-vault"></a>使用 PowerShell 设置密钥保管库
若要使用 PowerShell 创建密钥保管库，请参阅[使用 PowerShell 在 Azure 密钥保管库中设置和检索机密](../../key-vault/quick-create-powershell.md)。

对于新的密钥保管库，可以使用此 PowerShell cmdlet：

    New-AzKeyVault -VaultName 'ContosoKeyVault' -ResourceGroupName 'ContosoResourceGroup' -Location 'China East' -EnabledForDeployment

对于现有的密钥保管库，可以使用此 PowerShell cmdlet：

    Set-AzKeyVaultAccessPolicy -VaultName 'ContosoKeyVault' -EnabledForDeployment

## <a name="use-cli-to-set-up-key-vault"></a>使用 CLI 设置密钥保管库
若要使用命令行接口 (CLI) 创建密钥保管库，请参阅[使用 CLI 管理密钥保管库](../../key-vault/key-vault-manage-with-cli2.md#create-a-key-vault)。

使用 CLI 时，必须先创建密钥保管库，然后分配部署策略。 可以使用以下命令来执行此操作：

    az keyvault create --name "ContosoKeyVault" --resource-group "ContosoResourceGroup" --location "chinaeast"

然后，要启用密钥保管库以用于模板部署，请运行以下命令：

    az keyvault update --name "ContosoKeyVault" --resource-group "ContosoResourceGroup" --enabled-for-deployment "true"

## <a name="use-templates-to-set-up-key-vault"></a>使用模板设置密钥保管库
使用模板时，必须将密钥保管库资源的 `enabledForDeployment` 属性设置为 `true`。

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

有关使用模板创建密钥保管库时可以配置的其他选项，请参阅[创建密钥保管库](https://github.com/Azure/azure-quickstart-templates/tree/master/101-key-vault-create/)。

> [!NOTE]
> 必须修改从 GitHub 存储库“azure-quickstart-templates”下载或参考的模板，以适应 Azure 中国云环境。 例如，替换某些终结点（将“blob.core.windows.net”替换为“blob.core.chinacloudapi.cn”，将“cloudapp.azure.com”替换为“cloudapp.chinacloudapi.cn”）；必要时更改某些不受支持的位置、VM 映像、VM 大小、SKU 以及资源提供程序的 API 版本。


<!-- Update_Description: update meta properties -->