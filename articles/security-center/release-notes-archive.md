---
title: Azure 安全中心的新增功能的存档
description: 介绍了 Azure 安全中心在六个月前和更早之前的新增功能和更改内容。
services: security-center
documentationcenter: na
author: memildin
manager: rkarlin
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/27/2020
ms.author: v-tawe
origin.date: 05/11/2020
ms.openlocfilehash: 5722947fe0fa277935e9111567dc74a35e89cd3d
ms.sourcegitcommit: cbaa1aef101f67bd094f6ad0b4be274bbc2d2537
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/28/2020
ms.locfileid: "84126796"
---
# <a name="archive-for-whats-new-in-azure-security-center"></a>Azure 安全中心的新增功能的存档？

主要的 [Azure Active Directory 中的新功能？](release-notes.md)发行说明页面包含过去六个月的更新，而此页面包含较旧的项。

本页提供以下事项的信息：

- 新增功能
- Bug 修复
- 已弃用的功能

## <a name="november-2019"></a>2019 年 11 月



### <a name="threat-protection-for-azure-storage-includes-malware-reputation-screening"></a>Azure 存储的威胁防护包括恶意软件信誉筛查

Azure 存储的威胁防护提供了由 Microsoft 威胁情报提供支持的新检测功能，可检测恶意软件向 Azure 存储进行的上传（使用哈希信誉分析作为检测方法）以及来自活动 Tor 出口节点（一种匿名代理）的可疑访问。 你现在可以使用 Azure 安全中心查看在各个存储帐户中检测到的恶意软件。


<!--Workflow automation is not available -->




### <a name="quick-fix-for-bulk-resources-generally-available"></a>适用于大量资源的快速修复已正式发布

由于在安全评分中用户需要完成许多任务，因此，在大量资源中高效地修正问题变得越来越有挑战性。

若要简化对安全错误配置的修正、快速修正针对大量资源的建议并提高安全评分，请使用快速修复修正。

此操作允许你选择要对其应用修正的资源，并启动一个将代表你对设置进行配置的修正操作。

快速修复目前已在安全中心推荐页面中正式提供给客户。

请在[安全建议参考指南](recommendations-reference.md)中查看哪些建议启用了快速修复。




### <a name="additional-regulatory-compliance-standards-preview"></a>其他合规性标准（预览版）

“合规性”仪表板根据安全中心评估提供对你的合规状况的见解。 此仪表板显示你的环境在遵循特定法规标准和行业基准所指定的控制和要求方面的情况，并为如何满足这些要求提供了规范性建议。

到目前为止，合规性仪表板支持四个内置标准：Azure CIS 1.1.0、PCI-DSS、ISO 27001 和 SOC-TSP。 我们现在宣布推出其他受支持标准的公共预览版：NIST SP 800-53 R4、SWIFT CSP CSCF v2020、加拿大联邦 PBMM 和 UK Official 以及 UK NHS。 我们还会发布 Azure CIS 1.1.0 的更新版，涵盖该标准中的更多控制措施并增强扩展性。

[详细了解如何在合规性仪表板中自定义标准集](update-regulatory-compliance-packages.md)。


### <a name="threat-protection-for-azure-kubernetes-service-preview"></a>Azure Kubernetes 服务的威胁防护（预览版）

Kubernetes 正在快速成为用于在云中部署和管理软件的新标准。 很少有用户对 Kubernetes 有丰富的经验，并且很多人只关注一般的工程和管理，而忽略安全方面。 需要小心配置 Kubernetes 环境以确保安全，确保没有将相应的攻击面暴露给专门针对容器的攻击者，不要门户大开。 安全中心正在将其在容器空间中的支持扩展到 Azure 中增长最快的服务之一 - Azure Kubernetes 服务 (AKS)。

此公共预览版中的新功能包括：

- **发现和可见性** - 在安全中心的已注册订阅内持续发现托管的 AKS 实例。
- **安全评分建议** - 这是一些可操作项，帮助客户遵守 AKS 中作为客户安全评分的一部分的安全最佳做法，例如，“应使用基于角色的访问控制来限制对 Kubernetes 服务群集的访问”。
- **威胁检测** - 基于主机和群集的分析，例如“检测到特权容器”。


