---
author: WenJason
ms.service: storage
ms.topic: include
origin.date: 11/06/2018
ms.date: 01/06/2020
ms.author: v-jay
ms.openlocfilehash: 679d26df1304b6a1dddeaa63efd8f492e395147e
ms.sourcegitcommit: 6a8bf63f55c925e0e735e830d67029743d2c7c0a
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/03/2020
ms.locfileid: "75624291"
---
## <a name="create-a-resource-group"></a>创建资源组

使用 [az group create](/cli/group) 命令创建 Azure 资源组。 资源组是在其中部署和管理 Azure 资源的逻辑容器。

请务必将尖括号中的占位符值替换为你自己的值：

```azurecli
az group create \
    --name <resource-group-name> \
    --location <location>
```

## <a name="create-a-storage-account"></a>创建存储帐户

使用 [az storage account create](https://docs.azure.cn/cli/storage/account) 命令创建常规用途存储帐户。 常规用途的存储帐户可用于以下四种服务：Blob、文件、表和队列。 

请务必将尖括号中的占位符值替换为你自己的值：

```azurecli
az storage account create \
    --name <account-name> \
    --resource-group <resource-group-name> \
    --location <location> \
    --sku Standard_LRS \
    --encryption blob
```

## <a name="specify-storage-account-credentials"></a>指定存储帐户凭据

Azure CLI 需要存储帐户凭据才能执行本教程的大部分命令。 提供凭据的选项有多种，其中最简单方法之一是设置 `AZURE_STORAGE_ACCOUNT` 和 `AZURE_STORAGE_KEY` 环境变量。

> [!NOTE]
> 本文介绍如何使用 Bash 设置环境变量。 其他环境可能需要修改语法。

首先，使用 [az storage account keys list](/cli/storage/account/keys) 命令显示存储帐户密钥。 请务必将尖括号中的占位符值替换为你自己的值：

```azurecli
az storage account keys list \
    --account-name <account-name> \
    --resource-group <resource-group-name> \
    --output table
```

现在，设置 `AZURE_STORAGE_ACCOUNT` 和 `AZURE_STORAGE_KEY` 环境变量。 可以通过使用 `export` 命令在 Bash shell 中完成此操作。 请务必将尖括号中的占位符值替换为你自己的值：

```bash
export AZURE_STORAGE_ACCOUNT="<account-name>"
export AZURE_STORAGE_KEY="<account-key>"
```

如需深入了解如何使用 Azure 门户检索帐户访问密钥，请参阅[管理存储帐户访问密钥](../articles/storage/common/storage-account-keys-manage.md)。