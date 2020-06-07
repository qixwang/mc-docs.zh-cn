---
title: Azure 安全中心发行说明
description: 介绍了 Azure 安全中心的新增功能和更改内容。
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
ms.openlocfilehash: 7788d14b9f7b72fcc69266611ee01899f326588a
ms.sourcegitcommit: cbaa1aef101f67bd094f6ad0b4be274bbc2d2537
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/28/2020
ms.locfileid: "84126797"
---
# <a name="whats-new-in-azure-security-center"></a>Azure 安全中心的新增功能有哪些？

Azure 安全措施正处于积极开发阶段，并不断地获得改进。 为了让大家随时了解最新的开发成果，本页将提供以下方面的信息：

- 新增功能
- Bug 修复
- 已弃用的功能

本页面会定期更新，请不时回来查看。 如果要查找早于 6 个月的项，可以在 [Azure 安全中心新增功能存档](release-notes-archive.md)中找到它们。


## <a name="may-2020"></a>2020 年 5 月

### <a name="changes-to-just-in-time-jit-virtual-machine-vm-access"></a>对实时 (JIT) 虚拟机 (VM) 访问的更改

安全中心包含一项可选功能，用于保护 VM 的管理端口。 这可针对最常见的暴力攻击形式提供防护。

此更新对此功能进行了以下更改：

- 建议你在 VM 上启用 JIT 的建议已重命名。 以前的“应在虚拟机上应用实时网络访问控制”现在是：“应通过实时网络访问控制来保护虚拟机的管理端口”。

- 此建议已设置为仅当存在打开的管理端口时才触发。

[详细了解 JIT 访问功能](security-center-just-in-time.md)。


### <a name="custom-recommendations-have-been-moved-to-a-separate-security-control"></a>自定义建议已移到单独的安全控件中

随增强的安全评分功能引入的安全控件之一是“实现安全最佳做法”。 为你的订阅创建的任何自定义建议都会自动放置在该控件中。 

为了更轻松地查找自定义建议，我们已将它们移到专用安全控件“自定义建议”中。 此控件不会对安全评分产生任何影响。

若要详细了解安全控件，请参阅 [Azure 安全中心内的增强安全评分（预览版）](secure-score-security-controls.md)。


### <a name="toggle-added-to-view-recommendations-in-controls-or-as-a-flat-list"></a>添加了查看建议时用于在控件视图与平面列表视图之间进行切换的一个开关

安全控件是相关安全建议的逻辑组。 它们反映了你的受攻击面。 控件是一组安全建议，其中包含有助于实现这些建议的说明。

若要立即查看你的组织对每个单独攻击面的保护力度，请查看每个安全控件的分数。

默认情况下，你的建议显示在安全控件中。 从此更新起，还可以将它们显示为列表。 若要将它们显示为按受影响资源的运行状况排序的简单列表，请使用新开关“按控件分组”。 此开关在门户中位于列表上方。

安全控件（和此开关）是新的安全评分体验的一部分。 请记得从门户中向我们发送你的反馈。

若要详细了解安全控件，请参阅 [Azure 安全中心内的增强安全评分（预览版）](secure-score-security-controls.md)。


### <a name="account-security-recommendations-moved-to-security-best-practices-security-control"></a>帐户安全建议已移到“安全最佳做法”安全控件中

随增强的安全评分功能引入的安全控件之一是“安全最佳做法”。 如果某个建议位于此控件中，则它不会影响安全评分。 

通过此更新，三个建议移出了它们最初所在的控件，移到了此最佳做法控件中。 我们执行此步骤是因为我们已确定这三个建议的风险比最初想象的要低。

这三个建议是：

- 应在对订阅拥有读取权限的帐户上启用 MFA（最初在“启用 MFA”控件中）
- 应从订阅中删除拥有读取权限的外部帐户（最初在“管理访问和权限”控件中）
- 最多为订阅指定 3 个所有者（最初在“管理访问和权限”控件中）

若要详细了解安全控件，请参阅 [Azure 安全中心内的增强安全评分（预览版）](secure-score-security-controls.md)。


