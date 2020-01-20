---
title: 从 Azure 下载市场项并发布到 Azure Stack | Microsoft Docs
description: 了解如何从 Azure 下载市场项并发布到 Azure Stack。
services: azure-stack
documentationcenter: ''
author: WenJason
manager: digimobile
editor: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
origin.date: 12/23/2019
ms.date: 01/13/2020
ms.author: v-jay
ms.reviewer: avishwan
ms.lastreviewed: 12/23/2018
ms.openlocfilehash: 38f6bfa5fc4f244d332be1869a557be4582592ca
ms.sourcegitcommit: 166549d64bbe28b28819d6046c93ee041f1d3bd7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/08/2020
ms.locfileid: "75737817"
---
# <a name="download-marketplace-items-to-azure-stack-hub"></a>将市场项下载到 Azure Stack Hub 

云操作员可从市场将项下载 Azure Stack Hub，并使这些项可供所有使用 Azure Stack Hub 环境的用户使用。 可以选择的项来自 Azure 市场项的有序列表，这些项已预先经过测试，支持与 Azure Stack 配合使用。 其他项会不断地添加到此列表中，因此请不时地返回查看新内容。

有两种下载市场产品的场景：

- **联网场景**：需将 Azure Stack Hub 环境连接到 Internet。 使用 Azure Stack Hub 管理员门户查找和下载项。
- **离线或部分联网场景**：需使用市场联合集成工具访问 Internet 来下载市场项。 然后，将下载内容传输到离线 Azure Stack 安装中。 此场景使用 PowerShell。

有关可下载的市场项的完整列表，请参阅 [Azure Stack 的 Azure 市场项](azure-stack-marketplace-azure-items.md)。 有关 Azure Stack 市场的最新添加、删除和更新的列表，请参阅 [Azure Stack 市场更改](azure-stack-marketplace-changes.md)一文。

> [!NOTE]
> 目录根据 Azure Stack Hub 系统连接到的云而异。 云环境由用于注册 Azure Stack Hub 的 Azure 订阅确定。

## <a name="connected-scenario"></a>联网场景

如果 Azure Stack Hub 连接到 Internet，则可以使用管理员门户下载市场项。

### <a name="prerequisites"></a>必备条件

Azure Stack Hub 部署必须已建立 Internet 连接，并且已注册到 Azure。

### <a name="use-the-portal-to-download-marketplace-items"></a>使用门户下载市场项

1. 登录到 Azure Stack Hub 管理员门户。

