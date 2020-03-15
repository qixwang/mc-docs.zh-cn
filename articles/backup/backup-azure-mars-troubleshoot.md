---
title: 排查 Azure 备份代理问题
description: 本文介绍如何排查 Azure 备份代理的安装和注册问题。
ms.reviewer: saurse
author: lingliw
ms.topic: troubleshooting
origin.date: 07/15/2019
ms.date: 09/15/2019
ms.author: v-lingwu
ms.openlocfilehash: c9ff4197ec8acdf20f4e6419eccc1864c28853f1
ms.sourcegitcommit: 3c98f52b6ccca469e598d327cd537caab2fde83f
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/13/2020
ms.locfileid: "79290874"
---
# <a name="troubleshoot-the-microsoft-azure-recovery-services-mars-agent"></a>排查 Microsoft Azure 恢复服务 (MARS) 代理问题

本文介绍如何解决在配置、注册、备份和还原期间可能会出现的错误。

## <a name="basic-troubleshooting"></a>基本故障排除

我们建议在开始排查 Microsoft Azure 恢复服务 (MARS) 代理问题之前检查以下各项：

- [确保 MARS 代理是最新的](https://go.microsoft.com/fwlink/?linkid=229525&clcid=0x409)。
- [确保已在 MARS 代理与 Azure 之间建立网络连接](https://aka.ms/AB-A4dp50)。
- 确保 MARS 正在运行（在服务控制台中）。 如果需要，请在重启后重试操作。
- [确保暂存文件夹位置有 5% 到 10% 的可用卷空间](https://aka.ms/AB-AA4dwtt)
- [检查其他进程或防病毒软件是否正在干扰 Azure 备份](https://aka.ms/AB-AA4dwtk)。
- 如果计划的备份失败，但手动备份可正常进行，请参阅[备份不按计划运行](https://aka.ms/ScheduledBackupFailManualWorks)。
- 确保 OS 中已安装最新的更新。
- [确保从备份中排除使用不受支持的属性的不受支持驱动器和文件](backup-support-matrix-mars-agent.md#supported-drives-or-volumes-for-backup)。
- 确保受保护系统上的时钟配置为正确时区。
- [确保在服务器上安装 .NET Framework 4.5.2 或更高版本](https://www.microsoft.com/download/details.aspx?id=30653)。
- 如果你正在尝试将服务器注册到保管库：
  - 确保在服务器上卸载代理并将其从门户中删除。
  - 使用最初用来注册服务器的相同通行短语。
- 对于脱机备份，请确保在开始备份之前，Azure PowerShell 3.7.0 已安装在源服务器和复制计算机上。
- 如果备份代理在 Azure 虚拟机上运行，请参阅[此文](https://aka.ms/AB-AA4dwtr)。

## <a name="invalid-vault-credentials-provided"></a>提供的保管库凭据无效

**错误消息**：提供的保管库凭据无效。 该文件已损坏，或者没有与恢复服务关联的最新凭据。 (ID:34513)

| 原因 | 建议的操作 |
| ---     | ---    |
| **保管库凭据无效** <br/> <br/> 保管库凭据文件可能已损坏或过期。 （例如，它们可能是在注册时的 48 以前下载的。）| 请从 Azure 门户上的恢复服务保管库下载新凭据。 （请参阅[下载 MARS 代理](/backup/backup-configure-vault#download-the-mars-agent)部分中的步骤 6。）然后相应地执行以下步骤： <ul><li> 如果已安装并注册 MARS，请打开 Microsoft Azure 备份代理 MMC 控制台，然后在“操作”窗格中选择“注册服务器”，以使用新凭据完成注册。   <br/> <li> 如果新的安装失败，请尝试使用新凭据重新安装。</ul> **注意**：如果已下载多个保管库凭据文件，在接下来的 48 小时，只有最新文件才有效。 我们建议下载新的保管库凭据文件。
| **代理服务器/防火墙正在阻止注册** <br/>或 <br/>**未建立 Internet 连接** <br/><br/> 如果计算机或代理服务器限制了 Internet 连接，并且你无法确保能够访问所需的 URL，则注册将会失败。| 请执行以下步骤：<br/> <ul><li> 与 IT 团队协作，确保系统已建立 Internet 连接。<li> 如果没有代理服务器，请确保在注册代理时不要选择代理选项。 [检查代理设置](#verifying-proxy-settings-for-windows)。<li> 如果你使用了防火墙/代理服务器，请与网络团队协作，确保这些 URL 和 IP 地址能够访问：<br/> <br> **URL**<br> `www.msftncsi.com` <br> .Microsoft.com <br> .WindowsAzure.com <br> .microsoftonline.com <br> .windows.net <br>**IP 地址**<br>  20.190.128.0/18 <br>  40.126.0.0/18 <br/></ul></ul>完成上述故障排除步骤后，再次尝试注册。
| **防病毒软件正在阻止注册** | 如果你在服务器上安装了防病毒软件，请将所需的排除规则添加到这些文件和文件夹的防病毒扫描项中： <br/><ul> <li> CBengine.exe <li> CSC.exe<li> scratch 文件夹。 其默认位置为 C:\Program Files\Microsoft Azure Recovery Services Agent\Scratch。 <li> bin 文件夹 C:\Program Files\Microsoft Azure Recovery Services Agent\Bin。

### <a name="additional-recommendations"></a>其他建议

- 转到 C:/Windows/Temp，检查是否存在超过 60,000 或 65,000 个扩展名为 .tmp 的文件。 如果存在，请删除这些文件。
- 确保计算机的日期和时间与本地时区相匹配。
- 确保已将[这些站点](backup-configure-vault.md#verify-internet-access)添加到 Internet Explorer 中的受信任站点。

### <a name="verifying-proxy-settings-for-windows"></a>验证 Windows 的代理设置

1. 从 [Sysinternals](https://docs.microsoft.com/sysinternals/downloads/psexec) 页下载 PsExec。
1. 在权限提升的命令提示符下运行 `psexec -i -s "c:\Program Files\Internet Explorer\iexplore.exe"`。

   此命令将打开 Internet Explorer。
1. 转到“工具” > “Internet 选项” > “连接” > “局域网设置”。    
1. 检查系统帐户的代理设置。
1. 如果未配置代理但提供了代理详细信息，请删除这些详细信息。
1. 如果已配置代理但代理详细信息不正确，请确保“代理 IP”和“端口”详细信息正确。  
1. 关闭 Internet Explorer。

## <a name="unable-to-download-vault-credential-file"></a>无法下载保管库凭据文件

| 错误   | 建议的操作 |
| ---     | ---    |
|未能下载保管库凭据文件。 (ID:403) | <ul><li> 使用不同的浏览器尝试下载保管库凭据，或执行以下步骤： <ul><li> 启动 Internet Explorer。 按 F12。 </li><li> 转到“网络”选项卡，并清除缓存和 Cookie。  </li> <li> 刷新页面。<br></li></ul> <li> 检查订阅是否已禁用/过期。<br></li> <li> 检查是否有任何防火墙规则阻止下载。 <br></li> <li> 确保未用完保管库的限额（每个保管库 50 台计算机）。<br></li>  <li> 确保用户拥有所需的 Azure 备份权限，可以下载保管库凭据并将服务器注册到保管库。 请参阅[使用基于角色的访问控制管理 Azure 备份恢复点](backup-rbac-rs-vault.md)。</li></ul> |

## <a name="the-microsoft-azure-recovery-service-agent-was-unable-to-connect-to-microsoft-azure-backup"></a>Microsoft Azure 恢复服务代理无法连接到 Microsoft Azure 备份

| 错误  | 可能的原因 | 建议的操作 |
| ---     | ---     | ---    |
| <br /><ul><li>Microsoft Azure 恢复服务代理无法连接到 Microsoft Azure 备份。 (ID:100050)请检查网络设置，并确保能够连接到 Internet。<li>(407) 需要代理身份验证。 |代理正在阻止连接。 |  <ul><li>在 Internet Explorer 中，转到“工具” > “Internet 选项” > “安全性” > “Internet”。     选择“自定义级别”，向下滚动到“文件下载”部分。   选择“启用”  。<p>可能还需要将这些 [URL 和 IP 地址](backup-configure-vault.md#verify-internet-access)添加到 Internet Explorer 中的受信任站点。<li>更改设置以使用代理服务器。 然后提供代理服务器详细信息。<li> 如果计算机的 Internet 访问状态受限，请确保计算机或代理上的防火墙设置允许以下 [URL 和 IP 地址](backup-configure-vault.md#verify-internet-access)： <li>如果服务器中安装了防病毒软件，请从防病毒软件扫描中排除这些文件： <ul><li>CBEngine.exe（而非 dpmra.exe）。<li>CSC.exe（与 .NET Framework 相关）。 服务器上安装的每个 .NET Framework 版本都有一个 CSC.exe。 排除受影响服务器上的所有 .NET Framework 版本的 CSC.exe 文件。 <li>scratch 文件夹或缓存位置。 <br>scratch 文件夹的默认位置或缓存路径为 C:\Program Files\Microsoft Azure Recovery Services Agent\Scratch。<li>bin 文件夹 C:\Program Files\Microsoft Azure Recovery Services Agent\Bin。

## <a name="failed-to-set-the-encryption-key-for-secure-backups"></a>未能设置安全备份的加密密钥

| 错误 | 可能的原因 | 建议的操作 |
| ---     | ---     | ---    |
| <br />无法设置安全备份的加密密钥。 激活未完全成功，但是加密通行短语已保存到以下文件中。 |<li>服务器已注册到另一个保管库。<li>在配置期间，通行短语已损坏。| 从该保管库中取消注册服务器，然后使用新通行短语重新注册。

## <a name="the-activation-did-not-complete-successfully"></a>激活未成功完成

| 错误  | 可能的原因 | 建议的操作 |
|---------|---------|---------|
|<br />激活未成功完成。 由于内部服务错误 [0x1FC07]，当前操作失败。 稍后重试操作。 如果该问题仍然存在，请联系 Microsoft 支持部门。     | <li> scratch 文件夹位于空间不足的卷上。 <li> 错误地移动了 scratch 文件夹。 <li> 缺少 OnlineBackup.KEK 文件。         | <li>升级到[最新版本](https://aka.ms/azurebackup_agent)的 MARS 代理。<li>将 scratch 文件夹或缓存位置移到可用空间相当于备份数据总大小 5% 到 10% 的卷。 若要正确移动缓存位置，请参阅[有关备份文件和文件夹的常见问题](/backup/backup-azure-file-folder-backup-faq#manage-the-backup-cache-folder)中的步骤。<li> 确保 OnlineBackup.KEK 文件存在。 <br>scratch 文件夹的默认位置或缓存路径为 C:\Program Files\Microsoft Azure Recovery Services Agent\Scratch。         |

## <a name="encryption-passphrase-not-correctly-configured"></a>未正确配置加密通行短语

| 错误  | 可能的原因 | 建议的操作 |
|---------|---------|---------|
| <br />错误 34506。 未在此计算机上正确配置存储的加密通行短语。    | <li> scratch 文件夹位于空间不足的卷上。 <li> 错误地移动了 scratch 文件夹。 <li> 缺少 OnlineBackup.KEK 文件。        | <li>升级到[最新版本](https://aka.ms/azurebackup_agent)的 MARS 代理。<li>将 scratch 文件夹或缓存位置移到可用空间相当于备份数据总大小 5% 到 10% 的卷。 若要正确移动缓存位置，请参阅[有关备份文件和文件夹的常见问题](/backup/backup-azure-file-folder-backup-faq#manage-the-backup-cache-folder)中的步骤。<li> 确保 OnlineBackup.KEK 文件存在。 <br>scratch 文件夹的默认位置或缓存路径为 C:\Program Files\Microsoft Azure Recovery Services Agent\Scratch。          |

## <a name="backups-dont-run-according-to-schedule"></a>备份不按计划运行

如果计划的备份未自动触发，而手动备份却能正常进行，请尝试以下操作：

- 确保 Windows Server 备份计划与 Azure 文件和文件夹备份计划不冲突。

- 确保联机备份状态设置为“启用”。  若要验证状态，请执行以下步骤：

  1. 在任务计划程序中，展开“Microsoft”并选择“联机备份”。  
  1. 双击“Microsoft-OnlineBackup”，然后转到“触发器”选项卡。  
  1. 检查状态是否设置为“已启用”。  如果不是，请依次选择“编辑”、“已启用”、“确定”。   

- 确保为运行任务而选择的用户帐户是服务器上的 **SYSTEM** 或**本地管理员组**。 若要验证用户帐户，请转到“常规”选项卡并检查“安全性”选项。  

- 确保服务器上已安装 PowerShell 3.0 或更高版本。 若要检查 PowerShell 版本，请运行以下命令，并确认 `Major` 版本号是否为 3 或更高：

  `$PSVersionTable.PSVersion`

- 确保此路径包含在 `PSMODULEPATH` 环境变量中：

  `<MARS agent installation path>\Microsoft Azure Recovery Services Agent\bin\Modules\MSOnlineBackup`

- 如果 `LocalMachine` 的 PowerShell 执行策略设置为 `restricted`，则触发备份任务的 PowerShell cmdlet 可能会失败。 以权限提升的模式运行以下命令，将执行策略设置为 `Unrestricted` 或 `RemoteSigned`：

 ```PowerShell
 Get-ExecutionPolicy -List

Set-ExecutionPolicy Unrestricted
```

- 确保 PowerShell 模块的 MSOnlineBackup 文件无缺失或损坏。 如果有任何文件缺失或损坏，请执行以下步骤：

  1. 在可正常运行 MARS 代理的任何计算机上，从 C:\Program Files\Microsoft Azure Recovery Services Agent\bin\Modules 复制“MSOnlineBackup”文件夹。
  1. 在有问题的计算机上，将复制的文件粘贴到相同的文件夹位置 (C:\Program Files\Microsoft Azure Recovery Services Agent\bin\Modules)。

     如果该计算机上已有一个 MSOnlineBackup 文件夹，请将文件粘贴到其中，或替换所有现有文件。

> [!TIP]
> 为确保一致地应用所做的更改，请在执行上述步骤后重启服务器。

## <a name="troubleshoot-restore-problems"></a>排查还原问题

即使等待几分钟，Azure 备份也可能不会成功装载恢复卷。 在此过程中，可能会出现错误消息。 若要开始正常恢复，请执行以下步骤：

1. 如果装载过程已运行了几分钟，请取消此过程。

2. 检查是否使用了最新版本的备份代理。 若要检查版本，请在 MARS 控制台的“操作”窗格中，选择“关于 Microsoft Azure 恢复服务代理”。   确认“版本号”等于或高于[此文](https://go.microsoft.com/fwlink/?linkid=229525)中所述的版本。  选择[下载最新版本](https://go.microsoft.com/fwLink/?LinkID=288905)的链接。

3. 转到“设备管理器” > “存储控制器”，并找到“Microsoft iSCSI 发起程序”    。 如果找到，请直接转到步骤 7。

4. 如果找不到 Microsoft iSCSI 发起程序服务，请尝试在“设备管理器” > “存储控制器”下找到硬件 ID 为“ROOT\ISCSIPRT”的“未知设备”条目     。

5. 右键单击“未知设备”并选择“更新驱动程序软件”   。

6. 选择“自动搜索更新的驱动程序软件”选项，更新驱动程序  。 此项更新应会将“未知设备”更改为“Microsoft iSCSI 发起程序”   ：

    ![Azure 备份设备管理器的屏幕截图，其中突出显示了“存储控制器”](./media/backup-azure-restore-windows-server/UnknowniSCSIDevice.png)

7. 转到“任务管理器” > “服务(本地)” > “Microsoft iSCSI 发起程序服务”    ：

    ![Azure 备份任务管理器的屏幕截图，其中突出显示了“服务(本地)”](./media/backup-azure-restore-windows-server/MicrosoftInitiatorServiceRunning.png)

8. 重启 Microsoft iSCSI 发起程序服务。 为此，请右键单击该服务，并选择“停止”  。 然后再次右键单击它并选择“启动”  。

9. 使用 [即时还原](backup-instant-restore-capability.md) 重试恢复。

如果恢复仍然失败，请重启服务器或客户端。 如果不想要重启，或者即使重启服务器，恢复也仍然失败，请尝试[从另一台计算机恢复](backup-azure-restore-windows-server.md#use-instant-restore-to-restore-data-to-an-alternate-machine)。

## <a name="troubleshoot-cache-problems"></a>排查缓存问题

如果缓存文件夹（也称为 scratch 文件夹）配置不正确、缺少必备项或具有受限访问权限，则备份操作可能会失败。

## <a name="need-help-contact-support"></a>需要帮助？ 联系支持人员
如果仍需要帮助，可 [联系支持人员](https://www.azure.cn/support/support-azure/) 来快速解决问题。

## <a name="next-steps"></a>后续步骤
* 详细了解[如何使用 Azure 备份代理备份 Windows Server](tutorial-backup-windows-server-to-azure.md)。
* 如果需要还原备份，请参阅[将文件还原到 Windows 计算机](backup-azure-restore-windows-server.md)。

<!-- Update_Description: wording update -->