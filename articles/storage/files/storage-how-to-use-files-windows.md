---
title: 将 Azure 文件共享与 Windows 配合使用 | Microsoft Docs
description: 了解如何在 Windows 和 Windows Server 中使用 Azure 文件共享。
author: WenJason
ms.service: storage
ms.topic: conceptual
origin.date: 06/07/2018
ms.date: 03/09/2020
ms.author: v-jay
ms.subservice: files
ms.openlocfilehash: 453adb66de908d26c5e6b16a0e0f43b99329791c
ms.sourcegitcommit: 3c98f52b6ccca469e598d327cd537caab2fde83f
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/13/2020
ms.locfileid: "79293518"
---
# <a name="use-an-azure-file-share-with-windows"></a>在 Windows 中使用 Azure 文件共享
[Azure 文件](storage-files-introduction.md)是易于使用的云文件系统。 可以在 Windows 和 Windows Server 中无缝使用 Azure 文件共享。 本文介绍在 Windows 和 Windows Server 中使用 Azure 文件共享时的注意事项。

若要在某个 Azure 文件共享的托管 Azure 区域（例如本地或其他 Azure 区域）外部使用该文件共享，OS 必须支持 SMB 3.0。 

可在 Azure VM 或本地运行的 Windows 安装中使用 Azure 文件共享。 下表说明了哪些 OS 版本支持在哪个环境中访问文件共享：

| Windows 版本        | SMB 版本 | 可以在 Azure VM 中装载 | 可以在本地装载 |
|------------------------|-------------|-----------------------|-----------------------|
| Windows Server 2019 | SMB 3.0 | 是 | 是 |
| Windows 10<sup>1</sup> | SMB 3.0 | 是 | 是 |
| Windows Server 半年通道<sup>2</sup> | SMB 3.0 | 是 | 是 |
| Windows Server 2016 | SMB 3.0 | 是 | 是 |
| Windows 8.1 | SMB 3.0 | 是 | 是 |
| Windows Server 2012 R2 | SMB 3.0 | 是 | 是 |
| Windows Server 2012 | SMB 3.0 | 是 | 是 |
| Windows 7<sup>3</sup> | SMB 2.1 | 是 | 否 |
| Windows Server 2008 R2<sup>3</sup> | SMB 2.1 | 是 | 否 |