2. 下载市场项之前，查看可用的存储空间。 稍后在选择要下载的项时，可将下载大小与可用存储容量进行比较。 如果容量有限，请考虑使用[管理可用空间](azure-stack-manage-storage-shares.md#manage-available-space)的选项。

   若要查看可用空间，请在“区域管理”中选择要浏览的区域，然后转到“资源提供程序” > “存储”：   

   ![在 Azure Stack 管理员门户中查看存储空间](media/azure-stack-download-azure-marketplace-item/storage.png)

3. 打开 Azure Stack Hub 市场并连接到 Azure。 为此，请依次选择“市场管理”  服务、“市场项”  和“从 Azure 中添加”  ：

   ![从 Azure 添加市场项](media/azure-stack-download-azure-marketplace-item/marketplace.png)

4. 每个行项还显示当前可用版本。 如果某个市场项有多个可用版本，“版本”列会显示“多个”。   可以单击每个项查看其说明和附加信息，包括其下载大小：

   ![从 Azure 添加](media/azure-stack-download-azure-marketplace-item/add-from-azure1.png)

5. 如果项的版本显示为“多个”，则你可以选择该项，然后从生成的版本选择器下拉列表中选择特定的版本： 

   ![从 Azure 添加](media/azure-stack-download-azure-marketplace-item/add-from-azure3.png)

6. 选择所需的项，然后选择“下载”  。 下载时间根据网络连接性能而异。 下载完成后，可以 Azure Stack 操作员或用户的身份部署新市场项。

7. 若要部署下载的项，请选择“+ 创建资源”，在类别中搜索该新市场项。  接下来，选择该项以开始部署过程。 该过程根据市场项的不同而异。

## <a name="disconnected-or-a-partially-connected-scenario"></a>离线场景或部分联网场景

如果 Azure Stack Hub 受限或未建立 Internet 连接，可以使用 PowerShell 和*市场联合工具*，将市场项下载到已建立 Internet 连接的计算机。 然后，将这些项传输到 Azure Stack Hub 环境。 在离线环境中，无法使用 Azure Stack Hub 门户下载市场项。

也可以在联网场景中使用市场联合工具。

此方案包含两个部分：

- **第 1 部分**：从市场项下载。 在能够访问 Internet 的计算机上配置 PowerShell，下载联合工具，然后从 Azure 市场下载项。
- **第 2 部分**：上传并发布到 Azure Stack Hub 市场。 将下载的文件移到 Azure Stack Hub 环境，将其导入 Azure Stack，然后将其发布到 Azure Stack 市场。

### <a name="prerequisites"></a>必备条件

- 联网环境（不必是 Azure Stack Hub）。 需要建立连接才能获取 Azure 中的产品列表及其详细信息，并在本地下载所有项。 完成此操作后，剩余的过程无需建立 Internet 连接。 此过程将创建以前下载的项的目录，供你在离线环境中使用。

- 一个用于连接离线环境和传输所有必要项目的可移动媒体。

- 符合以下先决条件的离线 Azure Stack Hub 环境：

  - Azure Stack 部署必须已注册到 Azure。

  - 已建立 Internet 连接的计算机上必须已安装 **Azure Stack PowerShell 模块版本 1.2.11** 或更高版本。 如果未安装，请[安装 Azure Stack 特定的 PowerShell 模块](azure-stack-powershell-install.md)。

  - 若要启用已下载市场项的导入，必须配置 [Azure Stack 操作员的 PowerShell 环境](azure-stack-powershell-configure-admin.md)。

  - 克隆 [Azure Stack 工具](https://github.com/Azure/AzureStack-Tools) GitHub 存储库。

- 必须在 Azure Stack 中拥有一个包含可公开访问的容器（存储 Blob）的[存储帐户](azure-stack-manage-storage-accounts.md)。 该容器将用作市场项库文件的临时存储。 如果你不熟悉存储帐户和容器，请参阅 Azure 文档中的[使用 Blob - Azure 门户](/storage/blobs/storage-quickstart-blobs-portal)。

- 在执行第一个过程期间，将下载市场联合工具。

- 可以安装 [AzCopy](/storage/common/storage-use-azcopy) 以获得最佳下载性能，但此工具不是必需的。

注册后，可以忽略市场管理边栏选项卡上显示的以下消息，因为此消息与离线用例无关：

![市场管理](media/azure-stack-download-azure-marketplace-item/toolsmsg.png)

### <a name="use-the-marketplace-syndication-tool-to-download-marketplace-items"></a>使用市场联合工具下载市场项

> [!IMPORTANT]
> 每当在离线场景中下载市场项时，都请确保下载市场联合工具。 此脚本经常发生更改，每次下载都应使用最新版本。

1. 在已建立 Internet 连接的计算机上，以管理员身份打开 PowerShell 控制台。

2. 添加已经用来注册过 Azure Stack 的 Azure 帐户。 若要添加帐户，请在 PowerShell 中不结合任何参数运行 **Add-AzureRmAccount**。 系统会提示输入 Azure 帐户凭据。根据帐户的配置，可能需要使用双因素身份验证。

   > [!NOTE]
   > 如果会话过期，密码已更改，或者只是希望切换帐户，请在使用 **Add-AzureRmAccount** 登录之前运行以下 cmdlet：**Remove-AzureRmAccount-Scope Process**。

3. 如果有多个订阅，请运行以下命令，选择已经用于注册的那个订阅：

   ```powershell  
   Get-AzureRmSubscription -SubscriptionID 'Your Azure Subscription GUID' | Select-AzureRmSubscription
   $AzureContext = Get-AzureRmContext
   ```

4. 使用以下脚本下载最新版本的市场联合工具：

   ```powershell
   # Download the tools archive.
   [Net.ServicePointManager]::SecurityProtocol = [Net.SecurityProtocolType]::Tls12
   invoke-webrequest https://github.com/Azure/AzureStack-Tools/archive/master.zip `
     -OutFile master.zip

   # Expand the downloaded files.
   expand-archive master.zip `
     -DestinationPath `
     -Force

   # Change to the tools directory.
   cd .\AzureStack-Tools-master
   ```

5. 通过运行以下命令导入联合模块并启动工具：

   ```powershell  
   Import-Module .\Syndication\AzureStack.MarketplaceSyndication.psm1
   ```

6. 若要导出市场项（例如 VM 映像、扩展或解决方案模板），请运行以下命令。 请将 destination folder path 替换为从 Azure 市场下载的文件的存储位置：

   ```powershell
   Export-AzSOfflineMarketplaceItem -Destination "Destination folder path in quotes" -azCopyDownloadThreads "[optional - AzCopy threads number]" -azureContext $AzureContext
   ```

   若要导出资源提供程序或 PaaS 服务，请运行以下命令：

   ```powershell
   Export-AzSOfflineResourceProvider -destination "Destination folder path" -azCopyDownloadThreads "AzCopy threads number" -azureContext $AzureContext
   ```

   `-azCopyDownloadThreads` 参数是可选的。 仅当网络带宽较低，并且使用的是高级下载时，才应使用此参数。 此选项指定 AzCopy 中的并发操作数目。 如果在低带宽网络中运行，可以指定较低的数量以避免由于资源争用所导致的故障。 有关详细信息，请参阅[此 Azure 文章](/storage/common/storage-use-azcopy#specify-the-number-of-concurrent-operations-to-start)。

   `-azureContext` 参数也是可选的。 如果未指定 Azure 上下文，该 cmdlet 将使用默认的 Azure 上下文。

7. 运行工具后，应会看到下图所示的屏幕，其中列出了可用的 Azure 市场项：

   ![市场项](media/azure-stack-download-azure-marketplace-item/tool1.png)

8. 如果某个市场项有多个可用版本，“版本”列会显示“多个版本”。 ****  **** 如果项的版本显示为“多个版本”，则你可以选择该项，然后从生成的版本选择器窗口中选择特定的版本。 ****

9. 如果尚未安装 Azure 存储工具，将出现以下消息。 若要安装这些工具，请确保下载 [AzCopy](/storage/common/storage-use-azcopy#download-azcopy)：

   ![存储工具](media/azure-stack-download-azure-marketplace-item/vmnew1.png)

10. 选择要下载的项，并记下“版本”的值。 **** 可以按住  **Ctrl** 键选择多个映像。 在下一过程中导入项时，将要引用该版本。 **

    也可通过“添加条件”选项来筛选映像的列表。 ****

11. 选择“确定”，然后查看并接受法律条款。 ****

12. 所需的下载时间取决于项的大小。 下载完成后，该项会出现在脚本中指定的文件夹内。 下载内容中包括一个 VHD 文件（适用于虚拟机）或 .zip 文件（适用于虚拟机扩展和资源提供程序）。 其中还可能包含一个  *.azpkg* 格式的库包（只是一个 .zip 文件）。

13. 如果下载失败，可以重新运行以下 PowerShell cmdlet 来重试下载：

   ```powershell
   # for Marketplace items
   Export-AzSOfflineMarketplaceItem -Destination "Destination folder path in quotes"

   # for Resource providers
   Export-AzSOfflineResourceProvider -Destination "Destination folder path in quotes"
   ```

   重试之前，请删除发生下载失败的产品文件夹。 例如，如果下载脚本在下载到 **D:\downloadFolder\microsoft.customscriptextension-arm-1.9.1**时失败，请删除 **D:\downloadFolder\microsoft.customscriptextension-arm-1.9.1** 文件夹，然后重新运行 cmdlet。

### <a name="import-the-download-and-publish-to-azure-stack-marketplace-using-powershell"></a>使用 PowerShell 导入下载内容并发布到 Azure Stack 市场

1. 必须在本地移动[以前下载](#use-the-marketplace-syndication-tool-to-download-marketplace-items)的文件，使其可供 Azure Stack 环境使用。 市场联合工具也必须可供 Azure Stack 环境使用，因为你需要使用该工具来执行导入操作。

   下图显示了文件夹结构示例。 **D:\downloadfolder** 包含所有已下载的市场项。 每个子文件夹是一个市场项（例如 **microsoft.custom-script-linux-arm-2.0.3**），并按产品 ID 命名。 每个子文件夹包含市场项的下载内容。

   ![市场下载目录结构](media/azure-stack-download-azure-marketplace-item/mp1.png)

2. 遵照[此文](azure-stack-powershell-configure-admin.md)中的说明配置 Azure Stack 操作员 PowerShell 会话。

3. 导入联合模块，然后运行以下脚本来启动市场联合工具：

   ```powershell
   $credential = Get-Credential -Message "Enter the azure stack operator credential:"
   Import-AzSOfflineMarketplaceItem -origin "marketplace content folder" -AzsCredential $credential
   ```

   `-origin` 参数指定包含所有已下载产品的顶级文件夹；例如 `"D:\downloadfolder"`。

   `-AzsCredential` 参数是可选的。 该参数用于续订访问令牌（如果已过期）。 如果未指定 `-AzsCredential` 参数且令牌已过期，则会出现输入操作员凭据的提示。

   > [!NOTE]
   > AD FS 仅支持通过用户标识进行交互式身份验证。 如果需要凭据对象，则必须使用服务主体 (SPN)。 若要详细了解如何在设置服务主体时将 Azure Stack 和 AD FS 作为标识管理服务，请参阅[管理 AD FS 服务主体](azure-stack-create-service-principals.md#manage-an-ad-fs-service-principal)。

4. 成功完成该脚本后，Azure Stack 市场中应会提供该项。
