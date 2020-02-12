---
title: 使用 Azure AD 凭据运行 Azure CLI 命令以访问 Blob 或队列数据
titleSuffix: Azure Storage
description: Azure CLI 支持使用 Azure AD 凭据登录，以便对 Azure 存储 Blob 和队列数据运行命令。 针对该会话提供了一个访问令牌，该访问令牌用于授权调用操作。 权限取决于分配给 Azure AD 安全主体的 RBAC 角色。
services: storage
author: WenJason
ms.service: storage
ms.topic: how-to
origin.date: 12/30/2019
ms.date: 02/10/2020
ms.author: v-jay
ms.reviewer: cbrooks
ms.subservice: common
ms.openlocfilehash: 00a9b981589df988fd11a48726c739574e316eb8
ms.sourcegitcommit: 5c4141f30975f504afc85299e70dfa2abd92bea1
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/05/2020
ms.locfileid: "77029361"
---
# <a name="run-azure-cli-commands-with-azure-ad-credentials-to-access-blob-or-queue-data"></a>使用 Azure AD 凭据运行 Azure CLI 命令以访问 Blob 或队列数据

Azure 存储为 Azure CLI 提供扩展，使用户可使用 Azure Active Directory (Azure AD) 凭据登录并运行脚本命令。 使用 Azure AD 凭据登录 Azure CLI 时，会返回 OAuth 2.0 访问令牌。 Azure CLI 自动使用该令牌针对 Blob 或队列存储进行后续数据操作授权。 对于支持的操作，无需再通过命令传递帐户密钥或 SAS 令牌。

可通过基于角色的访问控制 (RBAC) 向 Azure AD 安全主体分配对 Blob 和队列数据的权限。 有关 Azure 存储中 RBAC 角色的详细信息，请参阅[通过 RBAC 管理 Azure 存储数据访问权限](storage-auth-aad-rbac.md)。

## <a name="supported-operations"></a>支持的操作

Azure 存储扩展支持针对 blob 和队列数据的操作。 可调用的操作取决于向 Azure AD 安全主体授予的权限，此安全主体用于登录 Azure CLI。 Azure 存储容器或队列的权限通过 RBAC 进行分配。 例如，如果为你分配了“Blob 数据读取者”角色，你可以运行从容器或队列读取数据的脚本命令。  如果为你分配了“Blob 数据参与者”角色，你可以运行脚本命令来读取、写入或删除容器、队列或其中所含数据。 

若要详细了解针对容器或队列的每个 Azure 存储操作所需的权限，请参阅[使用 OAuth 令牌调用存储操作](https://docs.microsoft.com/rest/api/storageservices/authorize-with-azure-active-directory#call-storage-operations-with-oauth-tokens)。  

## <a name="call-azure-cli-commands-using-azure-ad-credentials"></a>使用 Azure AD 凭据调用 Azure CLI 命令

Azure CLI 支持使用 `--auth-mode` 参数进行 Blob 和队列数据操作：

- 将 `--auth-mode` 参数设置为 `login`，以使用 Azure AD 安全主体登录。
- 将 `--auth-mode` 参数设置为旧的 `key` 值，以便在未提供帐户的身份验证参数时，查询帐户密钥。

以下示例演示如何在 Azure CLI 中使用 Azure AD 凭据，在新的存储帐户中创建一个容器。 请务必将尖括号中的占位符值替换为你自己的值：

1. 确保已安装 Azure CLI 2.0.46 或更高版本。 运行 `az --version` 以查看已安装版本。

1. 运行 `az login` 并在浏览器窗口中进行身份验证：

    ```azurecli
    az login
    ```

1. 指定所需的订阅。 使用 [az group create](/cli/group?view=azure-cli-latest#az-group-create) 创建资源组。 使用 [az storage account create](/cli/storage/account?view=azure-cli-latest#az-storage-account-create) 在该资源组中创建存储帐户：

    ```azurecli
    az account set --subscription <subscription-id>

    az group create \
        --name sample-resource-group-cli \
        --location chinaeast2

    az storage account create \
        --name <storage-account> \
        --resource-group sample-resource-group-cli \
        --location chinaeast2 \
        --sku Standard_LRS \
        --encryption-services blob
    ```

1. 创建容器之前，请向自己分配[存储 Blob 数据参与者](../../role-based-access-control/built-in-roles.md#storage-blob-data-contributor)角色。 即使你是帐户所有者，也需要显式权限才能针对存储帐户执行数据操作。 有关如何分配 RBAC 角色的详细信息，请参阅[在 Azure 门户中使用 RBAC 授予对 Azure Blob 和队列数据的访问权限](storage-auth-aad-rbac.md)。

    > [!IMPORTANT]
    > 传播 RBAC 角色分配可能需要花费几分钟时间。

1. 在将 `--auth-mode` 参数设置为 `login` 的情况下，调用 [az storage container create](/cli/storage/container?view=azure-cli-latest#az-storage-container-create) 命令以使用 Azure AD 凭据创建容器：

    ```azurecli
    az storage container create \ 
        --account-name <storage-account> \ 
        --name sample-container \
        --auth-mode login
    ```

与 `--auth-mode` 参数关联的环境变量为 `AZURE_STORAGE_AUTH_MODE`。 可在环境变量中指定相应的值，以免每次调用 Azure 存储数据操作都要包含该值。

## <a name="next-steps"></a>后续步骤

- [使用 Azure CLI 为 blob 和队列数据分配 RBAC 角色](storage-auth-aad-rbac-cli.md)
- [使用 Azure 资源托管标识授予对 Blob 和队列数据的访问权限](storage-auth-aad-msi.md)