<sup>1</sup>Windows 10 版本 1507、1607、1709、1803、1809、1903 和 1909。  
<sup>2</sup>Windows Server 版本 1809、1903 和 1909。  
<sup>3</sup>Microsoft 对 Windows 7 和 Windows Server 2008 R2 的常规支持已结束。 只有通过[扩展安全更新 (ESU) 程序](https://support.microsoft.com/help/4497181/lifecycle-faq-extended-security-updates)才能购买对安全更新的附加支持。 我们强烈建议从这些操作系统中迁移。

> [!Note]  
> 我们始终建议使用相对于 Windows 版本来说最新的 KB。

## <a name="prerequisites"></a>先决条件 
* **存储帐户名**：若要装载 Azure 文件共享，需要存储帐户的名称。

* **存储帐户密钥**：若要装载 Azure 文件共享，需要主（或辅助）存储密钥。 目前不支持使用 SAS 密钥进行装载。

* **确保端口 445 处于打开状态**：SMB 协议要求 TCP 端口 445 处于打开状态；如果端口 445 已被阻止，连接将会失败。 可以使用 `Test-NetConnection` cmdlet 检查防火墙是否阻止了端口 445。 可以在此处了解[如何通过各种方式来解决端口 445 被阻止的问题](/storage/files/storage-troubleshoot-windows-file-connection-problems#cause-1-port-445-is-blocked)。

    以下 PowerShell 代码假设已安装 Azure PowerShell 模块。有关详细信息，请参阅[安装 Azure PowerShell 模块](https://docs.microsoft.com/powershell/azure/install-az-ps)。 请记得将 `<your-storage-account-name>` 和 `<your-resource-group-name>` 替换为存储帐户的相关名称。

    ```powershell
    $resourceGroupName = "<your-resource-group-name>"
    $storageAccountName = "<your-storage-account-name>"

    # This command requires you to be logged into your Azure account, run Login-AzAccount -Environment AzureChinaCloud if you haven't
    # already logged in.
    $storageAccount = Get-AzStorageAccount -ResourceGroupName $resourceGroupName -Name $storageAccountName

    # The ComputerName, or host, is <storage-account>.file.core.chinacloudapi.cn for Azure China Regions.
    Test-NetConnection -ComputerName ([System.Uri]::new($storageAccount.Context.FileEndPoint).Host) -Port 445
    ```

    如果连接成功，应会看到以下输出：

    ```
    ComputerName     : <storage-account-host-name>
    RemoteAddress    : <storage-account-ip-address>
    RemotePort       : 445
    InterfaceAlias   : <your-network-interface>
    SourceAddress    : <your-ip-address>
    TcpTestSucceeded : True
    ```

    > [!Note]  
    > 以上命令返回存储帐户的当前 IP 地址。 无法保证此 IP 地址相同，它随时可能更改。 不要在任何脚本或防火墙配置中对此 IP 地址进行硬编码。 

## <a name="using-an-azure-file-share-with-windows"></a>在 Windows 中使用 Azure 文件共享
若要在 Windows 中使用某个 Azure 文件共享，必须装载该文件共享（为其分配驱动器号或装载点路径），或通过其 [UNC 路径](https://msdn.microsoft.com/library/windows/desktop/aa365247.aspx)来访问它。 

与其他可以交互的 SMB 共享（例如，托管在 Windows Server、Linux Samba 服务器或 NAS 设备上的共享）不同，Azure 文件共享目前不支持对 Active Directory (AD) 或 Azure Active Directory (AAD) 标识使用 Kerberos 身份验证，不过，我们正在[开发](https://feedback.azure.com/forums/217298-storage/suggestions/6078420-acl-s-for-azurefiles)此功能。 必须使用包含 Azure 文件共享的存储帐户的存储帐户密钥访问该 Azure 文件共享。 存储帐户密钥是存储帐户的管理员密钥，包括对所要访问的文件共享中的所有文件和文件夹的管理员权限，以及对存储帐户中包含的所有文件共享和其他存储资源（Blob、队列、表等）的管理员权限。

将预期需要 SMB 文件共享的业务线 (LOB) 应用程序直接迁移到 Azure 的常见模式是使用 Azure 文件共享，而不是在 Azure VM 中运行专用的 Windows 文件服务器。 成功迁移业务线应用程序以使用 Azure 文件共享的一个重要注意事项是，许多业务线应用程序在具有有限系统权限的专用服务帐户的上下文中运行，而不是在 VM 的管理帐户下运行。 因此，必须确保装载/保存服务帐户上下文（而不是管理帐户）中 Azure 文件共享的凭据。

### <a name="persisting-azure-file-share-credentials-in-windows"></a>在 Windows 中保存 Azure 文件共享凭据  
使用 [cmdkey](https://docs.microsoft.com/windows-server/administration/windows-commands/cmdkey) 实用工具可在 Windows 中存储存储帐户凭据。 这意味着，在尝试通过 Azure 文件共享的 UNC 路径访问该文件共享或装载 Azure 文件共享时，不需要指定凭据。 若要保存存储帐户的凭据，请运行以下 PowerShell 命令（适当替换 `<your-storage-account-name>` 和 `<your-resource-group-name>`）。

```powershell
$resourceGroupName = "<your-resource-group-name>"
$storageAccountName = "<your-storage-account-name>"

# These commands require you to be logged into your Azure account, run Login-AzAccount -Environment AzureChinaCloud if you haven't
# already logged in.
$storageAccount = Get-AzStorageAccount -ResourceGroupName $resourceGroupName -Name $storageAccountName
$storageAccountKeys = Get-AzStorageAccountKey -ResourceGroupName $resourceGroupName -Name $storageAccountName

# The cmdkey utility is a command-line (rather than PowerShell) tool. We use Invoke-Expression to allow us to 
# consume the appropriate values from the storage account variables. The value given to the add parameter of the
# cmdkey utility is the host address for the storage account, <storage-account>.file.core.chinacloudapi.cn for Azure 
# China Regions.
Invoke-Expression -Command ("cmdkey /add:$([System.Uri]::new($storageAccount.Context.FileEndPoint).Host) " + `
    "/user:AZURE\$($storageAccount.StorageAccountName) /pass:$($storageAccountKeys[0].Value)")
```

可以使用 list 参数来验证 cmdkey 实用工具是否已存储存储帐户的凭据：

```powershell
cmdkey /list
```

如果已成功存储 Azure 文件共享的凭据，预期输出将如下所示（列表中可能会存储其他密钥）：

```
Currently stored credentials:

Target: Domain:target=<storage-account-host-name>
Type: Domain Password
User: AZURE\<your-storage-account-name>
```

现在，应该可以装载或访问该共享，而无需提供其他凭据。

#### <a name="advanced-cmdkey-scenarios"></a>高级 cmdkey 场景
对于 cmdkey，需要考虑到其他两种场景：在计算机上存储另一用户（例如某个服务帐户）的凭据，以及使用 PowerShell 远程连接在远程计算机上存储凭据。

可以很轻松地在计算机上存储另一用户的凭据：只需登录到帐户后，执行以下 PowerShell 命令即可：

```powershell
$password = ConvertTo-SecureString -String "<service-account-password>" -AsPlainText -Force
$credential = New-Object System.Management.Automation.PSCredential -ArgumentList "<service-account-username>", $password
Start-Process -FilePath PowerShell.exe -Credential $credential -LoadUserProfile
```

这会在服务帐户（或用户帐户）的用户上下文中打开一个新的 PowerShell 窗口。 然后，可以根据[前面](#persisting-azure-file-share-credentials-in-windows)所述使用 cmdkey 实用工具。

但是，无法使用 PowerShell 远程连接在远程计算机上存储凭据，因为在用户通过 PowerShell 远程连接登录后，cmdkey 不允许访问其凭据存储，即使是在添加组件时。 我们建议使用[远程桌面](https://docs.microsoft.com/windows-server/remote/remote-desktop-services/clients/windows)登录到计算机。

### <a name="mount-the-azure-file-share-with-powershell"></a>使用 PowerShell 装载 Azure 文件共享
在常规的（即权限未提升）PowerShell 会话中运行以下命令来装载 Azure 文件共享。 请记得将 `<your-resource-group-name>`、`<your-storage-account-name>`、`<your-file-share-name>` 和 `<desired-drive-letter>` 替换为适当的信息。

```powershell
$resourceGroupName = "<your-resource-group-name>"
$storageAccountName = "<your-storage-account-name>"
$fileShareName = "<your-file-share-name>"

# These commands require you to be logged into your Azure account, run Login-AzAccount -Environment AzureChinaCloud if you haven't
# already logged in.
$storageAccount = Get-AzStorageAccount -ResourceGroupName $resourceGroupName -Name $storageAccountName
$storageAccountKeys = Get-AzStorageAccountKey -ResourceGroupName $resourceGroupName -Name $storageAccountName
$fileShare = Get-AzStorageShare -Context $storageAccount.Context | Where-Object { 
    $_.Name -eq $fileShareName -and $_.IsSnapshot -eq $false
}

if ($fileShare -eq $null) {
    throw [System.Exception]::new("Azure file share not found")
}

# The value given to the root parameter of the New-PSDrive cmdlet is the host address for the storage account, 
# <storage-account>.file.core.chinacloudapi.cn for Azure China Regions. 
$password = ConvertTo-SecureString -String $storageAccountKeys[0].Value -AsPlainText -Force
$credential = New-Object System.Management.Automation.PSCredential -ArgumentList "AZURE\$($storageAccount.StorageAccountName)", $password
New-PSDrive -Name <desired-drive-letter> -PSProvider FileSystem -Root "\\$($fileShare.StorageUri.PrimaryUri.Host)\$($fileShare.Name)" -Credential $credential -Persist
```

> [!Note]  
> 在 `New-PSDrive` cmdlet 中使用 `-Persist` 选项只允许在启动时重新装载文件共享（如果已保存凭据）。 可根据[前面所述](#persisting-azure-file-share-credentials-in-windows)，使用 cmdkey 来保存凭据。 

如果需要，可以使用以下 PowerShell cmdlet 卸载 Azure 文件共享。

```powershell
Remove-PSDrive -Name <desired-drive-letter>
```

### <a name="mount-the-azure-file-share-with-file-explorer"></a>使用文件资源管理器装载 Azure 文件共享
> [!Note]  
> 请注意，以下说明是在 Windows 10 上显示的，在较旧的版本上可能稍有不同。 

1. 打开文件资源管理器。 可以从“开始”菜单打开，也可以按 Win+E 快捷键打开文件资源管理器。

2. 导航到窗口左侧的“此电脑”项。  这样会更改功能区中的可用菜单。 在“计算机”菜单中，选择“映射网络驱动器”。 
    
    ![“映射网络驱动器”下拉菜单的屏幕截图](./media/storage-how-to-use-files-windows/1_MountOnWindows10.png)

3. 从 Azure 门户的“连接”窗格中复制 UNC 路径。  

    ![Azure 文件“连接”窗格中的 UNC 路径](./media/storage-how-to-use-files-windows/portal_netuse_connect.png)

4. 选择驱动器号并输入 UNC 路径。 
    
    ![“映射网络驱动器”对话框的屏幕截图](./media/storage-how-to-use-files-windows/2_MountOnWindows10.png)

5. 使用带 `AZURE\` 前缀的存储帐户名称作为用户名，使用存储帐户密钥作为密码。
    
    ![网络凭据对话框的屏幕快照](./media/storage-how-to-use-files-windows/3_MountOnWindows10.png)

6. 根据需要使用 Azure 文件共享。
    
    ![Azure 文件共享现已装载](./media/storage-how-to-use-files-windows/4_MountOnWindows10.png)

7. 做好卸载 Azure 文件共享的准备后，可在文件资源管理器中右键单击“网络位置”下对应于共享的条目，并选择“断开连接”。  

### <a name="accessing-share-snapshots-from-windows"></a>从 Windows 访问共享快照
如果已手动或通过脚本或 Azure 备份等服务自动获取共享快照，则可以从 Windows 上的文件共享查看以前版本的共享、目录或特定文件。 可以通过 [Azure 门户](storage-how-to-use-files-portal.md)、[Azure PowerShell](storage-how-to-use-files-powershell.md) 和 [Azure CLI](storage-how-to-use-files-cli.md) 创建共享快照。

#### <a name="list-previous-versions"></a>列出以前版本
浏览到需要还原的项或父项。 通过双击转到所需的目录。 右键单击，然后从菜单中选择“属性”。 

![所选目录的右键单击菜单](./media/storage-how-to-use-files-windows/snapshot-windows-previous-versions.png)

选择"以前版本”  ，以查看此目录的共享快照列表。 列表可能需要几秒钟才能加载，具体要取决于网速和目录中共享快照的数量。

![“以前版本”选项卡](./media/storage-how-to-use-files-windows/snapshot-windows-list.png)

可以选择“打开”  以打开特定快照。 

![打开的快照](./media/storage-how-to-use-files-windows/snapshot-browse-windows.png)

#### <a name="restore-from-a-previous-version"></a>从以前版本还原
选择“还原”  ，以递归方式将整个目录在共享快照创建时包含的内容复制到原始位置。
 ![警告消息中的“还原”按钮](./media/storage-how-to-use-files-windows/snapshot-windows-restore.png) 

## <a name="securing-windowswindows-server"></a>保护 Windows/Windows Server
若要在 Windows 上装载 Azure 文件共享，端口 445 必须可访问。 由于 SMB 1 固有的安全风险，许多组织会阻止端口 445。 SMB 1（也称为通用 Internet 文件系统，简称 CIFS）是 Windows 和 Windows Server 中随附的一个传统文件系统协议。 SMB 1 是一个已过时的低效协议，最重要的是，它不安全。 好消息是 Azure 文件不支持 SMB 1，所有支持的 Windows 和 Windows Server 版本允许删除或禁用 SMB 1。 我们始终[强烈建议](https://aka.ms/stopusingsmb1)在生产环境中使用 Azure 文件共享之前，删除或禁用 Windows 中的 SMB 1 客户端和服务器。

下表提供了有关每个 Windows 版本上 SMB 1 状态的详细信息：

| Windows 版本                           | SMB 1 默认状态 | 禁用/删除方法       | 
|-------------------------------------------|----------------------|-----------------------------|
| Windows Server 2019                       | 已禁用             | 使用 Windows 功能删除 |
| Windows Server 版本 1709+            | 已禁用             | 使用 Windows 功能删除 |
| Windows 10 版本 1709+                | 已禁用             | 使用 Windows 功能删除 |
| Windows Server 2016                       | Enabled              | 使用 Windows 功能删除 |
| Windows 10 版本 1507、1607 和 1703 | Enabled              | 使用 Windows 功能删除 |
| Windows Server 2012 R2                    | Enabled              | 使用 Windows 功能删除 | 
| Windows 8.1                               | Enabled              | 使用 Windows 功能删除 | 
| Windows Server 2012                       | Enabled              | 使用注册表禁用       | 
| Windows Server 2008 R2                    | Enabled              | 使用注册表禁用       |
| Windows 7                                 | Enabled              | 使用注册表禁用       | 

### <a name="auditing-smb-1-usage"></a>审核 SMB 1 使用情况
> 适用于 Windows Server 2019、Windows Server 半年通道（版本 1709 和 1803）、Windows Server 2016、Windows 10（版本 1507、1607、1703、1709 和 1803）、Windows Server 2012 R2 和 Windows 8.1

在环境中删除 SMB 1 之前，可以审核 SMB 1 使用情况，以确定所做的更改是否会中断任何客户端。 如果针对使用 SMB 1 的 SMB 共享发出了任何请求，将在事件日志中的 `Applications and Services Logs > Microsoft > Windows > SMBServer > Audit` 下面记录一个审核事件。 

> [!Note]  
> 若要在 Windows Server 2012 R2 和 Windows 8.1 上启用审核支持，至少应安装 [KB4022720](https://support.microsoft.com/help/4022720/windows-8-1-windows-server-2012-r2-update-kb4022720)。

若要启用审核，请在权限提升的 PowerShell 会话中执行以下 cmdlet：

```powershell
Set-SmbServerConfiguration –AuditSmb1Access $true
```

### <a name="removing-smb-1-from-windows-server"></a>从 Windows Server 中删除 SMB 1
> 适用于 Windows Server 2019、Windows Server 半年通道（版本 1709 和 1803）、Windows Server 2016、Windows Server 2012 R2

若要从 Windows Server 实例中删除 SMB 1，请在权限提升的 PowerShell 会话中执行以下 cmdlet：

```powershell
Remove-WindowsFeature -Name FS-SMB1
```

若要完成删除过程，请重启服务器。 

> [!Note]  
> 从 Windows 10 和 Windows Server 版本 1709 开始，默认不会安装 SMB 1，SMB 1 客户端和 SMB 1 服务器有独立的 Windows 功能。 我们始终建议保持卸载 SMB 1 服务器 (`FS-SMB1-SERVER`) 和 SMB 1 客户端 (`FS-SMB1-CLIENT`)。

### <a name="removing-smb-1-from-windows-client"></a>从 Windows 客户端中删除 SMB 1
> 适用于 Windows 10（版本 1507、1607、1703、1709 和 1803）和 Windows 8.1

若要从 Windows 客户端中删除 SMB 1，请在权限提升的 PowerShell 会话中执行以下 cmdlet：

```powershell
Disable-WindowsOptionalFeature -Online -FeatureName SMB1Protocol
```

若要完成删除过程，请重启电脑。

### <a name="disabling-smb-1-on-legacy-versions-of-windowswindows-server"></a>在早期版本的 Windows/Windows Server 上禁用 SMB 1
> 适用于 Windows Server 2012、Windows Server 2008 R2 和 Windows 7

无法在早期版本的 Windows/Windows Server 上完全删除 SMB 1，但可以通过注册表将其禁用。 若要禁用 SMB 1，请创建 `DWORD` 类型的新注册表项 `SMB1`，并在 `HKEY_LOCAL_MACHINE > SYSTEM > CurrentControlSet > Services > LanmanServer > Parameters` 下面添加值 `0`。

也可以使用以下 PowerShell cmdlet 轻松实现此目的：

```powershell
Set-ItemProperty -Path "HKLM:\SYSTEM\CurrentControlSet\Services\LanmanServer\Parameters" SMB1 -Type DWORD -Value 0 –Force
```

创建此注册表项以后，必须重启服务器才能禁用 SMB 1。

### <a name="smb-resources"></a>SMB 资源
- [Stop using SMB 1](https://blogs.technet.microsoft.com/filecab/2016/09/16/stop-using-smb1/)（停止使用 SMB 1）
- [SMB 1 Product Clearinghouse](https://blogs.technet.microsoft.com/filecab/2017/06/01/smb1-product-clearinghouse/)（SMB 1 产品交换所）
- [Discover SMB 1 in your environment with DSCEA](https://blogs.technet.microsoft.com/ralphkyttle/2017/04/07/discover-smb1-in-your-environment-with-dscea/)（使用 DSCEA 发现环境中的 SMB 1）
- [Disabling SMB 1 through Group Policy](https://blogs.technet.microsoft.com/secguide/2017/06/15/disabling-smbv1-through-group-policy/)（通过组策略禁用 SMB 1）

## <a name="next-steps"></a>后续步骤
请参阅以下链接，获取有关 Azure 文件的更多信息：
- [规划 Azure 文件部署](storage-files-planning.md)
- [常见问题](../storage-files-faq.md)
- [在 Windows 上进行故障排除](storage-troubleshoot-windows-file-connection-problems.md)      
