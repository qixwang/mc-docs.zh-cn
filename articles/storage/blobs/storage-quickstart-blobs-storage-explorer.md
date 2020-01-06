---
title: 快速入门 - 使用 Azure 存储资源管理器创建 blob
titleSuffix: Azure Storage
description: 本快速入门介绍如何使用 Azure 存储资源管理器创建容器和 blob、将 blob 下载到本地计算机，以及查看容器中的所有 blob。
services: storage
author: WenJason
ms.custom: mvc
ms.service: storage
ms.topic: quickstart
origin.date: 12/04/2019
ms.date: 01/06/2020
ms.author: v-jay
ms.openlocfilehash: 112f657e25680efedf616461807fd7768c936b46
ms.sourcegitcommit: 6a8bf63f55c925e0e735e830d67029743d2c7c0a
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/03/2020
ms.locfileid: "75624277"
---
# <a name="quickstart-use-azure-storage-explorer-to-create-a-blob"></a>快速入门：使用 Azure 存储资源管理器创建 blob

本快速入门介绍如何使用 [Azure 存储资源管理器](https://azure.microsoft.com/features/storage-explorer/)创建容器和 blob。 接下来，介绍如何将 blob 下载到本地计算机，以及如何查看容器中的所有 blob。 还介绍如何创建 blob 的快照、管理容器访问策略以及创建共享访问签名。

## <a name="prerequisites"></a>先决条件

[!INCLUDE [storage-quickstart-prereq-include](../../../includes/storage-quickstart-prereq-include.md)]

本快速入门要求安装 Azure 存储资源管理器。 若要安装适用于 Windows、Macintosh 或 Linux 的 Azure 存储资源管理器，请参阅 [Azure 存储资源管理器](https://azure.microsoft.com/features/storage-explorer/)。

## <a name="log-in-to-storage-explorer"></a>登录到存储资源管理器

首次启动时，会显示“Microsoft Azure 存储资源管理器 - 连接”窗口。  存储资源管理器提供多种连接到存储帐户的方式。 下表列出了连接方式的差别：

|任务|目的|
|---|---|
|添加 Azure 帐户 | 将你重定向到组织的登录页，向 Azure 进行身份验证。 |
|使用连接字符串或共享访问签名 URI | 可用于通过 SAS 令牌或共享连接字符串直接访问容器或存储帐户。 |
|使用存储帐户名称和密钥| 使用存储帐户的存储帐户名称和密钥连接到 Azure 存储。|

选择“添加 Azure 帐户”  ，并单击“登录”  。遵照屏幕提示登录到 Azure 帐户。

![“Microsoft Azure 存储资源管理器 - 连接”窗口](media/storage-quickstart-blobs-storage-explorer/connect.png)

完成连接以后，Azure 存储资源管理器会进行加载并显示“资源管理器”选项卡。  以下视图可以查看通过 [Azure 存储模拟器](../common/storage-use-emulator.md?toc=%2fstorage%2fblobs%2ftoc.json)、[Cosmos DB](../../cosmos-db/storage-explorer.md?toc=%2fstorage%2fblobs%2ftoc.json) 帐户或 [Azure Stack](/azure-stack/user/azure-stack-storage-connect-se?toc=%2fstorage%2fblobs%2ftoc.json) 环境配置的所有 Azure 存储帐户和本地存储。

![“Microsoft Azure 存储资源管理器 - 连接”窗口](media/storage-quickstart-blobs-storage-explorer/mainpage.png)

## <a name="create-a-container"></a>创建容器

始终将 Blob 上传到容器中。 这样，就能够整理 blob 组，就像在计算机的文件夹中整理文件一样。

若要创建容器，请展开在前一步骤中创建的存储帐户。 选择并右键单击“Blob 容器”，然后选择“创建 Blob 容器”。   输入 Blob 容器的名称。 有关命名 blob 容器的规则和限制的列表，请参阅[创建容器](storage-quickstart-blobs-dotnet.md#create-a-container)部分。 完成后，请按 **Enter** 创建 Blob 容器。 成功创建 Blob 容器后，该容器将显示在所选存储帐户的“Blob 容器”文件夹下。 

## <a name="upload-blobs-to-the-container"></a>将 blob 上传到容器

blob 存储支持块 blob、追加 blob 和页 blob。 用于备份 IaaS VM 的 VHD 文件都是页 blob。 追加 blob 用于日志记录，例如有时需要写入到文件，再继续添加更多信息。 Blob 存储中存储的大多数文件都是块 blob。

在容器功能区中选择“上传”。  此操作提供上传文件夹或文件的选项。

选择要上传的文件或文件夹。 选择“Blob 类型”。  可接受的选项包括“追加”、“页 Blob”或“块 Blob”。   

若要上传 .vhd 或 .vhdx 文件，请选择“以页 Blob 形式上传 .vhd/.vhdx 文件(推荐)”。 

在“上传到文件夹(可选)”字段中，请输入容器下面用于存储文件或文件夹的某个文件夹的名称。  如果未选择任何文件夹，则文件将直接上传到容器下面。

![Microsoft Azure 存储资源管理器 - 上传 Blob](media/storage-quickstart-blobs-storage-explorer/uploadblob.png)

选择“确定”后，选定的文件会排队等待上传，然后上传每个文件。  上传完成后，结果将显示在“活动”窗口中。 

## <a name="view-blobs-in-a-container"></a>查看容器中的 Blob

在“Azure 存储资源管理器”应用程序中，选择存储帐户下的某个容器。  主窗格将显示选定容器中的 Blob 列表。

![Microsoft Azure 存储资源管理器 - 列出容器中的 Blob](media/storage-quickstart-blobs-storage-explorer/listblobs.png)

## <a name="download-blobs"></a>下载 Blob

若要使用“Azure 存储资源管理器”下载 Blob，请选择所需的 Blob，然后在功能区中选择“下载”。   此时将打开文件对话框，可在其中输入文件名。 选择“保存”，开始将 Blob 下载到本地位置。 

## <a name="manage-snapshots"></a>管理快照

Azure 存储资源管理器提供创建和管理 Blob [快照](storage-blob-snapshots.md)的功能。 若要拍摄 Blob 的快照，请右键单击 Blob，然后选择“创建快照”。  若要查看某个 Blob 的快照，请右键单击该 Blob 并选择“管理快照”。  当前选项卡中会显示 Blob 的快照列表。

![Microsoft Azure 存储资源管理器 - 列出容器中的 Blob](media/storage-quickstart-blobs-storage-explorer/snapshots.png)

## <a name="manage-access-policies"></a>管理访问策略

存储资源管理器提供相应的功能用于管理其用户界面中容器的访问策略。 有两种类型的安全访问策略 (SAS)：服务级别和帐户级别。 帐户级别 SAS 以存储帐户为目标，可应用到多个服务和资源。 服务级别 SAS 是针对特定服务下的资源定义的。 若要生成服务级别 SAS，请右键单击任一容器，并选择“管理访问策略...”。  若要生成帐户级别 SAS，请右键单击存储帐户。

选择“添加”以添加新的访问策略，并定义策略的权限。  完成后，请选择“保存”以保存访问策略。  现在，在配置共享访问签名时，可以使用此策略。

## <a name="work-with-shared-access-signatures"></a>使用共享访问签名

可以通过存储资源管理器检索共享访问签名 (SAS)。 右键单击某个存储帐户、容器或 Blob，并选择“获取共享访问签名...”。  选择开始时间和过期时间以及 SAS URL 的权限，并选择“创建”。  系统会提供包含查询字符串的完整 URL 以及查询字符串本身，在下一个屏幕中可以复制这些信息。

![Microsoft Azure 存储资源管理器 - 列出容器中的 Blob](media/storage-quickstart-blobs-storage-explorer/sharedaccesssignature.png)

## <a name="next-steps"></a>后续步骤

在此快速入门教程中，我们学习了如何使用 **Azure 存储资源管理器**在本地磁盘与 Azure Blob 存储之间传输文件。 要深入了解如何使用 Blob 存储，请继续学习 Blob 存储操作说明。

> [!div class="nextstepaction"]
> [Blob 存储操作说明](storage-quickstart-blobs-powershell.md)