### <a name="custom-policies-with-custom-metadata-generally-available"></a>采用自定义元数据的自定义策略已正式发布

自定义策略现在是安全中心建议体验、安全评分和合规性标准仪表板的一部分。 此功能现已正式发布，可让你在安全中心扩展组织的安全评估覆盖范围。 

在 Azure 策略中创建自定义计划，向其添加策略并将其加入到 Azure 安全中心，然后将其可视化为建议。

现在，我们还添加了用于编辑自定义建议元数据的选项。 元数据选项包括严重性、修正步骤、威胁信息，等等。  

[详细了解如何利用详细信息增强你的自定义建议](custom-security-policies.md#enhancing-your-custom-recommendations-with-detailed-information)。


## <a name="april-2020"></a>2020 年 4 月

### <a name="dynamic-compliance-packages-now-generally-available"></a>动态合规性包现已正式发布

Azure 安全中心合规性仪表板现在包括了**动态合规性包**（现已正式发布），用于跟踪更多行业和法规标准。

可以通过安全中心安全策略页将动态合规性包添加到订阅或管理组。 如果你已加入某个标准或基准，则该标准会显示在你的合规性仪表板中，所有关联的合规性数据都会映射为评估。 已加入的所有标准的汇总报表将可供下载。

现在，你可以添加如下标准：

- **NIST SP 800-53 R4**
- **SWIFT CSP CSCF-v2020**
- **UK OFFICIAL 和 UK NHS**
- **加拿大联邦 PBMM**
- **Azure CIS 1.1.0（新）** （这是 Azure CIS 1.1.0 的更完整表示形式）

此外，我们最近添加了 **Azure 安全基准**，它是由 Microsoft 基于公用合规性框架编写的特定于 Azure 的安全性和合规性最佳做法准则。 当有更多标准可用时，仪表板会支持它们。  
 
[详细了解如何在合规性仪表板中自定义标准集](update-regulatory-compliance-packages.md)。


### <a name="identity-recommendations-now-included-in-azure-security-center-free-tier"></a>标识建议现在已包括在 Azure 安全中心免费层中

Azure 安全中心免费层上针对标识和访问的安全建议现已正式发布。 这是为了使云安全状况管理 (CSPM) 功能免费而做的工作的一部分。 到目前为止，这些建议仅适用于标准定价层。

标识和访问建议的示例包括：

- “应在对订阅拥有所有者权限的帐户上启用多重身份验证。”
- “最多只为订阅指定三个所有者。”
- “应从订阅中删除弃用的帐户。”

如果你在免费定价层上拥有订阅，则其安全评分将受此更改影响，因为之前从未对其标识和访问安全性进行评估。

[详细了解标识和访问建议](recommendations-reference.md#recs-identity)。
[详细了解如何监视标识和访问](security-center-identity-access.md)。


## <a name="march-2020"></a>2020 年 3 月

<!--Workflow automation is not available-->



### <a name="integration-of-azure-security-center-with-windows-admin-center"></a>Azure 安全中心与 Windows 管理中心的集成

现在可以将本地 Windows 服务器从 Windows 管理中心直接移到 Azure 安全中心。 然后，安全中心将成为你用于查看所有 Windows 管理中心资源（包括本地服务器、虚拟机和其他 PaaS 工作负荷）的安全信息的单一窗格。

将服务器从 Windows 管理中心移动到 Azure 安全中心后，你可以：

- 在 Windows 管理中心的安全中心扩展中查看安全警报和建议。
- 通过 Azure 门户的安全中心（或通过 API）查看安全状况，并检索 Windows 管理中心托管的服务器的其他详细信息。

详细了解如何[将 Azure 安全中心与 Windows 管理中心集成](windows-admin-center-integration.md)。


### <a name="protection-for-azure-kubernetes-service"></a>对 Azure Kubernetes 服务的保护

Azure 安全中心正在扩展其容器安全功能以保护 Azure Kubernetes 服务 (AKS)。

流行的开源平台 Kubernetes 已被广泛采用，它现在是容器业务流程的行业标准。 尽管已经达到了这种程度的广泛实现，但是公众仍然不怎么了解如何保护 Kubernetes 环境。 增强容器化应用程序受攻击面的防御需要专业知识，以确保安全地配置基础结构，并对其进行持续监视以检测潜在的威胁。

安全中心防御包括：

- **发现和可见性** - 在已注册到安全中心的订阅中持续发现托管的 AKS 实例。
- **安全建议** - 这是一些可操作的建议，让你能够满足 AKS 的最佳安全做法。 这些建议包括在安全评分中，这样是为了确保用户在查看组织的安全状况时会查看它们。 你可能会看到的一个 AKS 相关建议示例是“应使用基于角色的访问控制来限制对 Kubernetes 服务群集的访问”。
- **威胁防护** - 通过持续分析 AKS 部署，安全中心会提醒你注意在主机和 AKS 群集级别检测到的威胁和恶意活动。

[详细了解 Azure Kubernetes 服务与安全中心的集成](azure-kubernetes-service-integration.md)。
[详细了解安全中心的容器安全功能](container-security.md)。


### <a name="improved-just-in-time-experience"></a>改进了实时体验

Azure 安全中心提供的用于保护管理端口的实时工具的功能、操作和 UI 已增强，如下所述： 

- **理由字段** - 通过 Azure 门户的实时页面请求访问虚拟机 (VM) 时，可以使用一个新的可选字段来输入请求理由。 可以在活动日志中跟踪输入到此字段的信息。 
- **自动清除冗余的实时 (JIT) 规则** - 每当你更新 JIT 策略时，系统会自动运行一个清理工具来检查整个规则集的有效性。 该工具查找策略中的规则与 NSG 中的规则之间的不匹配项。 如果清理工具发现不匹配项，它会确定原因，并寻找安全的时机来删除不再需要的内置规则。 清理工具从不会删除你创建的规则。 

[详细了解 JIT 访问功能](security-center-just-in-time.md)。


### <a name="two-security-recommendations-for-web-applications-deprecated"></a>弃用了针对 Web 应用程序的两个安全建议

与 Web 应用程序相关的两个安全建议将被弃用： 

- 应加强 IaaS NSG 上 Web 应用程序的规则。
    （相关策略：应该强化 IaaS 上 Web 应用程序的 NSG 规则）

- 应限制对应用服务的访问。
    （相关策略：应限制对应用服务的访问[预览版]）

这些建议将不再出现在安全中心建议列表中。 相关策略将不再包含在名为“安全中心默认值”的计划中。

[详细了解安全建议](recommendations-reference.md)。

## <a name="february-2020"></a>2020 年 2 月

### <a name="fileless-attack-detection-for-linux-is-now-in-preview"></a>适用于 Linux 的无文件攻击检测目前为预览版

由于使用更隐蔽的方法来避免检测的攻击者越来越多，因此，除了 Windows 外，Azure 安全中心还扩展了 Linux 的无文件攻击检测。 无文件攻击利用软件漏洞，将恶意有效负载注入到良性系统进程中，并隐藏在内存中。 这些方法可以：

- 最大程度地减少或消除了磁盘上的恶意软件跟踪
- 大大降低被基于磁盘的恶意软件扫描解决方案检测到的机率

为了应对这种威胁，Azure 安全中心在 2018 年 10 月发布了适用于 Windows 的无文件攻击检测，现在又扩展了 Linux 上的无文件攻击检测。 


## <a name="january-2020"></a>2020 年 1 月

### <a name="enhanced-secure-score"></a>增强了安全评分功能

Azure 安全中心的安全评分功能的一个增强版本现在以预览版形式提供。 在此版本中，多个建议被分组到了安全控制中，以便更好地反映受攻击面（例如，限制对管理端口的访问）。

请在预览版阶段自行熟悉安全评分更改，并确定可帮助你进一步保护环境的其他修正。

在 [Azure 安全中心的强化安全评分（预览版）](secure-score-security-controls.md)中了解更多信息。