### <a name="virtual-machine-vulnerability-assessment-preview"></a>虚拟机漏洞评估（预览版）

安装在虚拟机中的应用程序可能经常会有可导致虚拟机被破坏的漏洞。 我们宣布，安全中心标准层包括了针对虚拟机的内置漏洞评估，用户无需额外付费。 漏洞评估（在公共预览版中由 Qualys 提供支持）将允许你持续扫描虚拟机上安装的所有应用程序，以查找易受攻击的应用程序，并在安全中心门户的体验中呈现这些结果。 安全中心负责所有部署操作，因此不需要用户做额外的工作。 今后，我们计划提供漏洞评估选项，以支持客户的独特业务需求。

[详细了解针对 Azure 虚拟机的漏洞评估](security-center-vulnerability-assessment-recommendations.md)。


### <a name="advanced-data-security-for-sql-servers-on-azure-virtual-machines-preview"></a>Azure 虚拟机上的 SQL Server 高级数据安全（预览版）

Azure 安全中心针对 IaaS VM 上运行的 SQL DB 提供的威胁防护和漏洞评估支持目前为公共预览版。

[漏洞评估](https://docs.azure.cn/sql-database/sql-vulnerability-assessment)是一项易于配置的服务，可以发现、跟踪并帮助修正潜在的数据库漏洞。 它可直观查看安全状况（这是 Azure 安全评分的一部分内容），包含解决安全问题的步骤，并可加强数据库的防御工事。

[高级威胁防护](https://docs.azure.cn/sql-database/sql-database-threat-detection-overview)会检测特定的异常活动，这些活动表明有人在尝试以异常且可能有害的方式访问或利用你的 SQL Server。 它不断监视数据库的可疑活动，并针对异常数据库访问模式提供操作导向的安全警报。 这些警报提供可疑活动的详细信息，并建议如何调查和缓解威胁。


### <a name="support-for-custom-policies-preview"></a>支持自定义策略（预览版）

Azure 安全中心现在支持自定义策略（预览版）。

我们的客户一直希望根据他们在 Azure Policy 中创建的策略，使用他们自己的安全评估来扩展他们在安全中心内的当前安全评估覆盖范围。 由于我们支持自定义策略，现在他们的这个目标可以实现了。

这些新策略将成为安全中心建议体验、安全评分和合规性标准仪表板的一部分。 由于我们支持自定义策略，你现在可以在 Azure Policy 中创建自定义计划，然后将其作为策略添加到安全中心，并将其可视化为建议。


### <a name="extending-azure-security-center-coverage-with-platform-for-community-and-partners"></a>通过适用于社区和合作伙伴的平台扩展 Azure 安全中心覆盖范围

使用安全中心不仅可以接收来自 Microsoft 的建议，还可以接收来自合作伙伴（例如 Check Point、Tenable 和 CyberArk）的现有解决方案中的建议，它们以后会提供更多的集成。  安全中心的简单加入流可以将你现有的解决方案连接到安全中心，使你能够在一个地方查看安全状况建议、运行统一的报告，以及根据内置的建议和合作伙伴的建议利用安全中心的所有功能。 你还可以将安全中心建议导出到合作伙伴产品。

[详细了解 Microsoft 智能安全协会](https://www.microsoft.com/security/partnerships/intelligent-security-association)。



### <a name="advanced-integrations-with-export-of-recommendations-and-alerts-preview"></a>与建议和警报导出功能进行的高级集成（预览版）

现在可以在除 Azure 门户或 API 之外的其他位置使用安全中心警报和建议，以便在安全中心的基础上实现企业级方案。 这些警报和建议可以直接导出到事件中心和 Log Analytics 工作区。 下面是你可以围绕这些新功能创建的几个工作流：

- 由于可以导出到 Log Analytics 工作区，因此你可以使用 Power BI 创建自定义仪表板。
- 由于可以导出到事件中心，因此你可以将安全中心警报和建议导出到第三方 SIEM、实时导出到第三方解决方案，或导出到 Azure 数据资源管理器。


### <a name="onboard-on-prem-servers-to-security-center-from-windows-admin-center-preview"></a>从 Windows 管理中心将本地服务器加入到安全中心（预览版）

Windows 管理中心是一个管理门户，适用于未部署在 Azure 中的 Windows 服务器，为它们提供一些 Azure 管理功能，例如备份和系统更新。 我们最近添加了一项用于加入这些非 Azure 服务器的功能，允许用户直接使用 Windows 管理中心体验通过 ASC 对这些服务器进行保护。

通过这种新体验，用户可将 WAC 服务器加入到 Azure 安全中心，并可直接在 Windows 管理中心体验中查看其安全警报和建议。


## <a name="september-2019"></a>2019 年 9 月

### <a name="managing-rules-with-adaptive-application-controls-improvements"></a>对通过自适应应用程序控制来管理规则这一功能的改进

使用自适应应用程序控制来管理虚拟机的规则的体验已改进。 Azure 安全中心的自适应应用程序控制帮助你控制哪些应用程序可以在虚拟机上运行。 除了对规则管理的一般改进之外，在添加新规则时还可以通过一项新权益来控制哪些文件类型会受到保护。

[详细了解自适应应用程序控制](security-center-adaptive-application.md)。


### <a name="control-container-security-recommendation-using-azure-policy"></a>使用 Azure Policy 控制容器安全建议

现在可以通过 Azure Policy 启用或禁用 Azure 安全中心提供的用于修复容器安全相关漏洞的建议。

若要查看已启用的安全策略，请从安全中心打开“安全策略”页。


## <a name="august-2019"></a>2019 年 8 月

### <a name="just-in-time-jit-vm-access-for-azure-firewall"></a>Azure 防火墙的实时 (JIT) VM 访问 

Azure 防火墙的实时 (JIT) VM 访问现已正式发布。 除了受 NSG 保护的环境之外，还可以使用它来保护受 Azure 防火墙保护的环境。

使用 NSG 和 Azure 防火墙规则时，JIT VM 访问仅在必要的情况下提供对 VM 的受控访问，降低了遭受网络容量耗尽攻击的风险。

在为 VM 启用 JIT 时，你将创建一个策略，该策略用于确定要保护的端口、端口保持打开的时长，以及可以从其访问这些端口的经批准的 IP 地址。 该策略帮助你控制用户在请求访问时可执行的操作。

请求将记录在 Azure 活动日志中，因此你可以轻松监视和审核访问。 实时页面还帮助你快速识别已启用了 JIT 的现有 VM 和建议启用 JIT 的 VM。

[详细了解 Azure 防火墙](https://docs.azure.cn/firewall/overview)。


### <a name="single-click-remediation-to-boost-your-security-posture-preview"></a>单击即可提升安全状况的修正（预览版）

安全评分是一个可帮助你评估工作负荷安全状况的工具。 它会评审你的安全建议并确定其优先级，让你知道首先执行哪些建议。 这可帮助你找到最严重的安全漏洞，以确定调查优先级。

为了简化对安全错误配置的修正并帮助你快速提高安全评分，我们添加了一项新功能，允许你通过一次单击执行对大量资源的修正建议。

此操作允许你选择要对其应用修正的资源，并启动一个将代表你对设置进行配置的修正操作。

请在[安全建议参考指南](recommendations-reference.md)中查看哪些建议启用了快速修复。




## <a name="july-2019"></a>2019 年 7 月

### <a name="updates-to-network-recommendations"></a>网络建议更新

Azure 安全中心 (ASC) 已推出了新的网络建议，并改进了一些现有的网络建议。 现在，使用安全中心可以确保为资源提供更好的网络保护。 

[详细了解网络建议](recommendations-reference.md#recs-network)。


## <a name="june-2019"></a>2019 年 6 月

### <a name="adaptive-network-hardening---generally-available"></a>自适应网络强化 - 正式发布

在公有云中运行的工作负荷的最大受攻击面之一是与公共 Internet 的连接。 我们的客户发现很难知道应该实施哪些网络安全组 (NSG) 规则来确保 Azure工作负荷仅可在必需的源范围内使用。 利用此功能，安全中心可了解 Azure 工作负荷的网络流量和连接模式，并为面向 Internet 的虚拟机提供 NSG 规则建议。 这可以帮助我们的客户更好地配置其网络访问策略，并降低他们遭受攻击的可能性。 

[详细了解自适应网络强化](security-center-adaptive-network-hardening.md)。
