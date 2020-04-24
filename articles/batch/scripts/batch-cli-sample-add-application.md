---
title: Azure CLI 脚本示例 - 在 Batch 中添加应用程序
description: 此示例脚本演示如何添加要与 Azure Batch 池或任务配合使用的应用程序。
services: batch
documentationcenter: ''
author: lingliw
manager: jeconnoc
editor: ''
ms.assetid: ''
ms.service: batch
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: multiple
ms.workload: na
origin.date: 01/29/2018
ms.date: 09/07/2018
ms.author: v-lingwu
ms.openlocfilehash: 8c8bf2998283de6633fc06fc0221b2e65a5ed6d7
ms.sourcegitcommit: c1ba5a62f30ac0a3acb337fb77431de6493e6096
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2020
ms.locfileid: "77028610"
---
# <a name="cli-example-add-an-application-to-an-azure-batch-account"></a>CLI 示例：向 Azure Batch 帐户添加应用程序

此脚本演示如何添加要与 Azure Batch 池或任务配合使用的应用程序。 若要设置添加到 Batch 帐户的应用程序，请将可执行文件与所有依赖文件一起打包为 zip 文件。 


如果选择在本地安装并使用 CLI，本文要求运行 Azure CLI 2.0.20 或更高版本。 运行 `az --version` 即可查找版本。 如果需要进行安装或升级，请参阅[安装 Azure CLI 2.0](/cli/install-azure-cli)。 

## <a name="example-script"></a>示例脚本

```azurecli
#!/bin/bash

# Create a resource group.
az group create --name myResourceGroup --location chinanorth

# Create a general-purpose storage account in your resource group.
az storage account create \
    --resource-group myResourceGroup \
    --name mystorageaccount \
    --location chinanorth \
    --sku Standard_LRS

# Create a Batch account.
az batch account create \
    --name mybatchaccount \
    --storage-account mystorageaccount \
    --resource-group myResourceGroup \
    --location chinanorth

# Authenticate against the account directly for further CLI interaction.
az batch account login \
    --name mybatchaccount \
    --resource-group myResourceGroup \
    --shared-key-auth

# Create a new application.
az batch application create \
    --resource-group myResourceGroup \
    --name mybatchaccount \
    --application-id myapp \
    --display-name "My Application"

# An application can reference multiple application executable packages
# of different versions. The executables and any dependencies need
# to be zipped up for the package. Once uploaded, the CLI attempts
# to activate the package so that it's ready for use.
az batch application package create \
    --resource-group myResourceGroup \
    --name mybatchaccount \
    --application-id myapp \
    --package-file my-application-exe.zip \
    --version 1.0

# Update the application to assign the newly added application
# package as the default version.
az batch application set \
    --resource-group myResourceGroup \
    --name mybatchaccount \
    --application-id myapp \
    --default-version 1.0
```
## <a name="clean-up-deployment"></a>清理部署

运行以下命令以删除资源组及其相关的所有资源。

```azurecli
az group delete --name myResourceGroup
```

## <a name="script-explanation"></a>脚本说明

此脚本使用以下命令。
表中的每条命令均链接到特定于命令的文档。

| Command | 说明 |
|---|---|
| [az group create](/cli/group#az-group-create) | 创建用于存储所有资源的资源组。 |
| [az storage account create](/cli/storage/account#az-storage-account-create) | 创建存储帐户。 |
| [az batch account create](/cli/batch/account#az-batch-account-create) | 创建批处理帐户。 |
| [az batch account login](/cli/batch/account#az-batch-account-login) | 针对指定的批处理帐户进行身份验证，以便进一步进行 CLI 交互。  |
| [az batch application create](/cli/batch/application#az-batch-application-create) | 创建应用程序。  |
| [az batch application package create](/cli/batch/application/package#az-batch-application-package-create) | 将应用程序包添加到指定的应用程序。  |
| [az batch application set](/cli/batch/application#az-batch-application-set) | 更新应用程序的属性。  |
| [az group delete](/cli/group#az-group-delete) | 删除资源组，包括所有嵌套的资源。 |

## <a name="next-steps"></a>后续步骤

有关 Azure CLI 的详细信息，请参阅 [Azure CLI 文档](/cli)。

<!-- Update_Description: link update -->