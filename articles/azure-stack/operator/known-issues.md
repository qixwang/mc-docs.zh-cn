---
title: Azure Stack Hub 已知问题
description: 了解 Azure Stack Hub 发行版中的已知问题。
author: WenJason
ms.topic: article
origin.date: 03/12/2020
ms.date: 03/23/2020
ms.author: v-jay
ms.reviewer: prchint
ms.lastreviewed: 11/21/2019
ms.openlocfilehash: a0e4ea04dcc73a3b9465090f22bb4bc4c9959b51
ms.sourcegitcommit: c1ba5a62f30ac0a3acb337fb77431de6493e6096
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2020
ms.locfileid: "79547075"
---
# <a name="azure-stack-hub-known-issues"></a>Azure Stack Hub 已知问题

本文列出了 Azure Stack Hub 发行版中的已知问题。 每当发现新的问题，此列表就会更新。

若要访问不同版本的已知问题，请使用左侧目录上方的版本选择器下拉列表。

::: moniker range=">=azs-1906"
> [!IMPORTANT]  
> 在应用更新之前，请先查看本部分。
::: moniker-end
::: moniker range="<azs-1906"
> [!IMPORTANT]  
> 如果 Azure Stack Hub 实例落后于两个以上的更新，则认为它不符合。 必须[至少更新到最低支持版本才能获得支持](azure-stack-servicing-policy.md#keep-your-system-under-support)。 
::: moniker-end

<!---------------------------------------------------------->
<!------------------- SUPPORTED VERSIONS ------------------->
<!---------------------------------------------------------->

::: moniker range="azs-1910"
## <a name="update"></a>更新

有关已知的 Azure Stack Hub 更新问题，请参阅[排查 Azure Stack Hub 中的更新问题](azure-stack-updates-troubleshoot.md)。

## <a name="portal"></a>门户

### <a name="administrative-subscriptions"></a>管理订阅

- 适用于：此问题适用于所有支持的版本。
- 原因：不应使用版本 1804 中引入的两个管理订阅。 这两种订阅类型为“计量订阅”和“消耗订阅”。  
- 补救措施：如果有资源在这两个订阅上运行，请在用户订阅中重新创建这些资源。
- 发生次数：通用

### <a name="subscriptions-lock-blade"></a>“订阅锁定”边栏选项卡

- 适用于：此问题适用于所有支持的版本。
- 原因：在管理员门户中，用户订阅的“锁定”  边栏选项卡有两个按钮，显示“订阅”  。
- 发生次数：通用

### <a name="subscription-permissions"></a>订阅权限

- 适用于：此问题适用于所有支持的版本。
- 原因：无法使用 Azure Stack Hub 门户查看订阅的权限。
- 补救措施：使用 [PowerShell 验证权限](https://docs.microsoft.com/powershell/module/azurerm.resources/get-azurermroleassignment)。
- 发生次数：通用

### <a name="storage-account-settings"></a>存储帐户设置

- 适用于：此问题适用于所有支持的版本。
- 原因：在用户门户中，存储帐户的“配置”边栏选项卡显示用于更改**安全传输类型**的选项。  Azure Stack Hub 目前不支持此功能。
- 发生次数：通用

### <a name="upload-blob-with-oauth-error"></a>使用 OAuth 上传 Blob 时出错

- 适用于：此问题适用于所有支持的版本。
- 原因：在用户门户中尝试使用“OAuth(preview)”选项上传 Blob 时，任务将会失败并出现错误消息。 
- 补救措施：使用 SAS 选项上传 Blob。
- 发生次数：通用

### <a name="upload-blob-option-unsupported"></a>不支持上传 Blob 选项

- 适用于：此问题适用于所有支持的版本。
- 原因：在用户门户中尝试从“上传”边栏选项卡上传 Blob 时，有一个选项可用于选择“AAD”或“密钥身份验证”，但是，Azure Stack Hub 不支持“AAD”。   
- 发生次数：通用

### <a name="load-balancer-backend-pool"></a>负载均衡器后端池

- 适用于：此问题适用于所有支持的版本。
- 原因：在用户门户中添加**负载均衡器**后端池时，此操作会导致出现错误消息“无法保存负载均衡器后端池”；但是，此操作实际上已成功。 
- 发生次数：通用

### <a name="alert-for-network-interface-disconnected"></a>网络接口已断开连接的警报

- 适用于：此问题适用于 1908 和 1910 版本。
- 原因：当线缆与网络适配器断开连接时，管理员门户中不显示警报。 此问题的原因是 Windows Server 2019 中默认会禁用此故障。
- 发生次数：通用

### <a name="incorrect-tooltip-when-creating-vm"></a>创建 VM 时出现不正确的工具提示

- 适用于：此问题适用于所有支持的版本。
- 原因：在用户门户中选择类型为“高级 SSD”的托管磁盘时，下拉列表显示“OS 磁盘”。  该选项旁边的工具提示显示“可以通过 Azure 试用帐户免费使用某些 OS 磁盘大小”；但是，这对于 Azure Stack Hub 而言是不正确的。  此外，该列表包含“符合免费帐户的条件”，而这对 Azure Stack Hub 而言也是不正确的。 
- 发生次数：通用

### <a name="vpn-troubleshoot-and-metrics"></a>VPN 故障排除和指标

- 适用于：此问题适用于所有支持的版本。
- 原因：用户门户显示 VPN 网关资源中的“VPN 故障排除”功能和“指标”，但这在 Azure Stack Hub 中不受支持。  
- 发生次数：通用

### <a name="adding-extension-to-vm-scale-set"></a>将扩展添加到 VM 规模集

- 适用于：此问题适用于 1907 和更高版本。
- 原因：在用户门户中创建虚拟机规模集之后，UI 就不允许用户添加扩展。
- 发生次数：通用

### <a name="delete-a-storage-container"></a>删除存储容器

- 适用于：此问题适用于所有支持的版本。
- 原因：在用户门户中，当用户尝试删除存储容器时，如果用户未切换到“替代 Azure Policy 和 RBAC 角色设置”，则该操作将会失败。 
- 补救措施：确保已选中“替代 Azure Policy 和 RBAC 角色设置”框。 
- 发生次数：通用

### <a name="refresh-button-on-virtual-machines-fails"></a>虚拟机上的刷新按钮失效

- 适用于：此问题适用于所有支持的版本。
- 原因：在用户门户中导航到“虚拟机”并尝试使用顶部的按钮进行刷新时，状态无法准确更新。 
- 补救措施：无论是否已按下刷新按钮，状态每隔 5 分钟都会自动更新。 请等待 5 分钟，然后检查状态。
- 发生次数：通用

### <a name="virtual-network-gateway"></a>虚拟网络网关

- 适用于：此问题适用于所有支持的版本。
- 原因：在用户门户中创建路由表时，“虚拟网络网关”显示为下一跃点类型选项；但是，Azure Stack Hub 不支持此类型。 
- 发生次数：通用

### <a name="storage-account-options"></a>存储帐户选项

- 适用于：此问题适用于所有支持的版本。
- 原因：在用户门户中，存储帐户名称显示为“存储帐户 - Blob、文件、表、队列”；但是，Azure Stack Hub 不支持“文件”。  
- 发生次数：通用

### <a name="storage-account-configuration"></a>存储帐户配置

- 适用于：此问题适用于所有支持的版本。
- 原因：在用户门户中创建存储帐户并查看其“配置”时，无法保存配置更改，因为这会导致 AJAX 错误。 
- 发生次数：通用

### <a name="capacity-monitoring-in-sql-resource-provider-keeps-loading"></a>SQL 资源提供程序中的容量监视保持正在加载状态

- 适用于：此问题适用于已安装 SQL 资源提供程序 1.1.33.0 或更低版本的 Azure Stack Hub 1910 更新或更高版本。
- 原因：当前的 SQL 资源提供程序版本与 1910 更新中最新的一些门户更改不兼容。
- 补救措施：将 Azure Stack Hub 升级到 1910 更新后，请按照资源提供程序更新过程应用 SQL 资源提供程序修补程序 1.1.47.0（[SQL RP 版本 1.1.47.0](https://aka.ms/azurestacksqlrp11470)）。 对于 MySQL 资源提供程序，我们还建议在将 Azure Stack Hub 升级到 1910 更新后，应用 MySQL 资源提供程序修补程序 1.1.47.0（[MySQL RP 版本 1.1.47.0](https://aka.ms/azurestackmysqlrp11470)）。
- 发生次数：通用

### <a name="access-control-iam"></a>访问控制 (IAM)

- 适用于：此问题适用于所有支持的版本。
- 原因：IAM 扩展已过时。 Azure Stack Hub 随附的 Ibiza 门户引入了新的行为，如果用户针对未在全局订阅选择器（用户门户中的“目录 + 订阅”）中选择的订阅打开“访问控制(IAM)”边栏选项卡，则会导致 RBAC 扩展失败。   该边栏选项卡反复显示“正在加载”，且用户无法将新角色添加到订阅。  “添加”边栏选项卡也反复显示“正在加载”。  
- 补救措施：确保已在“目录 + 订阅”菜单中选中该订阅。  可以从门户顶部、“通知”按钮附近，或通过显示以下内容的“所有资源”边栏选项卡上的快捷方式来访问此菜单：“看不到订阅?    请打开‘目录 + 订阅’设置”。 必须在此菜单中选择该订阅。

### <a name="sql-resource-provider"></a>SQL 资源提供程序

- 适用于：此问题适用于运行 1908 或更低版本的阵列。
- 原因：部署 SQL 资源提供程序 (RP) 版本 1.1.47.0 时，门户不显示与 SQL RP 关联的任何资产。
- 补救措施：删除 RP、升级阵列，然后重新部署 SQL RP。

### <a name="activity-log-blade"></a>“活动日志”边栏选项卡

- 适用于：此问题适用于运行 1907 或更高版本的阵列。 <!-- Note: Applies to 2002 as well -->
- 原因：访问活动日志时，门户只显示条目的第一页。 **加载更多结果**将不会加载附加条目。
- 补救措施：调整筛选器中的时间范围，以查看第一页之后的条目。

## <a name="networking"></a>网络

### <a name="load-balancer"></a>负载均衡器

- 适用于：此问题适用于所有支持的版本。
- 原因：将可用性集 VM 添加到负载均衡器的后端池时，门户上显示错误消息，指出“无法保存负载均衡器后端池”。  这是门户的表面问题；此功能仍在起作用，VM 在内部已成功添加到后端池。
- 发生次数：通用

### <a name="network-security-groups"></a>网络安全组

- 适用于：此问题适用于所有支持的版本。 
- 原因：无法在 NSG 中创建显式 **DenyAllOutbound** 规则，因为这会使 VM 部署所需的基础结构无法完成所有内部通信。
- 发生次数：通用

### <a name="service-endpoints"></a>服务终结点

- 适用于：此问题适用于所有支持的版本。
- 原因：在用户门户中，“虚拟网络”边栏选项卡显示使用“服务终结点”的选项。   Azure Stack Hub 目前不支持此功能。
- 发生次数：通用

### <a name="network-interface"></a>Linux

#### <a name="addingremoving-network-interface"></a>添加/删除网络接口

- 适用于：此问题适用于所有支持的版本。
- 原因：新网络接口无法添加到处于“正在运行”状态的 VM。 
- 补救措施：添加或删除网络接口之前，先停止虚拟机。
- 发生次数：通用

#### <a name="primary-network-interface"></a>主网络接口

- 适用于：此问题适用于所有支持的版本。
- 原因：无法更改 VM 的主要 NIC。 删除或分离主要 NIC 导致启动 VM 时出现问题。
- 发生次数：通用

### <a name="virtual-network-gateway"></a>虚拟网络网关

#### <a name="alerts"></a>警报

- 适用于：此问题适用于所有支持的版本。
- 原因：在用户门户中，“虚拟网络网关”边栏选项卡显示使用“警报”的选项。   Azure Stack Hub 目前不支持此功能。
- 发生次数：通用

#### <a name="active-active"></a>主动-主动

- 适用于：此问题适用于所有支持的版本。
- 原因：在用户门户（创建期间）和“虚拟网络网关”的资源菜单中，看到用于启用“主动-主动”配置的选项。   Azure Stack Hub 目前不支持此功能。
- 发生次数：通用

#### <a name="vpn-troubleshooter"></a>VPN 故障排除程序

- 适用于：此问题适用于所有支持的版本。
- 原因：在用户门户中，“连接”边栏选项卡显示一项名为“VPN 故障排除程序”的功能。   Azure Stack Hub 目前不支持此功能。
- 发生次数：通用

#### <a name="documentation"></a>文档

- 适用于：此问题适用于所有支持的版本。
- 原因：虚拟网络网关概述页中的文档链接链接到特定于 Azure 的文档，而不是 Azure Stack Hub 文档。 使用以下链接获取 Azure Stack Hub 文档：

  - [网关 SKU](../user/azure-stack-vpn-gateway-about-vpn-gateways.md#gateway-skus)
  - [高可用性连接](../user/azure-stack-vpn-gateway-about-vpn-gateways.md#gateway-availability)
  - [在 Azure Stack Hub 上配置 BGP](../user/azure-stack-vpn-gateway-settings.md#gateway-requirements)
  - [ExpressRoute 线路](azure-stack-connect-expressroute.md)
  - [指定自定义的 IPsec/IKE 策略](../user/azure-stack-vpn-gateway-settings.md#ipsecike-parameters)

## <a name="compute"></a>计算

### <a name="vm-boot-diagnostics"></a>VM 启动诊断

- 适用于：此问题适用于所有支持的版本。
- 原因：创建新的 Windows 虚拟机 (VM) 时，可能会显示以下错误：**无法启动虚拟机 'vm-name'。错误：无法更新 VM 'vm-name' 的串行输出设置**。 如果在 VM 上启用了启动诊断，但删除了启动诊断存储帐户，则会发生该错误。
- 补救措施：使用以前所用的相同名称重新创建存储帐户。
- 发生次数：通用

### <a name="consumed-compute-quota"></a>已消耗的计算配额

- 适用于：此问题适用于所有支持的版本。
- 原因：创建新虚拟机时，可能会收到一则错误消息，例如“此订阅在此位置的区域 vCPU 总数已达到上限。  此订阅使用了所有可用的 50 个区域 vCPU”。 这表示可用的核心配额总计已达到上限。
- 补救措施：请求操作员提供配额更高的附加计划。 编辑当前计划的配额不起作用，也不会反映提高的配额。
- 发生次数：罕见

### <a name="privileged-endpoint"></a>特权终结点

- 适用于：此问题适用于 1910 和更低版本。
- 原因：无法从运行非英语版 Windows 的计算机连接到特权终结点 (ERC VM)。
- 补救措施：这是一个已知问题，已在 1910 以上的版本中修复。 解决方法之一是，使用 **en-US** 区域性运行 **New-PSSession** 和 **Enter-PSSession** Powershell cmdlet；有关示例，请使用以下脚本来设置区域性： https://resources.oreilly.com/examples/9780596528492/blob/master/Use-Culture.ps1 。
- 发生次数：罕见

### <a name="virtual-machine-scale-set"></a>虚拟机规模集

#### <a name="create-failures-during-patch-and-update-on-4-node-azure-stack-hub-environments"></a>在 4 节点的 Azure Stack Hub 环境中进行修补和更新时出现故障

- 适用于：此问题适用于所有支持的版本。
- 原因：在包含 3 个容错域的可用性集中创建 VM 以及创建虚拟机规模集实例失败，在一个 4 节点 Azure Stack Hub 环境中进行更新时出现 **FabricVmPlacementErrorUnsupportedFaultDomainSize** 错误。
- 补救措施：可以在包含 2 个容错域的可用性集中成功创建单一 VM。 但是，在 4 节点 Azure Stack Hub 部署中进行更新时，仍然不能创建规模集实例。

<!-- ## Storage -->
<!-- ## SQL and MySQL-->
<!-- ## App Service -->
<!-- ## Usage -->
<!-- ### Identity -->
<!-- ### Marketplace -->
::: moniker-end

::: moniker range="azs-1908"
## <a name="1908-update-process"></a>1908 更新过程

- 适用于：此问题适用于所有支持的版本。
- 原因：尝试安装 Azure Stack Hub 更新时，更新状态可能会失败并将状态更改为 **PreparationFailed**。 这是因为更新资源提供程序 (URP) 无法正确将文件从存储容器传输到内部基础结构共享进行处理。
- 补救措施：从版本 1901 (1.1901.0.95) 开始，可以通过再次单击“立即更新”（而不是“恢复”）来解决此问题。   然后，URP 会清理上次尝试更新时下载的文件，并重新开始下载。 如果此问题持续存在，建议按照[“安装更新”部分](azure-stack-apply-updates.md#install-updates-and-monitor-progress)的说明手动上传更新包。
- 发生次数：通用

## <a name="portal"></a>门户

### <a name="administrative-subscriptions"></a>管理订阅

- 适用于：此问题适用于所有支持的版本。
- 原因：不应使用版本 1804 中引入的两个管理订阅。 这两种订阅类型为“计量订阅”和“消耗订阅”。  
- 补救措施：如果有资源在这两个订阅上运行，请在用户订阅中重新创建这些资源。
- 发生次数：通用

### <a name="subscriptions-properties-blade"></a>“订阅属性”边栏选项卡

- 适用于：此问题适用于所有支持的版本。
- 原因：在管理员门户中，订阅的“属性”  边栏选项卡未正确加载
- 补救措施：可以在“订阅概述”边栏选项卡的“概要”窗格中查看这些订阅属性   。
- 发生次数：通用

### <a name="subscriptions-lock-blade"></a>“订阅锁定”边栏选项卡

- 适用于：此问题适用于所有支持的版本。
- 原因：在管理员门户中，用户订阅的“锁定”  边栏选项卡有两个按钮，其带有“订阅”标签  。
- 发生次数：通用

### <a name="subscription-permissions"></a>订阅权限

- 适用于：此问题适用于所有支持的版本。
- 原因：无法使用 Azure Stack Hub 门户查看订阅的权限。
- 补救措施：使用 [PowerShell 验证权限](https://docs.microsoft.com/powershell/module/azurerm.resources/get-azurermroleassignment)。
- 发生次数：通用

### <a name="storage-account-settings"></a>存储帐户设置

- 适用于：此问题适用于所有支持的版本。
- 原因：在用户门户中，存储帐户的“配置”边栏选项卡显示用于更改**安全传输类型**的选项。  Azure Stack Hub 目前不支持此功能。
- 发生次数：通用

### <a name="upload-blob"></a>上传 blob

- 适用于：此问题适用于所有支持的版本。
- 原因：在用户门户中尝试使用“OAuth(preview)”选项上传 Blob 时，任务将会失败并出现错误消息。 
- 补救措施：使用 SAS 选项上传 Blob。
- 发生次数：通用

### <a name="alert-for-network-interface-disconnected"></a>网络接口已断开连接的警报

- 适用于：此问题适用于 1908 版本。
- 原因：当线缆与网络适配器断开连接时，管理员门户中不显示警报。 此问题的原因是 Windows Server 2019 中默认会禁用此故障。
- 发生次数：通用

## <a name="networking"></a>网络

### <a name="load-balancer"></a>负载均衡器

- 适用于：此问题适用于所有支持的版本。 
- 原因：将可用性集 VM 添加到负载均衡器的后端池时，门户上显示错误消息，指出“无法保存负载均衡器后端池”。  这是门户的表面问题；此功能仍在起作用，VM 在内部已成功添加到后端池。 
- 发生次数：通用

### <a name="network-security-groups"></a>网络安全组

- 适用于：此问题适用于所有支持的版本。 
- 原因：无法在 NSG 中创建显式 **DenyAllOutbound** 规则，因为这会使 VM 部署所需的基础结构无法完成所有内部通信。
- 发生次数：通用

### <a name="service-endpoints"></a>服务终结点

- 适用于：此问题适用于所有支持的版本。
- 原因：在用户门户中，“虚拟网络”边栏选项卡显示使用“服务终结点”的选项。   Azure Stack Hub 目前不支持此功能。
- 发生次数：通用

### <a name="network-interface"></a>Linux

#### <a name="addingremoving-network-interface"></a>添加/删除网络接口

- 适用于：此问题适用于所有支持的版本。
- 原因：新网络接口无法添加到处于“正在运行”状态的 VM。 
- 补救措施：添加/删除网络接口之前，先停止虚拟机。
- 发生次数：通用

#### <a name="primary-network-interface"></a>主要网络接口

- 适用于：此问题适用于所有支持的版本。
- 原因：新网络接口无法添加到处于“正在运行”状态的 VM。 
- 补救措施：添加/删除网络接口之前，先停止虚拟机。
- 发生次数：通用

### <a name="virtual-network-gateway"></a>虚拟网络网关

#### <a name="alerts"></a>警报

- 适用于：此问题适用于所有支持的版本。
- 原因：在用户门户中，“虚拟网络网关”边栏选项卡显示使用“警报”的选项。   Azure Stack Hub 目前不支持此功能。
- 发生次数：通用

#### <a name="active-active"></a>主动-主动

- 适用于：此问题适用于所有支持的版本。
- 原因：在用户门户（创建期间）和“虚拟网络网关”的资源菜单中，看到用于启用“主动-主动”配置的选项。   Azure Stack Hub 目前不支持此功能。
- 发生次数：通用

#### <a name="vpn-troubleshooter"></a>VPN 故障排除程序

- 适用于：此问题适用于所有支持的版本。
- 原因：在用户门户中，“连接”边栏选项卡显示一项名为“VPN 故障排除程序”的功能。   Azure Stack Hub 目前不支持此功能。
- 发生次数：通用

#### <a name="documentation"></a>文档

- 适用于：此问题适用于所有支持的版本。
- 原因：虚拟网络网关概述页中的文档链接链接到特定于 Azure 的文档，而不是 Azure Stack Hub 文档。 使用以下链接获取 Azure Stack Hub 文档：

  - [网关 SKU](../user/azure-stack-vpn-gateway-about-vpn-gateways.md#gateway-skus)
  - [高可用性连接](../user/azure-stack-vpn-gateway-about-vpn-gateways.md#gateway-availability)
  - [在 Azure Stack Hub 上配置 BGP](../user/azure-stack-vpn-gateway-settings.md#gateway-requirements)
  - [ExpressRoute 线路](azure-stack-connect-expressroute.md)
  - [指定自定义的 IPsec/IKE 策略](../user/azure-stack-vpn-gateway-settings.md#ipsecike-parameters)

## <a name="compute"></a>计算

### <a name="vm-boot-diagnostics"></a>VM 启动诊断

- 适用于：此问题适用于所有支持的版本。
- 原因：创建新的 Windows 虚拟机 (VM) 时，可能会显示以下错误：**无法启动虚拟机 'vm-name'。错误：无法更新 VM 'vm-name' 的串行输出设置**。 如果在 VM 上启用了启动诊断，但删除了启动诊断存储帐户，则会发生该错误。
- 补救措施：使用以前所用的相同名称重新创建存储帐户。
- 发生次数：通用

### <a name="virtual-machine-scale-set"></a>虚拟机规模集

#### <a name="create-failures-during-patch-and-update-on-4-node-azure-stack-hub-environments"></a>在 4 节点的 Azure Stack Hub 环境中进行修补和更新时出现故障

- 适用于：此问题适用于所有支持的版本。
- 原因：在包含 3 个容错域的可用性集中创建 VM 以及创建虚拟机规模集实例失败，在一个 4 节点 Azure Stack Hub 环境中进行更新时出现 **FabricVmPlacementErrorUnsupportedFaultDomainSize** 错误。
- 补救措施：可以在包含 2 个容错域的可用性集中成功创建单一 VM。 但是，在 4 节点 Azure Stack Hub 上进行更新时，仍然不能创建规模集实例。

### <a name="ubuntu-ssh-access"></a>Ubuntu SSH 访问

- 适用于：此问题适用于所有支持的版本。
- 原因：如果使用创建时已启用 SSH 授权的 Ubuntu 18.04 VM，则无法使用 SSH 密钥登录。
- 补救措施：在预配后使用针对 Linux 扩展的 VM 访问权限来实现 SSH 密钥，或者使用基于密码的身份验证。
- 发生次数：通用

### <a name="virtual-machine-scale-set-reset-password-does-not-work"></a>虚拟机规模集重置密码无法进行

- 适用于：此问题适用于所有支持的版本。
- 原因：规模集 UI 中出现新的密码重置边栏选项卡，但 Azure Stack Hub 尚不支持在规模集上重置密码。
- 补救措施：无。
- 发生次数：通用

### <a name="rainy-cloud-on-scale-set-diagnostics"></a>规模集诊断上出现哭泣的云

- 适用于：此问题适用于所有支持的版本。
- 原因：虚拟机规模集概述页显示空白图表。 单击该空白图表会打开“哭泣的云”边栏选项卡。 这是规模集诊断信息的图表（例如 CPU 百分比），但这不是当前 Azure Stack Hub 内部版本支持的功能。
- 补救措施：无。
- 发生次数：通用

### <a name="virtual-machine-diagnostic-settings-blade"></a>虚拟机诊断设置边栏选项卡

- 适用于：此问题适用于所有支持的版本。    
- 原因：虚拟机诊断设置边栏选项卡包含“接收器”选项卡，用于请求 **Application Insights 帐户**  。 这是新边栏选项卡中的结果，Azure Stack Hub 中尚不支持此功能。
- 补救措施：无。
- 发生次数：通用

<!-- ## Storage -->
<!-- ## SQL and MySQL-->
<!-- ## App Service -->
<!-- ## Usage -->
<!-- ### Identity -->
<!-- ### Marketplace -->
::: moniker-end

::: moniker range="azs-1907"
## <a name="1907-update-process"></a>1907 更新过程

- 适用于：此问题适用于所有支持的版本。
- 原因：尝试安装 1907 Azure Stack Hub 更新时，更新状态可能会失败并更改为 **PreparationFailed**。 这是因为更新资源提供程序 (URP) 无法正确将文件从存储容器传输到内部基础结构共享进行处理。
- 补救措施：从版本 1901 (1.1901.0.95) 开始，可以通过再次单击“立即更新”（而不是“恢复”）来解决此问题。   然后，URP 会清理上次尝试更新时下载的文件，并重新开始下载。 如果此问题持续存在，建议按照[“导入并安装更新”部分](azure-stack-apply-updates.md)的说明手动上传更新包。
- 发生次数：通用

## <a name="portal"></a>门户

### <a name="administrative-subscriptions"></a>管理订阅

- 适用于：此问题适用于所有支持的版本。
- 原因：不应使用版本 1804 中引入的两个管理订阅。 这两种订阅类型为“计量订阅”和“消耗订阅”。  
- 补救措施：如果有资源在这两个订阅上运行，请在用户订阅中重新创建这些资源。
- 发生次数：通用

### <a name="subscriptions-properties-blade"></a>“订阅属性”边栏选项卡

- 适用于：此问题适用于所有支持的版本。
- 原因：在管理员门户中，订阅的“属性”  边栏选项卡未正确加载
- 补救措施：可以在“订阅概述”边栏选项卡的“概要”窗格中查看这些订阅属性   。
- 发生次数：通用

### <a name="subscription-permissions"></a>订阅权限

- 适用于：此问题适用于所有支持的版本。
- 原因：无法使用 Azure Stack Hub 门户查看订阅的权限。
- 补救措施：使用 [PowerShell 验证权限](https://docs.microsoft.com/powershell/module/azurerm.resources/get-azurermroleassignment)。
- 发生次数：通用

### <a name="storage-account-settings"></a>存储帐户设置

- 适用于：此问题适用于所有支持的版本。
- 原因：在用户门户中，存储帐户的“配置”边栏选项卡显示用于更改**安全传输类型**的选项。  Azure Stack Hub 目前不支持此功能。
- 发生次数：通用

### <a name="upload-blob"></a>上传 blob

- 适用于：此问题适用于所有支持的版本。
- 原因：在用户门户中尝试使用“OAuth(preview)”选项上传 Blob 时，任务将会失败并出现错误消息。 
- 补救措施：使用 SAS 选项上传 Blob。
- 发生次数：通用

## <a name="networking"></a>网络

### <a name="load-balancer"></a>负载均衡器

- 适用于：此问题适用于所有支持的版本。 
- 原因：将可用性集 VM 添加到负载均衡器的后端池时，门户上显示错误消息，指出“无法保存负载均衡器后端池”。  这是门户的表面问题；此功能仍在起作用，VM 在内部已成功添加到后端池。 
- 发生次数：通用

### <a name="network-security-groups"></a>网络安全组

- 适用于：此问题适用于所有支持的版本。 
- 原因：无法在 NSG 中创建显式 **DenyAllOutbound** 规则，因为这会使 VM 部署所需的基础结构无法完成所有内部通信。
- 发生次数：通用

### <a name="service-endpoints"></a>服务终结点

- 适用于：此问题适用于所有支持的版本。
- 原因：在用户门户中，“虚拟网络”边栏选项卡显示使用“服务终结点”的选项。   Azure Stack Hub 目前不支持此功能。
- 发生次数：通用

### <a name="network-interface"></a>Linux

#### <a name="addingremoving-network-interface"></a>添加/删除网络接口

- 适用于：此问题适用于所有支持的版本。
- 原因：新网络接口无法添加到处于“正在运行”状态的 VM。 
- 补救措施：添加/删除网络接口之前，先停止虚拟机。
- 发生次数：通用

#### <a name="primary-network-interface"></a>主要网络接口

- 适用于：此问题适用于所有支持的版本。
- 原因：新网络接口无法添加到处于“正在运行”状态的 VM。 
- 补救措施：添加/删除网络接口之前，先停止虚拟机。
- 发生次数：通用

### <a name="virtual-network-gateway"></a>虚拟网络网关

#### <a name="alerts"></a>警报

- 适用于：此问题适用于所有支持的版本。
- 原因：在用户门户中，“虚拟网络网关”边栏选项卡显示使用“警报”的选项。   Azure Stack Hub 目前不支持此功能。
- 发生次数：通用

#### <a name="active-active"></a>主动-主动

- 适用于：此问题适用于所有支持的版本。
- 原因：在用户门户（创建期间）和“虚拟网络网关”的资源菜单中，看到用于启用“主动-主动”配置的选项。   Azure Stack Hub 目前不支持此功能。
- 发生次数：通用

#### <a name="vpn-troubleshooter"></a>VPN 故障排除程序

- 适用于：此问题适用于所有支持的版本。
- 原因：在用户门户中，“连接”边栏选项卡显示一项名为“VPN 故障排除程序”的功能。   Azure Stack Hub 目前不支持此功能。
- 发生次数：通用

### <a name="network-connection-type"></a>网络连接类型

- 适用于：此问题适用于任何 1906 或 1907 环境。 
- 原因：在用户门户中，“添加连接”  边栏选项卡显示了使用 **VNet-to-VNet** 的选项。 Azure Stack Hub 目前不支持此功能。 
- 发生次数：通用 

#### <a name="documentation"></a>文档

- 适用于：此问题适用于所有支持的版本。
- 原因：虚拟网络网关概述页中的文档链接链接到特定于 Azure 的文档，而不是 Azure Stack Hub 文档。 使用以下链接获取 Azure Stack Hub 文档：

  - [网关 SKU](../user/azure-stack-vpn-gateway-about-vpn-gateways.md#gateway-skus)
  - [高可用性连接](../user/azure-stack-vpn-gateway-about-vpn-gateways.md#gateway-availability)
  - [在 Azure Stack Hub 上配置 BGP](../user/azure-stack-vpn-gateway-settings.md#gateway-requirements)
  - [ExpressRoute 线路](azure-stack-connect-expressroute.md)
  - [指定自定义的 IPsec/IKE 策略](../user/azure-stack-vpn-gateway-settings.md#ipsecike-parameters)

## <a name="compute"></a>计算

### <a name="vm-boot-diagnostics"></a>VM 启动诊断

- 适用于：此问题适用于所有支持的版本。
- 原因：创建新的 Windows 虚拟机 (VM) 时，可能会显示以下错误：**无法启动虚拟机 'vm-name'。错误：无法更新 VM 'vm-name' 的串行输出设置**。 如果在 VM 上启用了启动诊断，但删除了启动诊断存储帐户，则会发生该错误。
- 补救措施：使用以前所用的相同名称重新创建存储帐户。
- 发生次数：通用

### <a name="virtual-machine-scale-set"></a>虚拟机规模集

#### <a name="create-failures-during-patch-and-update-on-4-node-azure-stack-hub-environments"></a>在 4 节点的 Azure Stack Hub 环境中进行修补和更新时出现故障

- 适用于：此问题适用于所有支持的版本。
- 原因：在包含 3 个容错域的可用性集中创建 VM 以及创建虚拟机规模集实例失败，在一个 4 节点 Azure Stack Hub 环境中进行更新时出现 **FabricVmPlacementErrorUnsupportedFaultDomainSize** 错误。
- 补救措施：可以在包含 2 个容错域的可用性集中成功创建单一 VM。 但是，在 4 节点 Azure Stack Hub 上进行更新时，仍然不能创建规模集实例。

### <a name="ubuntu-ssh-access"></a>Ubuntu SSH 访问

- 适用于：此问题适用于所有支持的版本。
- 原因：如果使用创建时已启用 SSH 授权的 Ubuntu 18.04 VM，则无法使用 SSH 密钥登录。
- 补救措施：在预配后使用针对 Linux 扩展的 VM 访问权限来实现 SSH 密钥，或者使用基于密码的身份验证。
- 发生次数：通用

### <a name="virtual-machine-scale-set-reset-password-does-not-work"></a>虚拟机规模集重置密码无法进行

- 适用于：此问题适用于 1906 和 1907 版本。
- 原因：规模集 UI 中出现新的密码重置边栏选项卡，但 Azure Stack Hub 尚不支持在规模集上重置密码。
- 补救措施：无。
- 发生次数：通用

### <a name="rainy-cloud-on-scale-set-diagnostics"></a>规模集诊断上出现哭泣的云

- 适用于：此问题适用于 1906 和 1907 版本。
- 原因：虚拟机规模集概述页显示空白图表。 单击该空白图表会打开“哭泣的云”边栏选项卡。 这是规模集诊断信息的图表（例如 CPU 百分比），但这不是当前 Azure Stack Hub 内部版本支持的功能。
- 补救措施：无。
- 发生次数：通用

### <a name="virtual-machine-diagnostic-settings-blade"></a>虚拟机诊断设置边栏选项卡

- 适用于：此问题适用于 1906 和 1907 版本。    
- 原因：虚拟机诊断设置边栏选项卡包含“接收器”选项卡，用于请求 **Application Insights 帐户**  。 这是新边栏选项卡中的结果，Azure Stack Hub 中尚不支持此功能。
- 补救措施：无。
- 发生次数：通用

<!-- ## Storage -->
<!-- ## SQL and MySQL-->
<!-- ## App Service -->
<!-- ## Usage -->
<!-- ### Identity -->
<!-- ### Marketplace -->
::: moniker-end

::: moniker range="azs-1906"
## <a name="1906-update-process"></a>1906 更新过程

- 适用于：此问题适用于所有支持的版本。
- 原因：尝试安装 1906 Azure Stack Hub 更新时，更新状态可能会失败并更改为 **PreparationFailed**。 这是因为更新资源提供程序 (URP) 无法正确将文件从存储容器传输到内部基础结构共享进行处理。 
- 补救措施：从版本 1901 (1.1901.0.95) 开始，可以通过再次单击“立即更新”（而不是“恢复”）来解决此问题。   然后，URP 会清理上次尝试更新时下载的文件，并重新开始下载。 如果此问题持续存在，建议按照[“导入并安装更新”部分](azure-stack-apply-updates.md)的说明手动上传更新包。
- 发生次数：通用

## <a name="portal"></a>门户

### <a name="administrative-subscriptions"></a>管理订阅

- 适用于：此问题适用于所有支持的版本。
- 原因：不应使用版本 1804 中引入的两个管理订阅。 这两种订阅类型为“计量订阅”和“消耗订阅”。  
- 补救措施：如果有资源在这两个订阅上运行，请在用户订阅中重新创建这些资源。
- 发生次数：通用

### <a name="subscription-resources"></a>订阅资源

- 适用于：此问题适用于所有支持的版本。
- 原因：删除用户订阅生成孤立的资源。
- 补救措施：先删除用户资源或整个资源组，然后再删除用户订阅。
- 发生次数：通用

### <a name="subscription-permissions"></a>订阅权限

- 适用于：此问题适用于所有支持的版本。
- 原因：无法使用 Azure Stack Hub 门户查看订阅的权限。
- 补救措施：使用 [PowerShell 验证权限](https://docs.microsoft.com/powershell/module/azurerm.resources/get-azurermroleassignment)。
- 发生次数：通用

### <a name="subscriptions-properties-blade"></a>“订阅属性”边栏选项卡
- 适用于：此问题适用于所有支持的版本。
- 原因：在管理员门户中，订阅的“属性”  边栏选项卡未正确加载
- 补救措施：可以在“订阅概述”边栏选项卡的“概要”窗格中查看这些订阅属性
- 发生次数：通用

### <a name="storage-account-settings"></a>存储帐户设置

- 适用于：此问题适用于所有支持的版本。
- 原因：在用户门户中，存储帐户的“配置”边栏选项卡显示用于更改**安全传输类型**的选项。  Azure Stack Hub 目前不支持此功能。
- 发生次数：通用

### <a name="upload-blob"></a>上传 blob

- 适用于：此问题适用于所有支持的版本。
- 原因：在用户门户中尝试使用“OAuth(preview)”选项上传 Blob 时，任务将会失败并出现错误消息。 
- 补救措施：使用 SAS 选项上传 Blob。
- 发生次数：通用

### <a name="update"></a>更新

- 适用于：此问题适用于 1906 版本。
- 原因：在操作员门户中，修补程序的更新状态显示更新状态不正确。 即使安装仍在进行，初始状态也会指示无法安装更新。
- 补救措施：刷新门户，然后状态将更新为“正在进行”。
- 发生次数：间歇性

## <a name="networking"></a>网络

### <a name="service-endpoints"></a>服务终结点

- 适用于：此问题适用于所有支持的版本。
- 原因：在用户门户中，“虚拟网络”边栏选项卡显示使用“服务终结点”的选项。   Azure Stack Hub 目前不支持此功能。
- 发生次数：通用

### <a name="network-interface"></a>Linux

- 适用于：此问题适用于所有支持的版本。
- 原因：新网络接口无法添加到处于“正在运行”状态的 VM。 
- 补救措施：添加/删除网络接口之前，先停止虚拟机。
- 发生次数：通用

### <a name="virtual-network-gateway"></a>虚拟网络网关

#### <a name="alerts"></a>警报

- 适用于：此问题适用于所有支持的版本。
- 原因：在用户门户中，“虚拟网络网关”边栏选项卡显示使用“警报”的选项。   Azure Stack Hub 目前不支持此功能。
- 发生次数：通用

#### <a name="active-active"></a>主动-主动

- 适用于：此问题适用于所有支持的版本。
- 原因：在用户门户（创建期间）和“虚拟网络网关”的资源菜单中，看到用于启用“主动-主动”配置的选项。   Azure Stack Hub 目前不支持此功能。
- 发生次数：通用

#### <a name="vpn-troubleshooter"></a>VPN 故障排除程序

- 适用于：此问题适用于所有支持的版本。
- 原因：在用户门户中，“连接”边栏选项卡显示一项名为“VPN 故障排除程序”的功能。   Azure Stack Hub 目前不支持此功能。
- 发生次数：通用

#### <a name="documentation"></a>文档

- 适用于：此问题适用于所有支持的版本。
- 原因：虚拟网络网关概述页中的文档链接链接到特定于 Azure 的文档，而不是 Azure Stack Hub 文档。 请使用以下链接获取 Azure Stack Hub 文档：

  - [网关 SKU](../user/azure-stack-vpn-gateway-about-vpn-gateways.md#gateway-skus)
  - [高可用性连接](../user/azure-stack-vpn-gateway-about-vpn-gateways.md#gateway-availability)
  - [在 Azure Stack Hub 上配置 BGP](../user/azure-stack-vpn-gateway-settings.md#gateway-requirements)
  - [ExpressRoute 线路](azure-stack-connect-expressroute.md)
  - [指定自定义的 IPsec/IKE 策略](../user/azure-stack-vpn-gateway-settings.md#ipsecike-parameters)

### <a name="load-balancer"></a>负载均衡器

#### <a name="add-backend-pool"></a>添加后端池

- 适用于：此问题适用于所有支持的版本。
- 原因：在用户门户尝试将**后端池**添加到**负载均衡器**时，该操作失败并出现错误消息“无法更新负载均衡器...”。 
- 补救措施：使用 PowerShell、CLI 或资源管理器模板将后端池关联到负载均衡器资源。
- 发生次数：通用

#### <a name="create-inbound-nat"></a>创建入站 NAT

- 适用于：此问题适用于所有支持的版本。
- 原因：在用户门户尝试为**负载均衡器**创建**入站 NAT 规则**时，该操作失败并出现错误消息“无法更新负载均衡器...”。 
- 补救措施：使用 PowerShell、CLI 或资源管理器模板将后端池关联到负载均衡器资源。
- 发生次数：通用

## <a name="compute"></a>计算

### <a name="vm-boot-diagnostics"></a>VM 启动诊断

- 适用于：此问题适用于所有支持的版本。
- 原因：创建新的 Windows 虚拟机 (VM) 时，可能会显示以下错误：**无法启动虚拟机 'vm-name'。错误：无法更新 VM 'vm-name' 的串行输出设置**。 如果在 VM 上启用了启动诊断，但删除了启动诊断存储帐户，则会发生该错误。
- 补救措施：使用以前所用的相同名称重新创建存储帐户。
- 发生次数：通用

### <a name="virtual-machine-scale-set"></a>虚拟机规模集


#### <a name="create-failures-during-patch-and-update-on-4-node-azure-stack-hub-environments"></a>在 4 节点的 Azure Stack Hub 环境中进行修补和更新时出现故障

- 适用于：此问题适用于所有支持的版本。
- 原因：在包含 3 个容错域的可用性集中创建 VM 以及创建虚拟机规模集实例失败，在一个 4 节点 Azure Stack Hub 环境中进行更新时出现 **FabricVmPlacementErrorUnsupportedFaultDomainSize** 错误。
- 补救措施：可以在包含 2 个容错域的可用性集中成功创建单一 VM。 但是，在 4 节点 Azure Stack Hub 上进行更新时，仍然不能创建规模集实例。

### <a name="ubuntu-ssh-access"></a>Ubuntu SSH 访问

- 适用于：此问题适用于所有支持的版本。
- 原因：如果使用创建时已启用 SSH 授权的 Ubuntu 18.04 VM，则无法使用 SSH 密钥登录。
- 补救措施：在预配后使用针对 Linux 扩展的 VM 访问权限来实现 SSH 密钥，或者使用基于密码的身份验证。
- 发生次数：通用

### <a name="virtual-machine-scale-set-reset-password-does-not-work"></a>虚拟机规模集重置密码无法进行

- 适用于：此问题适用于 1906 版本。
- 原因：规模集 UI 中出现新的密码重置边栏选项卡，但 Azure Stack Hub 尚不支持在规模集上重置密码。
- 补救措施：无。
- 发生次数：通用

### <a name="rainy-cloud-on-scale-set-diagnostics"></a>规模集诊断上出现哭泣的云

- 适用于：此问题适用于 1906 版本。
- 原因：虚拟机规模集概述页显示空白图表。 单击该空白图表会打开“哭泣的云”边栏选项卡。 这是规模集诊断信息的图表（例如 CPU 百分比），但这不是当前 Azure Stack Hub 内部版本支持的功能。
- 补救措施：无。
- 发生次数：通用

### <a name="issues-creating-resources"></a>创建资源时的问题

- 适用于：此问题适用于 1906 版本。
- 原因：1906 中存在一个与创建资源时的自定义角色和权限分配相关的已知问题。 即使你拥有正确的权限，在创建资源时也可能会遇到问题。
- 补救措施：请更新到内部版本 1907 来缓解此问题。
- 发生次数：通用

### <a name="virtual-machine-diagnostic-settings-blade"></a>虚拟机诊断设置边栏选项卡

- 适用于：此问题适用于 1906 版本。
- 原因：虚拟机诊断设置边栏选项卡包含“接收器”选项卡，用于请求 **Application Insights 帐户**  。 这是新边栏选项卡中的结果，Azure Stack Hub 中尚不支持此功能。
- 补救措施：无。
- 发生次数：通用

<!-- ## Storage -->
<!-- ## SQL and MySQL-->
<!-- ## App Service -->
<!-- ## Usage -->
<!-- ### Identity -->
<!-- ### Marketplace -->
::: moniker-end

::: moniker range=">=azs-1906"
## <a name="archive"></a>Archive

若要访问旧版本的已存档已知问题，请使用左侧目录上方的版本选择器下拉列表，然后选择要查看的版本。

## <a name="next-steps"></a>后续步骤

- [查看更新活动清单](release-notes-checklist.md)
- [查看安全更新列表](release-notes-security-updates.md)
::: moniker-end

<!------------------------------------------------------------>
<!------------------- UNSUPPORTED VERSIONS ------------------->
<!------------------------------------------------------------>
::: moniker range="azs-1905"
## <a name="1905-archived-known-issues"></a>1905 已存档的已知问题
::: moniker-end
::: moniker range="azs-1904"
## <a name="1904-archived-known-issues"></a>1904 已存档的已知问题
::: moniker-end
::: moniker range="azs-1903"
## <a name="1903-archived-known-issues"></a>1903 已存档的已知问题
::: moniker-end
::: moniker range="azs-1902"
## <a name="1902-archived-known-issues"></a>1902 已存档的已知问题
::: moniker-end
::: moniker range="azs-1901"
## <a name="1901-archived-known-issues"></a>1901 已存档的已知问题
::: moniker-end
::: moniker range="azs-1811"
## <a name="1811-archived-known-issues"></a>1811 已存档的已知问题
::: moniker-end
::: moniker range="azs-1809"
## <a name="1809-archived-known-issues"></a>1809 已存档的已知问题
::: moniker-end
::: moniker range="azs-1808"
## <a name="1808-archived-known-issues"></a>1808 已存档的已知问题
::: moniker-end
::: moniker range="azs-1807"
## <a name="1807-archived-known-issues"></a>1807 已存档的已知问题
::: moniker-end
::: moniker range="azs-1805"
## <a name="1805-archived-known-issues"></a>1805 已存档的已知问题
::: moniker-end
::: moniker range="azs-1804"
## <a name="1804-archived-known-issues"></a>1804 已存档的已知问题
::: moniker-end
::: moniker range="azs-1803"
## <a name="1803-archived-known-issues"></a>1803 已存档的已知问题
::: moniker-end
::: moniker range="azs-1802"
## <a name="1802-archived-known-issues"></a>1802 已存档的已知问题
::: moniker-end

::: moniker range="<azs-1906"
可以访问 [TechNet 库中旧版本 Azure Stack Hub 的已知问题](https://aka.ms/azsarchivedrelnotes)。 提供这些已存档文档仅供参考，并不意味着支持这些版本。 有关 Azure Stack Hub 支持的信息，请参阅 [Azure Stack Hub 服务策略](azure-stack-servicing-policy.md)。 如需进一步的帮助，请联系 Azure 客户支持服务。
::: moniker-end