---
title: 快速入门 - 使用 Azure CLI 创建 blob
titleSuffix: Azure Storage
description: 在本快速入门中，你将了解如何使用 Azure CLI 将 blob 上传到 Azure 存储、下载 blob 以及在容器中列出 blob。
services: storage
author: WenJason
ms.custom: mvc
ms.service: storage
ms.topic: quickstart
origin.date: 12/04/2019
ms.date: 01/06/2020
ms.author: v-jay
ms.openlocfilehash: 5701c3f3b876dbfac7d46e9de734b7d6710885b4
ms.sourcegitcommit: 6a8bf63f55c925e0e735e830d67029743d2c7c0a
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/03/2020
ms.locfileid: "75624292"
---
# <a name="quickstart-create-download-and-list-blobs-with-azure-cli"></a>快速入门：使用 Azure CLI 创建、下载和列出 blob

Azure CLI 是 Azure 的命令行体验，用于管理 Azure 资源。 可以将其安装在 macOS、Linux 和 Windows 上，然后从命令行运行它。 本快速入门介绍了如何使用 Azure CLI 通过 Azure Blob 存储来上传和下载数据。

[!INCLUDE [storage-multi-protocol-access-preview](../../../includes/storage-multi-protocol-access-preview.md)]

## <a name="prerequisites"></a>先决条件

[!INCLUDE [storage-quickstart-prereq-include](../../../includes/storage-quickstart-prereq-include.md)]

本快速入门需要运行 Azure CLI 2.0.4 或更高版本。 运行 `az --version` 即可确定你的版本。 如需进行安装或升级，请参阅[安装 Azure CLI](https://docs.azure.cn/cli/install-azure-cli)。

[!INCLUDE [storage-quickstart-tutorial-intro-include-cli](../../../includes/storage-quickstart-tutorial-intro-include-cli.md)]

## <a name="create-a-container"></a>创建容器

始终将 Blob 上传到容器中。 可以整理 Blob 组，就像在计算机的文件夹中整理文件一样。

可以使用 [az storage container create](https://docs.azure.cn/cli/storage/container) 命令创建用于存储 blob 的容器。

```azurecli
az storage container create --name sample-container
```

## <a name="upload-a-blob"></a>上传 blob

Blob 存储支持块 blob、追加 blob 和页 blob。 本快速入门中的示例介绍如何使用块 blob。

首先，创建要上传到块 blob 的文件。

此示例使用 [az storage blob upload](/cli/storage/blob) 命令将 Blob 上传到在上一个步骤中创建的容器中。

```azurecli
az storage blob upload \
    --container-name sample-container \
    --name helloworld \
    --file ~/path/to/local/file
```

此操作将创建 Blob（如果该 Blob 尚不存在），或者覆盖 Blob（如果该 Blob 已存在）。 上传尽可能多的文件，然后继续操作。

若要同时上传多个文件，则可使用 [az storage blob upload-batch](/cli/storage/blob) 命令。

## <a name="list-the-blobs-in-a-container"></a>列出容器中的 Blob

使用 [az storage blob list](/cli/storage/blob) 命令列出容器中的 blob。

```azurecli
az storage blob list \
    --container-name sample-container \
    --output table
```

## <a name="download-a-blob"></a>下载 Blob

使用 [az storage blob download](/cli/storage/blob) 命令下载之前上传的 Blob。

```azurecli
az storage blob download \
    --container-name sample-container \
    --name helloworld \
    --file ~/destination/path/for/file
```

## <a name="data-transfer-with-azcopy"></a>使用 AzCopy 传输数据

若要按可编写脚本的方式高性能地传输 Azure 存储中的数据，还可使用 [AzCopy](../common/storage-use-azcopy-linux.md?toc=%2fstorage%2fblobs%2ftoc.json) 实用工具。 可使用 AzCopy 将数据传输到 Blob、文件和表存储或将数据从中传出。

下面的示例使用 AzCopy 将名为 myfile.txt 的文件上传到 sample-container 容器   。 请务必将尖括号中的占位符值替换为你自己的值：

```bash
azcopy \
    --source /mnt/myfiles \
    --destination https://<account-name>.blob.core.chinacloudapi.cn/sample-container \
    --dest-key <account-key> \
    --include "myfile.txt"
```

## <a name="clean-up-resources"></a>清理资源

如果不再需要资源组中的任何资源（包括在本快速入门中创建的存储帐户），请使用 [az group delete](/cli/group) 命令删除该资源组。 请务必将尖括号中的占位符值替换为你自己的值：

```azurecli
az group delete --name <resource-group-name>
```

## <a name="next-steps"></a>后续步骤

在此快速入门中，你了解了如何在本地文件系统和 Azure Blob 存储中的容器之间传输文件。 若要详细了解如何使用 Azure 存储中的 blob，请继续通过本教程了解如何使用 Azure Blob 存储。

> [!div class="nextstepaction"]
> [如何：通过 Azure CLI 对 Blob 存储执行操作](storage-how-to-use-blobs-cli.md)
