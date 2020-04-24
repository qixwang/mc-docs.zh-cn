---
title: 在 Windows 中排查 Azure 文件问题 | Microsoft Docs
description: 在 Windows 中排查 Azure 文件问题
author: WenJason
ms.service: storage
ms.topic: conceptual
origin.date: 01/02/2019
ms.date: 03/09/2020
ms.author: v-jay
ms.subservice: files
ms.openlocfilehash: 68b9e5c1c0575e454e5dae4009257de6ec9c0660
ms.sourcegitcommit: c1ba5a62f30ac0a3acb337fb77431de6493e6096
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2020
ms.locfileid: "78412597"
---
# <a name="troubleshoot-azure-files-problems-in-windows"></a>在 Windows 中排查 Azure 文件问题

本文列出了从 Windows 客户端连接时与 Azure 文件相关的常见问题， 并提供了这些问题的可能原因和解决方法。 除本文中的疑难解答步骤之外，还可使用 [AzFileDiagnostics](https://gallery.technet.microsoft.com/Troubleshooting-tool-for-a9fa1fe5) ，以确保 Windows 客户端环境满足正确的先决条件。 AzFileDiagnostics 会自动检测本文中提及的大多数症状，并帮助设置环境以获得最佳性能。 还可在 [Azure 文件共享疑难解答](https://support.microsoft.com/help/4022301/troubleshooter-for-azure-files-shares)中找到这些信息，该疑难解答提供相关步骤来帮助解决连接/映射/装载 Azure 文件共享时遇到的问题。

<a id="error5"></a>
## <a name="error-5-when-you-mount-an-azure-file-share"></a>装载 Azure 文件共享时出现错误 5

尝试装载文件共享时，可能会收到以下错误：

- 发生系统错误 5。 访问被拒绝。

### <a name="cause-1-unencrypted-communication-channel"></a>原因 1：通信通道未加密

出于安全原因，如果信道未加密，且未从 Azure 文件共享所在的数据中心尝试连接，则到 Azure 文件共享的连接将受阻。 如果在存储帐户中启用[需要安全传输](/storage/common/storage-require-secure-transfer)设置，则还可以阻止同一数据中心中未加密的连接。 仅当用户的客户端 OS 支持 SMB 加密时，才提供加密的信道。

Windows 8、Windows Server 2012 及更高版本的每次系统协商均要求其包含支持加密的 SMB 3.0。

### <a name="solution-for-cause-1"></a>原因 1 的解决方案

1. 从支持 SMB 加密的客户端（Windows 8、Windows Server 2012 或更高版本）进行连接，或者从用于 Azure 文件共享的 Azure 存储帐户所在数据中心内的虚拟机进行连接。
2. 如果客户端不支持 SMB 加密，请验证是否已在存储帐户上禁用[需要安全传输](/storage/common/storage-require-secure-transfer)设置。

### <a name="cause-2-virtual-network-or-firewall-rules-are-enabled-on-the-storage-account"></a>原因 2：在存储帐户上启用了虚拟网络或防火墙规则 

如果在存储帐户上配置了虚拟网络 (VNET) 和防火墙规则，则将拒绝访问网络流量，除非允许客户端 IP 地址或虚拟网络访问。

### <a name="solution-for-cause-2"></a>原因 2 的解决方案

验证是否已在存储帐户上正确配置虚拟网络和防火墙规则。 若要测试虚拟网络或防火墙规则是否导致此问题，请将存储帐户上的设置临时更改为“允许来自所有网络的访问”  。 若要了解详细信息，请参阅[配置 Azure 存储防火墙和虚拟网络](/storage/common/storage-network-security)。

<a id="error53-67-87"></a>
## <a name="error-53-error-67-or-error-87-when-you-mount-or-unmount-an-azure-file-share"></a>装载或卸载 Azure 文件共享时出现“错误 53”、“错误 67”或“错误 87”

尝试从本地或其他数据中心装载文件共享时，可能会出现以下错误：

- 发生系统错误 53。 找不到网络路径。
- 发生系统错误 67。 找不到网络名称。
- 发生系统错误 87。 参数不正确。

### <a name="cause-1-port-445-is-blocked"></a>原因 1：端口 445 被阻止

如果端口 445 到 Azure 文件数据中心的出站通信受阻，可能会发生系统错误 53 或 67。 如需大致了解允许或禁止从端口 445 进行访问的 ISP，请访问 [TechNet](https://social.technet.microsoft.com/wiki/contents/articles/32346.azure-summary-of-isps-that-allow-disallow-access-from-port-445.aspx)。

若要检查防火墙或 ISP 是否阻止端口 445，请使用 [AzFileDiagnostics](https://gallery.technet.microsoft.com/Troubleshooting-tool-for-a9fa1fe5) 工具或 `Test-NetConnection` cmdlet。 

若要使用 `Test-NetConnection` cmdlet，则必须安装 Azure PowerShell 模块。有关详细信息，请参阅[安装 Azure PowerShell 模块](https://docs.microsoft.com/powershell/azure/install-Az-ps)。 请记得将 `<your-storage-account-name>` 和 `<your-resource-group-name>` 替换为存储帐户的相关名称。

   
    $resourceGroupName = "<your-resource-group-name>"
    $storageAccountName = "<your-storage-account-name>"

    # This command requires you to be logged into your Azure account, run Login-AzAccount -Environment AzureChinaCloud if you haven't
    # already logged in.
    $storageAccount = Get-AzStorageAccount -ResourceGroupName $resourceGroupName -Name $storageAccountName

    # The ComputerName, or host, is <storage-account>.file.core.chinacloudapi.cn for Azure China Regions.
    Test-NetConnection -ComputerName ([System.Uri]::new($storageAccount.Context.FileEndPoint).Host) -Port 445
    
如果连接成功，应会看到以下输出：
    
  
    ComputerName     : <your-storage-account-name>
    RemoteAddress    : <storage-account-ip-address>
    RemotePort       : 445
    InterfaceAlias   : <your-network-interface>
    SourceAddress    : <your-ip-address>
    TcpTestSucceeded : True
 

> [!Note]  
> 以上命令返回存储帐户的当前 IP 地址。 无法保证此 IP 地址相同，它随时可能更改。 不要在任何脚本或防火墙配置中对此 IP 地址进行硬编码。

### <a name="solution-for-cause-1"></a>原因 1 的解决方案

#### <a name="solution-1---unblock-port-445-with-help-of-your-ispit-admin"></a>解决方案 1 - 在 ISP/IT 管理员的帮助下取消阻止端口 445
与 IT 部门或 ISP 配合，向 [Azure IP 范围](https://www.microsoft.com/download/details.aspx?id=42064)开放端口 445 出站通信。

#### <a name="solution-2---use-rest-api-based-tools-like-storage-explorerpowershell"></a>解决方案 2 - 使用基于 REST API 的工具，例如存储资源管理器/Powershell
除了 SMB，Azure 文件存储还支持 REST。 REST 访问可以通过端口 443 进行（标准 tcp）。 有许多工具是用 REST API 编写的，可以给用户带来丰富的 UI 体验。 [存储资源管理器](/vs-azure-tools-storage-manage-with-storage-explorer?tabs=windows)是其中之一。 [下载并安装存储资源管理器](https://azure.microsoft.com/features/storage-explorer/)，然后将其连接到 Azure 文件存储支持的文件共享。 也可使用 [PowerShell](/storage/files/storage-how-to-use-files-powershell)，此工具也使用 REST API。

### <a name="cause-2-ntlmv1-is-enabled"></a>原因 2：NTLMv1 已启用

如果在客户端上启用 NTLMv1 通信，则可能发生系统错误 53 或系统错误 87。 Azure 文件仅支持 NTLMv2 身份验证。 启用 NTLMv1 会降低客户端的安全性。 因此，Azure 文件的通信受阻。 

若要确定这是否是错误原因，请验证以下注册表子项是否设为值 3：

**HKLM\SYSTEM\CurrentControlSet\Control\Lsa > LmCompatibilityLevel**

有关详细信息，请参阅 TechNet 上的 [LmCompatibilityLevel](https://technet.microsoft.com/library/cc960646.aspx) 主题。

### <a name="solution-for-cause-2"></a>原因 2 的解决方案

在下述注册表子项中将 **LmCompatibilityLevel** 值还原为默认值 3：

  **HKLM\SYSTEM\CurrentControlSet\Control\Lsa**

<a id="error1816"></a>
## <a name="error-1816-not-enough-quota-is-available-to-process-this-command-when-you-copy-to-an-azure-file-share"></a>复制到 Azure 文件共享时出现错误 1816“处理此命令时没有足够的配额可用”

### <a name="cause"></a>原因

达到并发开放句柄数的上限时，会发生错误 1816。在计算机中装载文件共享时，会为计算机中的文件启用开放句柄。

### <a name="solution"></a>解决方案

关闭一些句柄，减少并发打开句柄的数量，再重试。 有关详细信息，请参阅 [Azure 存储性能和可伸缩性核对清单](../common/storage-performance-checklist.md?toc=%2fstorage%2ffiles%2ftoc.json)。

若要查看文件共享、目录或文件的打开句柄，请使用 [Get-AzStorageFileHandle](https://docs.microsoft.com/powershell/module/az.storage/get-azstoragefilehandle) PowerShell cmdlet。  

若要关闭文件共享、目录或文件的打开句柄，请使用 [Close-AzStorageFileHandle](https://docs.microsoft.com/powershell/module/az.storage/close-azstoragefilehandle) PowerShell cmdlet。

> [!Note]  
> Get-AzStorageFileHandle 和 Close-AzStorageFileHandle cmdlet 包括在 Az PowerShell 模块 2.4 或更高版本中。 若要安装最新 Az PowerShell 模块，请参阅[安装 Azure PowerShell 模块](https://docs.microsoft.com/powershell/azure/install-az-ps)。

<a id="noaaccessfailureportal"></a>
## <a name="error-no-access-when-you-try-to-access-or-delete-an-azure-file-share"></a>尝试访问或删除 Azure 文件共享时出现错误“无访问权限”  
尝试访问或删除门户中的 Azure 文件共享时，可能会收到以下错误：

无访问权限  
错误代码：403 

### <a name="cause-1-virtual-network-or-firewall-rules-are-enabled-on-the-storage-account"></a>原因 1：在存储帐户上启用了虚拟网络或防火墙规则

### <a name="solution-for-cause-1"></a>原因 1 的解决方案

验证是否已在存储帐户上正确配置虚拟网络和防火墙规则。 若要测试虚拟网络或防火墙规则是否导致此问题，请将存储帐户上的设置临时更改为“允许来自所有网络的访问”  。 若要了解详细信息，请参阅[配置 Azure 存储防火墙和虚拟网络](/storage/common/storage-network-security)。

### <a name="cause-2-your-user-account-does-not-have-access-to-the-storage-account"></a>原因 2：你的用户帐户无权访问该存储帐户

### <a name="solution-for-cause-2"></a>原因 2 的解决方案

浏览到Azure文件共享所在的存储帐户，单击“访问控制(IAM)”，确保你的用户帐户有权访问该存储帐户  。 若要了解详细信息，请参阅[如何使用基于角色的访问控制 (RBAC) 来保护存储帐户](/storage/blobs/security-recommendations#data-protection)。

<a id="open-handles"></a>
## <a name="unable-to-delete-a-file-or-directory-in-an-azure-file-share"></a>无法删除 Azure 文件共享中的文件或目录
尝试删除文件时，可能会收到以下错误：

SMB 客户端已将指定的资源标记为删除。

### <a name="cause"></a>原因
如果该文件或目录有一个打开的句柄，通常会出现此问题。 

### <a name="solution"></a>解决方案

如果 SMB 客户端关闭了所有打开的句柄，但问题仍然出现，请执行以下操作：

- 使用 [Get-AzStorageFileHandle](https://docs.microsoft.com/powershell/module/az.storage/get-azstoragefilehandle) PowerShell cmdlet 查看打开的句柄。

- 使用 [Close-AzStorageFileHandle](https://docs.microsoft.com/powershell/module/az.storage/close-azstoragefilehandle) PowerShell cmdlet 关闭打开的句柄。 

> [!Note]  
> Get-AzStorageFileHandle 和 Close-AzStorageFileHandle cmdlet 包括在 Az PowerShell 模块 2.4 或更高版本中。 若要安装最新 Az PowerShell 模块，请参阅[安装 Azure PowerShell 模块](https://docs.microsoft.com/powershell/azure/install-az-ps)。

<a id="slowfilecopying"></a>
## <a name="slow-file-copying-to-and-from-azure-files-in-windows"></a>在 Windows 中将文件复制到 Azure 文件以及从中复制文件时速度缓慢

尝试将文件传输到 Azure 文件服务时，可能会出现性能下降的情况。

- 如果你没有特定的 I/O 大小下限要求，我们建议使用 1 MiB 的 I/O 大小以获得最佳性能。
- 如果知道要通过写入进行扩展的文件的最终大小，并且软件在文件上未写入的尾部包含零时尚未出现兼容性问题，请提前设置文件大小，而不是使每次写入都成为扩展写入。
- 使用正确的复制方法：
    - 为两个文件共享之间的任何传输使用 [AzCopy](../common/storage-use-azcopy.md?toc=%2fstorage%2ffiles%2ftoc.json)。
    - 在本地计算机上的文件共享之间使用 [Robocopy](/storage/files/storage-files-deployment-guide#robocopy)。

### <a name="considerations-for-windows-81-or-windows-server-2012-r2"></a>Windows 8.1 或 Windows Server 2012 R2 的注意事项

对于运行 Windows 8.1 或 Windows Server 2012 R2 的客户端，请确保安装有 [KB3114025](https://support.microsoft.com/help/3114025) 修补程序。 该修补程序可提升创建和关闭句柄时的性能。

可运行以下脚本，检查是否安装了该修补程序：

`reg query HKLM\SYSTEM\CurrentControlSet\Services\LanmanWorkstation\Parameters\Policies`

如果已安装，显示以下输出：

`HKEY_Local_MACHINE\SYSTEM\CurrentControlSet\Services\LanmanWorkstation\Parameters\Policies {96c345ef-3cac-477b-8fcd-bea1a564241c} REG_DWORD 0x1`

> [!Note]
> 自 2015 年 12 月起，Azure 市场中的 Windows Server 2012 R2 映像将默认安装修补程序 KB3114025。

<a id="shareismissing"></a>
## <a name="no-folder-with-a-drive-letter-in-my-computer-or-this-pc"></a>“我的电脑”或“这台电脑”中没有带驱动器号的文件夹

如果以管理员身份使用 net use 来映射 Azure 文件共享，则会缺失共享。

### <a name="cause"></a>原因

默认情况下，Windows 文件资源管理器不以管理员身份运行。 如果通过管理性命令提示符运行 net use，则可以管理员身份映射网络驱动器。 由于映射的驱动器以用户为中心，如果在其他用户帐户上安装了这些驱动器，则登录的用户帐户不会显示它们。

### <a name="solution"></a>解决方案
通过非管理员命令行中装载共享。 或者，可按照[此 TechNet 主题](https://technet.microsoft.com/library/ee844140.aspx)配置 **EnableLinkedConnections** 注册表值。

<a id="netuse"></a>
## <a name="net-use-command-fails-if-the-storage-account-contains-a-forward-slash"></a>如果存储帐户包含正斜杠，则 net use 命令会失败

### <a name="cause"></a>原因

net use 命令将正斜杠 (/) 解释为命令行选项。 如果用户帐户名称以正斜杠开头，则驱动器映射会失败。

### <a name="solution"></a>解决方案

可使用下述某个步骤解决此问题：

- 运行以下 PowerShell 命令：

  `New-SmbMapping -LocalPath y: -RemotePath \\server\share -UserName accountName -Password "password can contain / and \ etc"`

  可以在批处理文件中通过以下方式运行该命令：

  `Echo new-smbMapping ... | powershell -command -`

- 将密钥用双引号括起以解决此问题（除非第一个字符是正斜杠）。 如果是，可使用交互模式并单独输入密码，或者重新生成密钥来获取不以正斜杠开头的密钥。

<a id="cannotaccess"></a>
## <a name="application-or-service-cannot-access-a-mounted-azure-files-drive"></a>应用程序或服务无法访问装载的 Azure 文件驱动器

### <a name="cause"></a>原因

根据用户装载驱动器。 如果在运行应用程序或服务时，所使用的用户帐户不同于装载驱动器的帐户，则应用程序看不到该驱动器。

### <a name="solution"></a>解决方案

请使用以下解决方案之一：

- 使用应用程序所在的同一用户帐户装载驱动器。 可以使用 PsExec 之类的工具。
- 传递 net use 命令的用户名和密码参数中的存储帐户名和密钥。
- 使用 cmdkey 命令将凭据添加到凭据管理器中。 通过交互式登录或使用 `runas`，在服务帐户上下文中从命令行执行此操作。
  
  `cmdkey /add:<storage-account-name>.file.core.chinacloudapi.cn /user:AZURE\<storage-account-name> /pass:<storage-account-key>`
- 不使用映射驱动器号直接映射共享。 某些应用程序可能无法正确地重新连接到驱动器号，因此使用完整的 UNC 路径可能会更可靠。 

  `net use * \\storage-account-name.file.core.chinacloudapi.cn\share`

按这些说明操作以后，可能会在为系统/网络服务帐户运行 net use 时出现以下错误消息：“发生系统错误 1312。 指定的登录会话不存在。 可能已终止该会话。” 若发生此情况，请确保传递到 net use 的用户名包括域信息（例如“[storage account name].file.core.chinacloudapi.cn”）。

<a id="doesnotsupportencryption"></a>
## <a name="error-you-are-copying-a-file-to-a-destination-that-does-not-support-encryption"></a>“正在将文件复制到不支持加密的目标”错误

通过网络复制文件时，会在源计算机上解密该文件，以明文形式进行传输，然后在目标计算机上重新加密该文件。 但是，尝试复制加密的文件时可能会看到以下错误：“正在将文件复制到不支持加密的目标。”

### <a name="cause"></a>原因
如果你使用的是加密文件系统 (EFS)，则可能会出现此问题。 可将 BitLocker 加密的文件复制到 Azure 文件。 不过，Azure 文件不支持 NTFS EFS。

### <a name="workaround"></a>解决方法
若要通过网络复制文件，必须先解密该文件。 使用以下方法之一：

- 使用  copy /d 命令。 该命令可以在目标计算机上将加密的文件另存为解密的文件。
- 设置以下注册表项：
  - 路径 = HKLM\Software\Policies\Microsoft\Windows\System
  - 值类型 = DWORD
  - 名称= CopyFileAllowDecryptedRemoteDestination
  - 值= 1

请注意，设置注册表项会影响所有针对网络共享进行的复制操作。

## <a name="slow-enumeration-of-files-and-folders"></a>文件和文件夹的枚举速度变慢

### <a name="cause"></a>原因

如果客户端计算机上用于大型目录的缓存不足，则可能会出现此问题。

### <a name="solution"></a>解决方案

若要解决此问题，请调整 DirectoryCacheEntrySizeMax 注册表值以允许在客户端计算机上缓存较大的目录列表  ：

- 位置：HKLM\System\CCS\Services\Lanmanworkstation\Parameters
- 值名称：DirectoryCacheEntrySizeMax 
- 值类型：DWORD
 
 
例如，可将其设置为 0x100000，并查看性能是否有所提高。

## <a name="need-help-contact-support"></a>需要帮助？ 请联系支持人员。
如果仍需帮助，请[联系支持人员](https://portal.azure.cn/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade)，以快速解决问题。
