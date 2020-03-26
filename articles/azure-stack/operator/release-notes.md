---
title: Azure Stack Hub 发行说明
description: Azure Stack Hub 集成系统的发行说明，包括更新和 bug 修复。
author: WenJason
ms.topic: article
origin.date: 03/16/2020
ms.date: 03/23/2020
ms.author: v-jay
ms.reviewer: prchint
ms.lastreviewed: 11/22/2019
ms.openlocfilehash: ddcc86889cee5bc666ea26009a554929fe430713
ms.sourcegitcommit: e500354e2fd8b7ac3dddfae0c825cc543080f476
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/19/2020
ms.locfileid: "79547080"
---
# <a name="azure-stack-hub-release-notes"></a>Azure Stack Hub 发行说明

本文介绍 Azure Stack Hub 更新包的内容。 此更新包括最新版 Azure Stack Hub 的改进和修复。

若要访问不同版本的发行说明，请使用左侧目录上方的版本选择器下拉列表。

::: moniker range=">=azs-1906"
> [!IMPORTANT]  
> 此更新包仅适用于 Azure Stack Hub 集成系统。 请勿将此更新包应用于 Azure Stack 开发工具包 (ASDK)。
::: moniker-end
::: moniker range="<azs-1906"
> [!IMPORTANT]  
> 如果 Azure Stack Hub 实例落后于两个以上的更新，则认为它不符合。 必须[至少更新到最低支持版本才能获得支持](azure-stack-servicing-policy.md#keep-your-system-under-support)。
::: moniker-end

## <a name="update-planning"></a>更新规划

应用更新之前，请务必查看以下信息：

- [已知问题](known-issues.md)
- [安全更新](release-notes-security-updates.md)
- [应用更新之前和之后的活动清单](release-notes-checklist.md)

有关对更新和更新过程进行故障排除的帮助，请参阅[对 Azure Stack Hub 的修补和更新问题进行故障排除](azure-stack-updates-troubleshoot.md)。

<!---------------------------------------------------------->
<!------------------- SUPPORTED VERSIONS ------------------->
<!---------------------------------------------------------->
::: moniker range="azs-1910"
## <a name="1910-build-reference"></a>1910 内部版本参考

Azure Stack Hub 1910 更新内部版本号为 **1.1910.0.58**。

### <a name="update-type"></a>更新类型

从版本 1908 开始，运行 Azure Stack Hub 的底层操作系统已更新为 Windows Server 2019。 此更新可以实现核心基础增强，并将更多功能引入 Azure Stack Hub。

Azure Stack Hub 1910 更新内部版本类型为“快速”  。

与以前的更新相比，1910 更新包更大，因此下载时间更长。 此更新将长时间保留为“正在准备”状态，操作员可预期此过程所需的时间长于以前的更新。  无论 Azure Stack Hub 环境中有多少个物理节点，完成 1910 更新的预估时间都大约为 10 小时。 确切的更新运行时间通常取决于租户工作负荷在系统上使用的容量、系统网络连接（如果已连接到 Internet），以及系统的硬件规格。 运行时间超过预期值并不常见，除非更新失败，否则无需 Azure Stack Hub 操作员采取措施。 此运行时近似值特定于 1910 更新，不应与其他 Azure Stack Hub 更新进行比较。

有关更新内部版本类型的详细信息，请参阅[在 Azure Stack Hub 中管理更新](azure-stack-updates.md)。

<!-- ## What's in this update -->

<!-- The current theme (if any) of this release. -->

### <a name="whats-new"></a>新增功能

<!-- What's new, also net new experiences and features. -->

- 管理员门户现在会在区域属性菜单中显示特权终结点 IP 地址，以方便进行发现。 此外，还会显示当前配置的时间服务器和 DNS 转发器。 有关详细信息，请参阅[使用 Azure Stack Hub 中的特权终结点](azure-stack-privileged-endpoint.md)。

- 现在，在发生错误时，Azure Stack Hub 运行状况和监视系统可针对各种硬件组件引发警报。 这些警报需要额外的配置。 有关详细信息，请参阅[监视 Azure Stack Hub 硬件组件](azure-stack-hardware-monitoring.md)。

- [Azure Stack Hub 的 Cloud-init 支持](/virtual-machines/linux/using-cloud-init)：Cloud-init 是一种广泛使用的方法，用于在首次启动 Linux VM 时对其进行自定义。 可使用 cloud-init 来安装程序包和写入文件，或者配置用户和安全性。 由于是在初始启动过程中调用 cloud-init，因此无需额外的步骤且无需代理来应用配置。 市场中的 Ubuntu 映像已更新为支持使用 cloud-init 进行预配。

- 与 Azure 一样，Azure Stack Hub 现在支持所有 Windows Azure Linux 代理版本。

- 提供了新版 Azure Stack Hub 管理员 PowerShell 模块。 <!-- For more information, see -->

- 已在特权终结点 (PEP) 中添加 **Set-AzSDefenderManualUpdate** cmdlet，用于为 Azure Stack Hub 基础结构中的 Windows Defender 定义配置手动更新。 有关详细信息，请参阅[更新 Azure Stack Hub 上的 Windows Defender Antivirus](azure-stack-security-av.md)。

- 已在特权终结点 (PEP) 中添加 **Get-AzSDefenderManualUpdate** cmdlet，用于为 Azure Stack Hub 基础结构中的 Windows Defender 定义检索手动更新配置。 有关详细信息，请参阅[更新 Azure Stack Hub 上的 Windows Defender Antivirus](azure-stack-security-av.md)。

- 已在特权终结点 (PEP) 中添加 **Set-AzSDnsForwarder** cmdlet，用于在 Azure Stack Hub 中更改 DNS 服务器的转发器设置。 有关 DNS 配置的详细信息，请参阅 [Azure Stack Hub 数据中心 DNS 集成](azure-stack-integrate-dns.md)。

- 已在特权终结点 (PEP) 中添加 **Get-AzSDnsForwarder** cmdlet，用于在 Azure Stack Hub 中检索 DNS 服务器的转发器设置。 有关 DNS 配置的详细信息，请参阅 [Azure Stack Hub 数据中心 DNS 集成](azure-stack-integrate-dns.md)。

### <a name="improvements"></a>改进

<!-- Changes and product improvements with tangible customer-facing value. -->

- Azure Stack Hub 的功能已得到改进，可自动补救一些修补升级问题，过去，这些问题会导致更新失败，或者使操作员无法启动 Azure Stack Hub 更新。 因此，**Test-AzureStack -UpdateReadiness** 组中包含的测试较少。 有关详细信息，请参阅[验证 Azure Stack Hub 系统状态](azure-stack-diagnostic-test.md#groups)。 以下三项测试保留在 **UpdateReadiness** 组中：

  - **AzSInfraFileValidation**
  - **AzSActionPlanStatus**
  - **AzsStampBMCSummary**

- 添加了审核规则来报告外部设备（例如 USB 密钥）装载到 Azure Stack Hub 基础结构节点的时间。 审核日志通过 syslog 发出，并显示为“Microsoft-Windows-Security-Auditing:  6416|即插即用事件”。 有关如何配置 syslog 客户端的详细信息，请参阅 [Syslog 转发](azure-stack-integrate-security.md)。

- Azure Stack Hub 的内部证书即将改用 4096 位 RSA 密钥。 运行内部机密轮换会将旧的 2048 位证书替换为 4096 位长的证书。 有关 Azure Stack Hub 中的机密轮换的详细信息，请参阅[在 Azure Stack Hub 中轮换机密](azure-stack-rotate-secrets.md)。

- 将多个内部组件升级到符合国家安全系统委员会 - 政策 15（CNSSP-15，该政策提供使用公共标准来安全共享信息的最佳做法）的密码编译算法复杂性和密钥强度。 其中的改进包括，在 Kerberos 身份验证中使用 AES256，以及在 VPN 加密中使用 SHA384。 有关 CNSSP-15 的详细信息，请参阅[国家安全系统委员会的“政策”页](http://www.cnss.gov/CNSS/issuances/Policies.cfm)。

- 由于上述升级，Azure Stack Hub 现在对 IPsec/IKEv2 配置使用新的默认值。 Azure Stack Hub 端使用的新默认值如下：

   **IKE 阶段 1（主模式）参数**

   | 属性              | Value|
   |-|-|
   | SDK 版本           | IKEv2 |
   |Diffie-Hellman 组   | ECP384 |
   | 身份验证方法 | 预共享密钥 |
   |加密和哈希算法 | AES256、SHA384 |
   |SA 生存期（时间）     | 28,800 秒|

   **IKE 阶段 2（快速模式）参数**

   | 属性| Value|
   |-|-|
   |SDK 版本 |IKEv2 |
   |加密和哈希算法（加密）     | GCMAES256|
   |加密和哈希算法（身份验证） | GCMAES256|
   |SA 生存期（时间）  | 27,000 秒  |
   |SA 生存期（千字节） | 33,553,408     |
   |完全向前保密 (PFS) | ECP384 |
   |死对等体检测 | 支持|

   [默认的 IPsec/IKE 提案](../user/azure-stack-vpn-gateway-settings.md#ipsecike-parameters)文档中也反映了这些更改。

- 基础结构备份服务改进了计算备份所需的可用空间的逻辑，而不是依赖固定的阈值。 该服务使用备份大小、保留策略、预留和外部存储位置的当前利用率，来确定是否需要对操作员引发警告。

### <a name="changes"></a>更改

- 将市场项从 Azure 下载到 Azure Stack Hub 时，可以使用新的用户界面来指定项的版本（如果存在多个版本时）。 新 UI 可用于联网场景和离线场景。 有关详细信息，请参阅[将市场项从 Azure 下载到 Azure Stack Hub](azure-stack-download-azure-marketplace-item.md)。  

- 从版本 1910 开始，Azure Stack Hub 系统**需要**额外的 /20 专用内部 IP 空间。 此网络是 Azure Stack Hub 系统的专用网络，并且可以在数据中心内的多个 Azure Stack Hub 系统上重复使用。 这是 Azure Stack Hub 的专用网络，不能与数据中心内的网络重叠。 /20 专用 IP 空间划分成多个网络，使你能够在容器上运行 Azure Stack Hub 基础结构（如以前的 [1905 发行说明](release-notes.md?view=azs-1905)中所述）。 在容器中运行 Azure Stack Hub 基础结构的目标是优化利用率并提升性能。 此外，/20 专用 IP 空间还用于实现正在进行的工作，以减少部署前所需的可路由 IP 空间。

  - 请注意，/20 输入是版本 1910 之后下一个 Azure Stack Hub 更新的先决条件。 发布版本 1910 之后的下一个 Azure Stack Hub 更新后，当你尝试安装该更新时，如果尚未完成以下补救步骤中所述的 /20 输入，则更新将会失败。 在完成上述补救步骤之前，管理员门户中会出现警报。 请参阅[数据中心网络集成](azure-stack-network.md#private-network)一文，了解如何使用此新专用空间。

  - 补救步骤：若要进行补救，请按照说明[打开 PEP 会话](azure-stack-privileged-endpoint.md#access-the-privileged-endpoint)。 准备一个大小为 /20 的[专用内部 IP 范围](azure-stack-network.md#logical-networks)，然后使用以下示例，在 PEP 会话中运行以下 cmdlet（仅适用于 1910 及更高版本）：`Set-AzsPrivateNetwork -UserSubnet 100.87.0.0/20`。 如果成功执行该操作，将会出现消息“Azs 内部网络范围已添加到配置”。  如果成功完成，管理员门户中的警报将会关闭。 Azure Stack Hub 系统现在可以更新到下一版本。
  
- 如果在上传过程中外部存储位置耗尽了容量，基础结构备份服务将会删除部分上传的备份数据。  

- 基础结构备份服务将标识服务添加到 AAD 部署的备份有效负载。  

- Azure Stack Hub PowerShell 模块已更新为适用于版本 1910 的 1.8.0 版。<br>更改包括：
   - **新的 DRP 管理模块**：使用部署资源提供程序 (DRP) 能够以协调的方式将资源提供程序部署到 Azure Stack Hub。 这些命令与 Azure 资源管理器层交互，从而与 DRP 交互。
   - **BRP**： <br />
           - 支持 Azure stack 基础结构备份的单个角色还原。 <br />
           - 将参数 `RoleName` 添加到 cmdlet `Restore-AzsBackup`。
   - **FRP**：“驱动器”和“卷”资源的中断性变更，提供 API 版本 `2019-05-01`。   Azure Stack Hub 1910 和更高版本支持的功能： <br />
            - `ID`、`Name`、`HealthStatus` 和 `OperationalStatus` 的值已更改。 <br />
            - 支持“驱动器”资源的新属性 `FirmwareVersion`、`IsIndicationEnabled`、`Manufacturer`和 `StoragePool`。  <br />
            - 已弃用“驱动器”资源的属性 `CanPool` 和 `CannotPoolReason`；改用 `OperationalStatus`。 

### <a name="fixes"></a>修复项

<!-- Product fixes that came up from customer deployments worth highlighting, especially if there's an SR/ICM associated to it. -->

- 修复了以下问题：在 Azure Stack Hub 1904 版本之前部署的环境中无法强制实施 TLS 1.2 策略。
- 修复了以下问题：创建时已启用 SSH 授权的 Ubuntu 18.04 VM 不允许使用 SSH 密钥登录。
- 从虚拟机规模集 UI 中删除了“重置密码”。 
- 修复了以下问题：从门户删除负载均衡器不会删除基础结构层中的对象。
- 修复了以下问题：管理员门户上的网关池用量警报显示不正确的百分比。
<!-- Fixed an issue where adding more than one public IP on the same NIC on a Virtual Machine resulted in internet connectivity issues. Now, a NIC with two public IPs should work as expected.[This fix actually didn't go in 1910 due to build issues, commenting out until next build (2002) ] -->

## <a name="security-updates"></a>安全更新

有关此 Azure Stack Hub 更新中的安全更新的信息，请参阅 [Azure Stack Hub 安全更新](release-notes-security-updates.md)。

## <a name="update-planning"></a>更新规划

应用更新之前，请务必查看以下信息：

- [已知问题](known-issues.md)
- [安全更新](release-notes-security-updates.md)
- [应用更新之前和之后的活动清单](release-notes-checklist.md)

## <a name="download-the-update"></a>下载更新

可从 [Azure Stack Hub 下载页](https://aka.ms/azurestackupdatedownload)下载 Azure Stack Hub 1910 更新包。

## <a name="hotfixes"></a>修补程序

Azure Stack Hub 定期发布修补程序。 将 Azure Stack Hub 更新到 1910 之前，请务必先安装 1908 的最新 Azure Stack Hub 修补程序。

> [!NOTE]
> Azure Stack Hub 修补程序版本是累积性的；你只需安装最新的修补程序即可获取该版本的任何以前修补程序版本中包含的所有修补程序。

Azure Stack Hub 修补程序仅适用于 Azure Stack Hub 集成系统；请勿尝试在 ASDK 上安装修补程序。

### <a name="prerequisites-before-applying-the-1910-update"></a>先决条件：应用 1910 更新之前

必须在包含以下修补程序的版本 1908 中应用 Azure Stack 版本 1910：

<!-- One of these. Either no updates at all, nothing is required, or the LATEST hotfix that is required-->
- [Azure Stack Hub 修补程序 1.1908.19.62](https://support.microsoft.com/help/4541349)

### <a name="after-successfully-applying-the-1910-update"></a>成功应用 1910 更新之后

安装此更新之后，请安装所有适用的修补程序。 有关详细信息，请参阅我们的[服务策略](azure-stack-servicing-policy.md)。

<!-- One of these. Either no updates at all, nothing is required, or the LATEST hotfix that is required-->
- [Azure Stack Hub 修补程序 1.1910.24.108](https://support.microsoft.com/help/4541350)
::: moniker-end

::: moniker range="azs-1908"
## <a name="1908-build-reference"></a>1908 内部版本参考

Azure Stack Hub 1908 更新内部版本号为 **1.1908.4.33**。

### <a name="update-type"></a>更新类型

对于 1908，运行 Azure Stack Hub 的底层操作系统已更新为 Windows Server 2019。 此更新可以实现核心基础增强，并将更多功能引入 Azure Stack Hub。

Azure Stack Hub 1908 更新内部版本类型为“完整”。  因此，1908 更新的运行时间比快速更新（例如 1906 和 1907）更久。 完整更新的确切运行时间通常取决于 Azure Stack Hub 实例包含的节点数目、租户工作负荷在系统上使用的容量、系统的网络连接（如果已连接到 Internet），以及系统的硬件配置。 在我们的内部测试中，1908 更新的预期运行时间如下：4 个节点 - 42 小时，8 个节点 - 50 小时，12 个节点 - 60 小时，16 个节点 - 70 小时。 更新运行时间超过这些预期值并不常见，因此，除了更新失败之外，无需要求 Azure Stack Hub 操作员执行操作。

有关更新内部版本类型的详细信息，请参阅[在 Azure Stack Hub 中管理更新](azure-stack-updates.md)。

- 确切的更新运行时间通常取决于租户工作负荷在系统上使用的容量、系统网络连接（如果已连接到 Internet），以及系统的硬件配置。
- 运行时间超过预期值并不常见，除非更新失败，否则无需 Azure Stack Hub 操作员采取措施。
- 此运行时近似值特定于 1908 更新，不应与其他 Azure Stack Hub 更新进行比较。

<!-- ## What's in this update -->

<!-- The current theme (if any) of this release. -->

### <a name="whats-new"></a>新增功能

<!-- What's new, also net new experiences and features. -->

- 对于 1908，请注意，运行 Azure Stack Hub 的底层操作系统已更新为 Windows Server 2019。 此更新可以实现核心基础增强，并将更多功能引入 Azure Stack Hub。
- Azure Stack Hub 基础结构的所有组件现在都以 FIPS 140-2 模式运行。
- Azure Stack Hub 操作员现在可以删除门户用户数据。 有关详细信息，请参阅[从 Azure Stack Hub 中清除门户用户数据](azure-stack-portal-clear.md)。

### <a name="improvements"></a>改进

<!-- Changes and product improvements with tangible customer-facing value. -->
- Azure Stack Hub 的静态数据加密已得到改进，可将机密持久保存到物理节点的硬件受信任平台模块 (TPM)。

### <a name="changes"></a>更改

- 硬件提供商将在 Azure Stack Hub 版本 1908 的同一发布时间发布 OEM 扩展包 2.1 或更高版本。 必须安装 OEM 扩展包 2.1 或更高版本才能使用 Azure Stack Hub 版本 1908。 有关如何下载 OEM 扩展包 2.1 或更高版本的详细信息，请与系统的硬件提供商联系，并参阅 [OEM 更新](azure-stack-update-oem.md#oem-contact-information)一文。  

### <a name="fixes"></a>修复项

- 修复了与将来的 Azure Stack Hub OEM 更新兼容的问题，以及使用客户用户映像进行 VM 部署的问题。 此问题是在 1907 中发现的，已在修补程序 [KB4517473](https://support.microsoft.com/en-us/help/4517473/azure-stack-hotfix-1-1907-12-44) 中予以修复  
- 修复了 OEM 固件更新的问题，并更正了 Fabric Ring Health 的 Test-AzureStack 中的诊断错误。 此问题是在 1907 中发现的，已在修补程序 [KB4515310](https://support.microsoft.com/en-us/help/4515310/azure-stack-hotfix-1-1907-7-35) 中予以修复
- 修复了 OEM 固件更新过程的问题。 此问题是在 1907 中发现的，已在修补程序 [KB4515650](https://support.microsoft.com/en-us/help/4515650/azure-stack-hotfix-1-1907-8-37) 中予以修复

<!-- Product fixes that came up from customer deployments worth highlighting, especially if there's an SR/ICM associated to it. -->

## <a name="security-updates"></a>安全更新

有关此 Azure Stack Hub 更新中的安全更新的信息，请参阅 [Azure Stack Hub 安全更新](release-notes-security-updates.md)。

## <a name="download-the-update"></a><a name="download-the-update-1908"></a>下载更新

可从 [Azure Stack Hub 下载页](https://aka.ms/azurestackupdatedownload)下载 Azure Stack Hub 1908 更新包。

## <a name="hotfixes"></a>修补程序

Azure Stack Hub 定期发布修补程序。 将 Azure Stack Hub 更新到 1908 之前，请务必先安装 1907 的最新 Azure Stack Hub 修补程序。

Azure Stack Hub 修补程序仅适用于 Azure Stack Hub 集成系统；请勿尝试在 ASDK 上安装修补程序。

### <a name="prerequisites-before-applying-the-1908-update"></a>先决条件：应用 1908 更新之前

必须在包含以下修补程序的版本 1907 中应用 Azure Stack Hub 版本 1908：

<!-- One of these. Either no updates at all, nothing is required, or the LATEST hotfix that is required-->
- [Azure Stack Hub 修补程序 1.1907.26.70](https://support.microsoft.com/help/4541348)

Azure Stack Hub 1908 更新需要系统硬件提供商提供的 **Azure Stack Hub OEM 2.1 或更高版本**。 OEM 更新包括 Azure Stack Hub 系统硬件的驱动程序和固件更新。 有关应用 OEM 更新的详细信息，请参阅[应用 Azure Stack Hub 原始设备制造商更新](azure-stack-update-oem.md)

### <a name="after-successfully-applying-the-1908-update"></a>成功应用 1908 更新之后

安装此更新之后，请安装所有适用的修补程序。 有关详细信息，请参阅我们的[服务策略](azure-stack-servicing-policy.md)。

<!-- One of these. Either no updates at all, nothing is required, or the LATEST hotfix that is required-->
- [Azure Stack Hub 修补程序 1.1908.19.62](https://support.microsoft.com/help/4541349)
::: moniker-end

::: moniker range="azs-1907"
## <a name="1907-build-reference"></a>1907 内部版本参考

Azure Stack Hub 1907 更新内部版本号为 **1.1907.0.20**。

### <a name="update-type"></a>更新类型

Azure Stack Hub 1907 更新内部版本类型为“快速”。  有关更新内部版本类型的详细信息，请参阅[管理 Azure Stack Hub 中的更新](azure-stack-updates.md)一文。 根据内部测试，完成 1907 更新所需的预期时间约为 13 个小时。

- 确切的更新运行时间通常取决于租户工作负荷在系统上使用的容量、系统网络连接（如果已连接到 Internet），以及系统的硬件配置。
- 运行时间超过预期值并不常见，除非更新失败，否则无需 Azure Stack Hub 操作员采取措施。
- 此运行时近似值特定于 1907 更新，不应与其他 Azure Stack Hub 更新进行比较。

## <a name="whats-in-this-update"></a>此更新的内容

<!-- The current theme (if any) of this release. -->

### <a name="whats-new"></a>新增功能

<!-- What's new, also net new experiences and features. -->

- 正式推出 Azure Stack Hub 诊断日志收集服务，以加速和改善诊断日志的收集。 Azure Stack Hub 诊断日志收集服务可让你轻松地通过 Microsoft 客户支持服务 (CSS) 收集和共享诊断日志。 此诊断日志收集服务可在 Azure Stack Hub 管理员门户中提供新的用户体验，让操作员设置在引发特定的关键警报时自动将诊断日志上传到存储 Blob。 该服务还可用于按需执行相同的操作。 有关详细信息，请参阅[诊断日志收集](azure-stack-diagnostic-log-collection-overview.md)一文。

- 正式推出 Azure Stack Hub 网络基础结构验证作为 Azure Stack Hub 验证工具 **Test-AzureStack** 的一部分。 Azure Stack Hub 网络基础结构将成为 **Test-AzureStack** 的一部分，可识别 Azure Stack Hub 的网络基础结构是否发生故障。 此测试绕过 Azure Stack Hub 软件定义的网络来检查网络基础结构的连接。 它会演示如何从公共 VIP 连接到配置的 DNS 转发器、NTP 服务器和标识终结点。 此外，在使用 Azure AD 作为标识提供者时，它会检查与 Azure 的连接；使用 ADFS 时，它会检查与联合服务器的连接。 有关详细信息，请参阅 [Azure Stack Hub 验证工具](azure-stack-diagnostic-test.md)一文。

- 添加了内部机密轮换过程，以便在系统更新期间轮换内部 SQL TLS 证书。

### <a name="improvements"></a>改进

<!-- Changes and product improvements with tangible customer-facing value. -->

- “Azure Stack Hub 更新”边栏选项卡现在会显示活动更新的“最后一个步骤已完成”时间。  转到“更新”边栏选项卡，然后单击某个正在运行的更新，即可查看此添加内容。 “最后一个步骤已完成”随后将显示在“更新运行详细信息”部分。  

- 改进了 **Start-AzureStack** 和 **Stop-AzureStack** 操作员操作。 Azure Stack Hub 的启动时间平均已减少 50%。 Azure Stack Hub 的关闭时间平均已减少 30%。 当缩放单元中的节点数目增加时，平均启动和关闭时间保持不变。

- 改进了已断开连接的市场工具的错误处理方式。 如果在使用 **Export-AzSOfflineMarketplaceItem** 时下载失败或部分成功，系统将显示详细的错误消息，其中包含有关错误的更详细信息和任何可行的缓解步骤。

- 改进了从大型页 Blob/快照创建托管磁盘的性能。 以前在创建大型磁盘时会触发超时。  

<!-- https://icm.ad.msft.net/imp/v3/incidents/details/127669774/home -->
- 改进了关闭节点之前的虚拟磁盘运行状况检查，以避免发生意外的虚拟磁盘分离。

- 改进了管理员操作内部日志的存储方式。 此新增功能可以尽量减少内部日志进程使用的内存和存储，从而改进管理员操作期间的性能和可靠性。 你还可能会注意到，管理员门户中的更新边栏选项卡页加载时间有所改善。 作为此项改进的一部分，系统将不再提供超过 6 个月的更新日志。 如果需要这些更新的日志，请务必先针对超过 6 个月的所有已运行更新[下载摘要](azure-stack-apply-updates.md)，然后执行 1907 更新。

### <a name="changes"></a>更改

- Azure Stack Hub 版本 1907 包含警告警报，指示操作员在更新到版本 1908 之前，先将其系统的 OEM 包更新为版本 2.1 或更高版本。 有关如何应用 Azure Stack Hub OEM 更新的详细信息，请参阅[应用 Azure Stack Hub 原始设备制造商更新](azure-stack-update-oem.md)。

- 已添加新的出站规则 (HTTPS) 来启用 Azure Stack Hub 诊断日志收集服务的通信。 有关详细信息，请参阅 [Azure Stack Hub 数据中心集成 - 发布终结点](azure-stack-integrate-endpoints.md#ports-and-urls-outbound)。

- 现在，如果外部存储位置耗尽了容量，基础结构备份服务将会删除部分上传的备份。

- 基础结构备份不再包含域服务数据的备份。 此项更改仅适用于使用 Azure Active Directory 作为标识提供者的系统。

- 我们现在会验证引入到“计算”->“VM 映像”边栏选项卡中的映像是否为页 Blob 类型。 

- 特权终结点命令 **Set-BmcCredential** 现在会更新基板管理控制器中的凭据。

### <a name="fixes"></a>修复项

<!-- Product fixes that came up from customer deployments worth highlighting, especially if there's an SR/ICM associated to it. -->
- 修复了在资源管理器模板中将发布者、套餐和 SKU 视为区分大小写的问题：除非映像参数的大小写与发布者、套餐和 SKU 的大小写相同，否则不会提取该映像进行部署。

<!-- https://icm.ad.msft.net/imp/v3/incidents/details/129536438/home -->
- 修复了因存储服务元数据备份期间超时而导致备份失败并显示 **PartialSucceeded** 错误消息的问题。  

- 修复了删除用户订阅导致孤立资源的问题。

- 修复了在创建套餐时不保存说明字段的问题。

- 修复了具有“只读”权限的用户能够创建、编辑和删除资源的问题。  现在，只有在获得“参与者”权限之后，用户才能创建资源。  

<!-- https://icm.ad.msft.net/imp/v3/incidents/details/127772311/home -->
- 修复了由于 WMI 提供程序主机锁定 DLL 文件而导致更新失败的问题。

- 修复了更新服务中使得可用更新无法显示在更新磁贴或资源提供程序中的问题。 此问题是在 1906 中发现的，已在修补程序 [KB4511282](https://support.microsoft.com/help/4511282/) 中予以修复。

- 修复了由于配置不当而导致管理平面变得不正常，从而导致更新失败的问题。 此问题是在 1906 中发现的，已在修补程序 [KB4512794](https://support.microsoft.com/help/4512794/) 中予以修复。

- 修复了导致用户无法从市场完成第三方映像部署的问题。 此问题是在 1906 中发现的，已在修补程序 [KB4511259](https://support.microsoft.com/help/4511259/) 中予以修复。

- 修复了用户映像管理器服务崩溃可能导致无法从托管映像创建 VM 的问题。 此问题是在 1906 中发现的，已在修补程序 [KB4512794](https://support.microsoft.com/help/4512794/) 中予以修复

- 修复了由于应用程序网关缓存未按预期刷新而导致 VM CRUD 操作失败的问题。 此问题是在 1906 中发现的，已在修补程序 [KB4513119](https://support.microsoft.com/en-us/help/4513119/) 中予以修复

- 修复了运行状况资源提供程序中的一个问题，该问题会影响管理员门户中区域和警报边栏选项卡的可用性。 此问题是在 1906 中发现的，已在修补程序 [KB4512794](https://support.microsoft.com/help/4512794) 中予以修复。

## <a name="security-updates"></a>安全更新

有关此 Azure Stack Hub 更新中的安全更新的信息，请参阅 [Azure Stack Hub 安全更新](release-notes-security-updates.md)。

## <a name="update-planning"></a>更新规划

应用更新之前，请务必查看以下信息：

- [已知问题](known-issues.md)
- [安全更新](release-notes-security-updates.md)
- [应用更新之前和之后的活动清单](release-notes-checklist.md)

## <a name="download-the-update"></a>下载更新

可从 [Azure Stack Hub 下载页](https://aka.ms/azurestackupdatedownload)下载 Azure Stack Hub 1907 更新包。

## <a name="hotfixes"></a>修补程序

Azure Stack Hub 定期发布修补程序。 将 Azure Stack Hub 更新到 1907 之前，请务必先安装 1906 的最新 Azure Stack Hub 修补程序。

Azure Stack Hub 修补程序仅适用于 Azure Stack Hub 集成系统；请勿尝试在 ASDK 上安装修补程序。

### <a name="before-applying-the-1907-update"></a>应用 1907 更新之前

必须在包含以下修补程序的版本 1906 中应用 Azure Stack Hub 版本 1907：

<!-- One of these. Either no updates at all, nothing is required, or the LATEST hotfix that is required-->
- [Azure Stack Hub 修补程序 1.1906.15.60](https://support.microsoft.com/help/4524559)

### <a name="after-successfully-applying-the-1907-update"></a>成功应用 1907 更新之后

安装此更新之后，请安装所有适用的修补程序。 有关详细信息，请参阅我们的[服务策略](azure-stack-servicing-policy.md)。

<!-- One of these. Either no updates at all, nothing is required, or the LATEST hotfix that is required-->
- [Azure Stack Hub 修补程序 1.1907.26.70](https://support.microsoft.com/help/4541348)
::: moniker-end

::: moniker range="azs-1906"
## <a name="1906-build-reference"></a>1906 内部版本参考

Azure Stack Hub 1906 更新内部版本号为 **1.1906.0.30**。

### <a name="update-type"></a>更新类型

Azure Stack Hub 1906 更新内部版本类型为“快速”  。 有关更新内部版本类型的详细信息，请参阅[管理 Azure Stack Hub 中的更新](azure-stack-updates.md)一文。 无论 Azure Stack Hub 环境中有多少个物理节点，完成 1906 更新的预估时间都大约为 10 小时。 确切的更新运行时间通常取决于租户工作负荷在系统上使用的容量、系统网络连接（如果已连接到 Internet），以及系统的硬件规格。 运行时间超过预期值并不常见，除非更新失败，否则无需 Azure Stack Hub 操作员采取措施。 此运行时近似值特定于 1906 更新，不应与其他 Azure Stack Hub 更新进行比较。

## <a name="whats-in-this-update"></a>此更新的内容

<!-- The current theme (if any) of this release. -->

<!-- What's new, also net new experiences and features. -->

- 在特权终结点 (PEP) 中添加 **Set-TLSPolicy** cmdlet，以在所有终结点上强制实施 TLS 1.2。 有关详细信息，请参阅 [Azure Stack Hub 安全控制](azure-stack-security-configuration.md)。

- 在特权终结点 (PEP) 中添加 **Get-TLSPolicy** cmdlet，以检索应用的 TLS 策略。 有关详细信息，请参阅 [Azure Stack Hub 安全控制](azure-stack-security-configuration.md)。

- 添加了内部机密轮换过程，以便在系统更新期间轮换内部 TLS 证书。

- 添加了一项保护措施，以便在机密过期的重大警报遭到忽略时，通过强制实施内部机密轮换来防止内部机密过期。 在日常运营过程中不应依赖此保护措施。 应在维护时段规划机密轮换。 有关详细信息，请参阅 [Azure Stack Hub 机密轮换](azure-stack-rotate-secrets.md)。

- 使用 AD FS 的 Azure Stack Hub 部署现在支持 Visual Studio Code。

### <a name="improvements"></a>改进

<!-- Changes and product improvements with tangible customer-facing value. -->

- 特权终结点中的 **Get-GraphApplication** cmdlet 现在显示当前使用的证书的指纹。 使用 AD FS 部署 Azure Stack Hub 时，此项更新可以改善服务主体的证书管理。

- 添加了运行状况监视规则来验证 AD Graph 和 AD FS 的可用性，包括引发警报的功能。

- 改善了基础结构备份服务移到另一个实例时的备份资源提供程序可靠性。

- 优化了外部机密轮换过程的性能，提供了统一的执行时间以简化维护时段的计划。

- **Test-AzureStack** cmdlet 现在会报告即将过期的内部机密（关键警报）。

- 特权终结点中的 **Register-CustomAdfs** cmdlet 有新参数可用，可让你在设置 AD FS 联合信任时跳过证书吊销列表的检查。

- 版本 1906 引入了更高的更新进度可见性，让你确保更新不会暂停。 此项更新使操作员可在“更新”边栏选项卡中看到更多的更新步骤总数。  你还可能会发现，与以前的更新相比，现在有更多的更新步骤同时进行。

#### <a name="networking-updates"></a>网络更新

- 将 DHCP 响应程序中设置的租约时间更新为与 Azure 一致。

- 改善了在发生资源部署失败的情况时资源提供程序的重试率。

- 从负载均衡器和公共 IP 中删除了**标准** SKU 选项，因为目前不支持该选项。

### <a name="changes"></a>更改

- 创建存储帐户的体验现在与 Azure 一致。

- 更改了内部机密过期的警报触发器：
  - 警告警报现在会在机密过期之前的 90 天引发。
  - 关键警报现在会在机密过期之前的 30 天引发。

- 更新了基础结构备份资源提供程序中的字符串以使用一致的术语。

### <a name="fixes"></a>修复项

<!-- Product fixes that came up from customer deployments worth highlighting, especially if there's an SR/ICM associated to it. -->

- 修复了托管磁盘的 VM 大小调整因“内部操作错误”而失败的问题。 

- 修复了以下问题：失败的用户映像创建使管理映像的服务处于不正常状态；这会导致无法删除失败的映像，并且无法创建新映像。 此问题也已在 1905 修补程序中得到修复。

- 现在，内部机密即将过期的活动警报在内部机密轮换成功执行之后会自动关闭。

- 修复了以下问题：如果更新运行超过 99 个小时，更新历史记录选项卡中的更新持续时间会去掉第一位数。

- “更新”边栏选项卡包含针对失败更新的“继续”选项。  

- 在管理员和用户门户中修复了以下市场问题：Docker 扩展未正确从搜索中返回，并且无法采取进一步的措施，因为 Azure Stack Hub 不提供此类措施。

- 修复了模板部署 UI 中的以下问题：如果模板名称以下划线束“_”开头，则不会填充参数。

- 修复了虚拟机规模集创建体验提供基于 CentOS 的 7.2 作为部署选项的问题。 Azure Stack Hub 不提供 CentOS 7.2。 我们现在提供 Centos 7.5 作为部署选项

- 现在可以从“虚拟机规模集”  边栏选项卡中删除规模集。

## <a name="security-updates"></a>安全更新

有关此 Azure Stack Hub 更新中的安全更新的信息，请参阅 [Azure Stack Hub 安全更新](release-notes-security-updates.md)。

## <a name="update-planning"></a>更新规划

应用更新之前，请务必查看以下信息：

- [已知问题](known-issues.md)
- [安全更新](release-notes-security-updates.md)
- [应用更新之前和之后的活动清单](release-notes-checklist.md)

## <a name="download-the-update"></a>下载更新

可从 [Azure Stack Hub 下载页](https://aka.ms/azurestackupdatedownload)下载 Azure Stack Hub 1906 更新包。

## <a name="hotfixes"></a>修补程序

Azure Stack Hub 定期发布修补程序。 将 Azure Stack Hub 更新到 1906 之前，请务必先安装 1905 的最新 Azure Stack Hub 修补程序。 更新后，安装[适用于 1906 的任何修补程序](#after-successfully-applying-the-1906-update)。

Azure Stack Hub 修补程序仅适用于 Azure Stack Hub 集成系统；请勿尝试在 ASDK 上安装修补程序。

### <a name="before-applying-the-1906-update"></a>应用 1906 更新之前

必须在包含以下修补程序的版本 1905 中应用 Azure Stack Hub 版本 1906：

<!-- One of these. Either no updates at all, nothing is required, or the LATEST hotfix that is required-->
- [Azure Stack Hub 修补程序 1.1905.3.48](https://support.microsoft.com/help/4510078)

### <a name="after-successfully-applying-the-1906-update"></a>成功应用 1906 更新之后

安装此更新之后，请安装所有适用的修补程序。 有关详细信息，请参阅我们的[服务策略](azure-stack-servicing-policy.md)。

<!-- One of these. Either no updates at all, nothing is required, or the LATEST hotfix that is required-->
- [Azure Stack Hub 修补程序 1.1906.15.60](https://support.microsoft.com/help/4524559)
::: moniker-end

::: moniker range=">=azs-1906"
## <a name="automatic-update-notifications"></a>自动更新通知

其系统可从基础结构网络访问 Internet 的客户在操作员门户中会看到“有可用的更新”消息。  无法访问 Internet 的系统可以下载并导入包含相应 .xml 的 .zip 文件。

> [!TIP]  
> 订阅下述 *RSS* 或 *Atom* 源，了解 Azure Stack Hub 修补程序的最新信息：
>
> - [RSS](https://support.microsoft.com/app/content/api/content/feeds/sap/en-us/32d322a8-acae-202d-e9a9-7371dccf381b/rss)
> - [Atom](https://support.microsoft.com/app/content/api/content/feeds/sap/en-us/32d322a8-acae-202d-e9a9-7371dccf381b/atom)

## <a name="archive"></a>Archive

若要访问旧版的已存档发行说明，请使用左侧目录上方的版本选择器下拉列表选择要查看的版本。

## <a name="next-steps"></a>后续步骤

- 有关 Azure Stack Hub 中更新管理的概述，请参阅[在 Azure Stack Hub 中管理更新的概述](azure-stack-updates.md)。  
- 有关如何在 Azure Stack Hub 中应用更新的详细信息，请参阅[在 Azure Stack Hub 中应用更新](azure-stack-apply-updates.md)。
- 若要查看 Azure Stack Hub 的服务策略，以及必须如何做才能使系统保持在受支持的状态，请参阅 [Azure Stack Hub 服务策略](azure-stack-servicing-policy.md)。  
- 若要使用特权终结点 (PEP) 来监视和恢复更新，请参阅[使用特权终结点监视 Azure Stack Hub 中的更新](azure-stack-monitor-update.md)。
::: moniker-end

<!------------------------------------------------------------>
<!------------------- UNSUPPORTED VERSIONS ------------------->
<!------------------------------------------------------------>
::: moniker range="azs-1905"
## <a name="1905-archived-release-notes"></a>1905 已存档的发行说明
::: moniker-end
::: moniker range="azs-1904"
## <a name="1904-archived-release-notes"></a>1904 已存档的发行说明
::: moniker-end
::: moniker range="azs-1903"
## <a name="1903-archived-release-notes"></a>1903 已存档的发行说明
::: moniker-end
::: moniker range="azs-1902"
## <a name="1902-archived-release-notes"></a>1902 已存档的发行说明
::: moniker-end
::: moniker range="azs-1901"
## <a name="1901-archived-release-notes"></a>1901 已存档的发行说明
::: moniker-end
::: moniker range="azs-1811"
## <a name="1811-archived-release-notes"></a>1811 已存档的发行说明
::: moniker-end
::: moniker range="azs-1809"
## <a name="1809-archived-release-notes"></a>1809 已存档的发行说明
::: moniker-end
::: moniker range="azs-1808"
## <a name="1808-archived-release-notes"></a>1808 已存档的发行说明
::: moniker-end
::: moniker range="azs-1807"
## <a name="1807-archived-release-notes"></a>1807 已存档的发行说明
::: moniker-end
::: moniker range="azs-1805"
## <a name="1805-archived-release-notes"></a>1805 已存档的发行说明
::: moniker-end
::: moniker range="azs-1804"
## <a name="1804-archived-release-notes"></a>1804 已存档的发行说明
::: moniker-end
::: moniker range="azs-1803"
## <a name="1803-archived-release-notes"></a>1803 已存档的发行说明
::: moniker-end
::: moniker range="azs-1802"
## <a name="1802-archived-release-notes"></a>1802 已存档的发行说明
::: moniker-end

::: moniker range="<azs-1906"
可以访问 [TechNet 库中旧版本 Azure Stack Hub 的发行说明](https://aka.ms/azsarchivedrelnotes)。 提供这些已存档文档仅供参考，并不意味着支持这些版本。 有关 Azure Stack Hub 支持的信息，请参阅 [Azure Stack Hub 服务策略](azure-stack-servicing-policy.md)。 如需进一步的帮助，请联系 Microsoft 客户支持服务。
::: moniker-end


