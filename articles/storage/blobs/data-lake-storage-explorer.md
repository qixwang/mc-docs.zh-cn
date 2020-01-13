---
title: 将 Azure 存储资源管理器与 Azure Data Lake Storage Gen2 配合使用
description: 了解如何使用 Azure 存储资源管理器在 Azure Data Lake Storage Gen2 帐户中创建文件系统，以及目录和文件。 接下来，介绍如何将文件下载到本地计算机，以及如何查看目录中的所有文件。
author: WenJason
ms.subservice: data-lake-storage-gen2
ms.service: storage
ms.topic: conceptual
origin.date: 11/19/2019
ms.date: 01/06/2020
ms.author: v-jay
ms.reviewer: stewu
ms.openlocfilehash: ae79e568548ef0262fcbd86d66807517588b1bf8
ms.sourcegitcommit: 6a8bf63f55c925e0e735e830d67029743d2c7c0a
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/03/2020
ms.locfileid: "75624353"
---
# <a name="use-azure-storage-explorer-with-azure-data-lake-storage-gen2"></a>将 Azure 存储资源管理器与 Azure Data Lake Storage Gen2 配合使用

本文介绍如何使用 [Azure 存储资源管理器](https://azure.microsoft.com/features/storage-explorer/)创建目录和 blob。 接下来，介绍如何将 blob 下载到本地计算机，以及如何在目录中查看所有 blob。 此外，还将了解如何创建 blob 快照、管理目录访问策略以及创建共享访问签名。

## <a name="prerequisites"></a>先决条件

[!INCLUDE [storage-quickstart-prereq-include](../../../includes/storage-quickstart-prereq-include.md)]

本快速入门要求安装 Azure 存储资源管理器。 若要安装适用于 Windows、Macintosh 或 Linux 的 Azure 存储资源管理器，请参阅 [Azure 存储资源管理器](https://azure.microsoft.com/features/storage-explorer/)。

## <a name="sign-in-to-storage-explorer"></a>登录到存储资源管理器

首次启动时，会显示“Microsoft Azure 存储资源管理器 - 连接”窗口。  尽管存储资源管理器提供了几种连接到存储帐户的方法，但是目前只有一种方法支持管理 ACL。

|任务|目的|
|---|---|
|添加 Azure 帐户 | 将你重定向到组织登录页，以便在 Azure 上进行身份验证。 目前，如果想管理和设置 ACL，这是唯一支持的身份验证方法。 |

选择“添加 Azure 帐户”  ，并单击“登录”  。遵照屏幕提示登录到 Azure 帐户。

![“Microsoft Azure 存储资源管理器 - 连接”窗口](media/storage-quickstart-blobs-storage-explorer/connect.png)

完成连接以后，Azure 存储资源管理器会进行加载并显示“资源管理器”选项卡。  以下视图可以查看通过 [Azure 存储模拟器](../common/storage-use-emulator.md?toc=%2fstorage%2fblobs%2ftoc.json)、[Cosmos DB](../../cosmos-db/storage-explorer.md?toc=%2fstorage%2fblobs%2ftoc.json) 帐户或 [Azure Stack](/azure-stack/user/azure-stack-storage-connect-se?toc=%2fstorage%2fblobs%2ftoc.json) 环境配置的所有 Azure 存储帐户和本地存储。

![“Microsoft Azure 存储资源管理器 - 连接”窗口](media/storage-quickstart-blobs-storage-explorer/mainpage.png)

## <a name="create-a-container"></a>创建容器

始终将 Blob 上传到目录中。 这样，就能够整理 blob 组，就像在计算机的文件夹中整理文件一样。

要创建目录，请展开在前面的步骤中创建的存储帐户。 选择“Blob 容器”，然后右键单击并选择“创建 Blob 容器”   。 输入容器的名称。 完成后，请按 **Enter** 创建容器。 成功创建 Blob 目录后，该目录会显示在所选存储帐户的“Blob 容器”文件夹下  。

![Microsoft Azure 存储资源管理器 - 创建容器](media/storage-quickstart-blobs-storage-explorer/creating-a-filesystem.png)

## <a name="upload-blobs-to-the-directory"></a>将 blob 上传到目录

Blob 存储支持块 blob、追加 blob 和页 blob。 用于备份 IaaS VM 的 VHD 文件都是页 blob。 追加 blob 用于日志记录，例如有时需要写入到文件，再继续添加更多信息。 Blob 存储中存储的大多数文件都是块 blob。

在目录功能区中，选择“上传”  。 此操作提供上传文件夹或文件的选项。

选择要上传的文件或文件夹。

在“上传到文件夹(可选)”字段中输入一个文件夹名称，用于在目录下的文件夹中存储文件或文件夹  。 如果没有选择任何文件夹，文件会直接上传到目录。

![Microsoft Azure 存储资源管理器 - 上传 Blob](media/storage-quickstart-blobs-storage-explorer/uploadblob.png)

选择“确定”后，选定的文件会排队等待上传，然后上传每个文件。  上传完成后，结果将显示在“活动”窗口中。 

## <a name="view-blobs-in-a-directory"></a>查看目录中的 Blob

在 Azure 存储资源管理器应用程序的存储帐户下选择一个目录  。 主窗格会显示一个列表，包含所选目录中的所有 Blob。

![Microsoft Azure 存储资源管理器 - 列出目录中的所有 Blob](media/storage-quickstart-blobs-storage-explorer/listblobs.png)

## <a name="download-blobs"></a>下载 Blob

若要使用“Azure 存储资源管理器”下载 Blob，请选择所需的 Blob，然后在功能区中选择“下载”。   此时将打开文件对话框，可在其中输入文件名。 选择“保存”，开始将 Blob 下载到本地位置。 

## <a name="next-steps"></a>后续步骤

在此快速入门教程中，我们学习了如何使用 **Azure 存储资源管理器**在本地磁盘与 Azure Blob 存储之间传输文件。 要了解如何在文件和目录上设置 ACL，请继续阅读该主题的“操作说明”。

> [!div class="nextstepaction"]
> [如何在文件和目录上设置 ACL](data-lake-storage-how-to-set-permissions-storage-explorer.md)
