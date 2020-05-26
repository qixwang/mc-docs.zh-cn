---
title: Azure 安全中心中的安全分数
description: Azure 安全中心的安全分数及其安全控件的说明
services: security-center
documentationcenter: na
author: memildin
manager: rkarlin
ms.assetd: c42d02e4-201d-4a95-8527-253af903a5c6
ms.service: security-center
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/14/2020
ms.author: v-tawe
origin.date: 03/10/2020
ms.openlocfilehash: 35d22217318927a4a464302eb8fa59c626fa2eb1
ms.sourcegitcommit: 134afb420381acd8d6ae56b0eea367e376bae3ef
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/15/2020
ms.locfileid: "83422965"
---
# <a name="enhanced-secure-score-preview-in-azure-security-center"></a>Azure 安全中心中的增强安全分数（预览版）

## <a name="introduction-to-secure-score"></a>安全分数简介

Azure 安全中心有两个主要目标：帮助你了解当前的安全情况，以及帮助你有效地提高安全性。 使你能够实现这些目标的安全中心的核心方面是安全分数。

安全中心会持续评估你的资源、订阅和组织的安全问题。 然后，它将所有调查结果汇总到一个分数中，以便你可以一目了然地了解当前的安全状况：分数越高，识别出的风险级别就越低。 使用分数跟踪组织中的安全工作和项目。 

安全中心的“安全分数”页包括：

- **分数** - 安全分数显示为百分比值，原值也一目了然：

    [![安全分数显示为百分比值，原值也一目了然](media/secure-score-security-controls/secure-score-with-percentage.png)](media/secure-score-security-controls/secure-score-with-percentage.png#lightbox)

- **安全控件** - 每个控件都是相关安全建议的逻辑组，反映易受攻击的攻击面。 控件是一组安全建议，其中包含有助于实现这些建议的说明。 仅当控件内针对单个资源的所有建议均得到实现时，分数才会提高。

    若要立即查看你的组织对每个单独攻击面的保护力度，请查看每个安全控件的分数。

    有关详细信息，请参阅下面的[如何计算安全分数](secure-score-security-controls.md#how-the-secure-score-is-calculated)。 


>[!TIP]
> 早期版本的安全中心在建议级别授予分数：实现单个资源的建议后就会提高安全分数。 目前，仅当控件内针对单个资源的所有建议均得到实现后，分数才会提高。 因此，仅当资源的安全性提高时，分数才会提高。
> 尽管此增强版本仍处于预览阶段，但可以从 Azure 门户中获得早期的安全分数体验。 


## <a name="locating-your-secure-score"></a>查找安全分数

安全中心会将你的分数显示在显著位置：是“概述”页面中显示的第一项内容。 如果单击浏览到专用安全分数页，将看到按订阅细分的分数。 单击单个订阅可查看重要建议的详细列表，以及实现这些建议将对订阅分数产生的潜在影响。

## <a name="how-the-secure-score-is-calculated"></a>如何计算安全分数 

建议页上清楚地显示了每个安全控件对总体安全分数的贡献。

[![增强的安全分数模式引入了安全控件](media/secure-score-security-controls/security-controls.png)](media/secure-score-security-controls/security-controls.png#lightbox)

要获取安全控件的所有可能分数，必须符合安全控件中针对所有资源的所有安全建议的要求。 例如，安全中心提供有关如何保护管理端口的多个建议。 过去，可以只实现其中一些相关和相互依赖的建议而不实现其他建议，你的安全分数仍可提高。 但客观来看，很容易得出结论：你的安全性并未得到改善，除非你解决了所有问题。 现在，需要将它们全部实现，才能提高安全分数。

例如，名为“应用系统更新”的安全控件的最高分数为六分，可以在关于该控件可能提高的分数（潜在增加分数）的工具提示中看到这个分数：

[![安全控件“应用系统更新”](media/secure-score-security-controls/apply-system-updates-control.png)](media/secure-score-security-controls/apply-system-updates-control.png#lightbox)

上面的屏幕截图中的安全控件“应用系统更新”的潜在增加分数为“2％（1 分）”。 这意味着，如果实现此控件中的所有建议，分数将增加 2%（在本例中为一分）。 为简单起见，建议列表的“潜在增加分数”列中的值均四舍五入为整数。 工具提示显示精确值：

* **最高分数** - 通过实现控件中的所有建议可以获得的最高分数。 控件的最高分数可以反映该控件的相对重要性。 使用最高分值进行会审，确定要首先解决的问题。 
* **潜在增加分数** - 控件可提升的剩余分值。 若要让安全分数增加这些分值，请实现控件中的所有建议。 在以上示例中，控件的一分实际上是 0.96 分。
* **当前分数** - 此控件的当前分数。 每个控件都对总分有贡献。 在此示例中，控件为总分贡献了 5.04 分。 

### <a name="calculations---understanding-your-score"></a>计算 - 了解分数

|指标|公式和示例|
|-|-|
|**安全控件的当前分数**|<br>![用于计算安全控件当前分数的公式](media/secure-score-security-controls/security-control-scoring-equation.png)<br><br>每个单独的安全控件都会影响安全分数。 控件中的建议所影响的每个资源都对控件的当前分数有贡献。 每个控件的当前分数是控件内资源状态的度量指标。<br>![显示计算安全控件当前分数时使用的值的工具提示](media/secure-score-security-controls/security-control-scoring-tooltips.png)<br>在此示例中，最大分数 6 将除以 78，因为后者是正常和不正常资源的总和。<br>6 / 78 = 0.0769<br>将其与正常资源的数量 (4) 相乘可得出当前分数：<br>0.0769 * 4 = 0.31<br><br>|
|**安全评分**<br>一个订阅|<br>![用于计算当前安全分数的公式](media/secure-score-security-controls/secure-score-equation.png)<br><br>![已启用所有控件的单个订阅安全分数](media/secure-score-security-controls/secure-score-example-single-sub.png)<br>在此示例中有一个订阅，其中包含所有可用的安全控件（可能的最高分数为 60 分）。 分数显示了可能的最高分数 60 分中的 28 分，其余 32 分通过安全控件的“潜在分数增加”数字得到反映。<br>![控件和潜在分数增加值列表](media/secure-score-security-controls/secure-score-example-single-sub-recs.png)|
|**安全评分**<br>多个订阅|<br>对所有订阅中所有资源的当前分数进行加总，计算方法与单个订阅的计算方法相同<br><br>查看多个订阅时，安全分数会评估所有启用的策略中的所有资源，并将所有资源对每个安全控件最高分数的总影响分组显示。<br>![启用了所有控件的多个订阅的安全分数](media/secure-score-security-controls/secure-score-example-multiple-subs.png)<br>综合分数不是平均值，而是对所有订阅中所有资源状态进行计算后得到的值。<br>并且，如果你转到推荐页，把潜在的可用分数加起来，你会发现这是当前分数 (24) 和最大可用分数 (60) 的差值。|
||||

## <a name="improving-your-secure-score"></a>提高安全分数

若要提高安全分数，请实现建议列表中的安全建议。 可以手动实现针对每个资源的每个建议，也可以使用快速修复！ 选项（如果可用）将某个建议的实现快速应用于一组资源。 有关详细信息，请参阅[实现建议](security-center-remediate-recommendations.md)。

>[!IMPORTANT]
> 仅内置建议会影响安全分数。

## <a name="security-controls-and-their-recommendations"></a>安全控件及其建议

下表列出了 Azure 安全中心的安全控件。 对于每个控件，如果你实现了控件中列出的针对所有资源的所有建议，可以看到安全分数可增加的最大分数 。 

> [!TIP]
> 如果要以不同的方式对此列表进行筛选或排序，请将其复制并粘贴到 Excel 中。

|安全控制|最大安全分数点|建议|
|----------------|:-------------------:|---------------|
|**启用 MFA**|10 个|- 应在对订阅拥有所有者权限的帐户上启用 MFA<br>- 应在对订阅拥有读取权限的帐户上启用 MFA<br>- 应对订阅中拥有写入权限的帐户启用 MFA|
|**安全管理端口**|8|- 应在虚拟机上应用实时网络访问控制<br>- 虚拟机应与网络安全组关联<br>- 应关闭虚拟机上的管理端口|
|**应用系统更新**|6|- 应在计算机上解决监视代理运行状况问题<br>- 应在虚拟机规模集上安装监视代理<br>- 应在计算机上安装监视代理<br>- 应为云服务角色更新 OS 版本<br>- 应在虚拟机规模集上安装系统更新<br>- 应在计算机上安装系统更新<br>- 应重启计算机来应用系统更新<br>- Kubernetes 服务应升级到不易受攻击的 Kubernetes 版本<br>- 应在虚拟机上安装监视代理|
|**修正漏洞**|6|- 应在 SQL 服务器上启用高级数据安全性<br>- 应修正 Azure 容器注册表映像中的漏洞<br>- 应修复 SQL 数据库中的漏洞<br>- 应通过漏洞评估解决方案修复漏洞<br>- 应对 SQL 托管实例启用漏洞评估<br>- 应对 SQL 服务器启用漏洞评估<br>- 应在虚拟机上安装漏洞评估解决方案|
|**启用静态加密**|4|- 应在虚拟机上应用磁盘加密<br>- 应在 SQL 数据库上启用透明数据加密<br>- 自动化帐户变量应进行加密<br>- Service Fabric 群集应将 ClusterProtectionLevel 属性设置为 EncryptAndSign<br>- 应使用自己的密钥加密 SQL 服务器的 TDE 保护器|
|**加密传输中的数据**|4|- 只能通过 HTTPS 访问 API 应用<br>- 应该只能通过 HTTPS 访问函数应用<br>- 只应启用与 Redis 缓存的安全连接<br>- 应启用安全传输到存储帐户<br>- 只能通过 HTTPS 访问 Web 应用程序|
|**管理访问和权限**|4|- 最多只为订阅指定 3 个所有者<br>- 应从订阅中删除弃用帐户（预览）<br>- 应从订阅中删除拥有所有者权限的弃用帐户（预览）<br>- 应从订阅中删除拥有所有者权限的外部帐户（预览）<br>- 应从订阅中删除拥有读取权限的外部帐户<br>- 应从订阅中删除拥有写入权限的外部帐户（预览）<br>- 应为订阅分配了多个所有者<br>- 应在 Kubernetes 服务上使用基于角色的访问控制 (RBAC)（预览）<br>- Service Fabric 群集应仅使用 Azure Active Directory 进行客户端身份验证|
|**修正安全配置**|4|- 应在 Kubernetes 服务上定义 Pod 安全策略<br>- 应该修复容器安全配置中的漏洞<br>- 应修复计算机上安全配置中的漏洞<br>- 应修复虚拟机规模集上安全配置中的漏洞<br>- 应在虚拟机上安装监视代理<br>- 应在计算机上安装监视代理<br>- 应在虚拟机规模集上安装监视代理<br>- 应在计算机上解决监视代理运行状况问题|
|**限制未经授权的网络访问**|4|- 应禁用虚拟机上的 IP 转发<br>- 应在 Kubernetes 服务上定义已授权 IP 范围（预览）<br>- （已弃用）应限制对应用程序服务的访问（预览）<br>- （已弃用）应加强 IaaS NSG 上 Web 应用程序的规则<br>- 虚拟机应与网络安全组关联<br>- CORS 不应允许所有资源访问 API 应用<br>- CORS 不应允许所有资源都能访问函数应用<br>- CORS 不应允许所有资源都能访问你的 Web 应用程序<br>- 应为 API 应用禁用远程调试<br>- 应对函数应用禁用远程调试<br>- 应禁用 Web 应用程序的远程调试<br>- 应限制具有面向 Internet 的 VM 的许可网络安全组的访问<br>- 应该强化面向 Internet 的虚拟机的网络安全组规则|
|**应用自适应应用程序控制**|3|- 应在虚拟机上启用自适应应用程序控制<br>- 应在虚拟机上安装监视代理<br>- 应在计算机上安装监视代理<br>- 应在计算机上解决监视代理运行状况问题|
|**应用数据分类**|2|- 应对 SQL 数据库中的敏感数据分类（预览）|
|**保护应用程序免受 DDoS 攻击**|2|- 应启用 DDoS 防护标准版|
|**启用终结点保护**|2|- 应在虚拟机规模集上修正终结点保护运行状况故障<br>- 应在计算机上解决终结点保护运行状况问题<br>- 应在虚拟机规模集上安装终结点保护解决方案<br>- 在虚拟机上安装终结点保护解决方案<br>- 应在计算机上解决监视代理运行状况问题<br>- 应在虚拟机规模集上安装监视代理<br>- 应在计算机上安装监视代理<br>- 应在虚拟机上安装监视代理<br>- 在计算机上安装终结点保护解决方案|
|**启用审核和日志记录**|1|- 应启用 SQL 服务器上的审核<br>- 应启用应用服务中的诊断日志<br>- 应启用 Azure Data Lake Store 中的诊断日志<br>- 应启用 Azure 流分析的诊断日志<br>- 应启用 Batch 帐户的诊断日志<br>- 应启用 Data Lake Analytics 中的诊断日志<br>- 应启用事件中心的诊断日志<br>- 应启用 IoT 中心的诊断日志<br>- 应启用 Key Vault 的诊断日志<br>- 应启用逻辑应用的诊断日志<br>- 应启用搜索服务的诊断日志<br>- 应启用服务总线中的诊断日志<br>- 应启用虚拟机规模集中的诊断日志<br>- 应在批处理帐户上配置指标警报规则<br>- SQL 审核设置中应包含配置为捕获关键活动的操作组<br>- 应将 SQL 服务器的审核保留期配置为大于 90 天。|
|**实现安全最佳实践**|0|- 应限制对具有防火墙和虚拟网络配置的存储帐户的访问<br>- 从事件中心命名空间删除 RootManageSharedAccessKey 之外的所有授权规则<br>- 应该为 SQL 服务器预配 Azure Active Directory 管理员<br>- 应针对事件中心实例定义授权规则<br>- 存储帐户应迁移到新的 Azure 资源管理器资源<br>- 应将虚拟机迁移到新的 Azure 资源管理器资源<br>- SQL 服务器的“高级数据安全性”设置应包含用于接收安全警报的电子邮件地址<br>- 应在托管实例上启用高级数据安全性<br>- 在 SQL 托管实例高级数据安全设置中，应启用所有高级威胁防护类型<br>- 应在 SQL 服务器高级数据安全设置中为管理员和订阅所有者启用电子邮件通知<br>- 应在 SQL 服务器的“高级数据安全性”设置中将“高级威胁保护类型”设置为“所有”<br>- 子网应与网络安全组关联<br>- 在 SQL Server 高级数据安全设置中，应启用所有高级威胁防护类型|
||||

## <a name="secure-score-faq"></a>安全功能分数

### <a name="why-has-my-secure-score-gone-down"></a>为什么安全评分会下降？
安全中心已切换到增强的安全分数模式（当前处于预览状态），其中包括分数计算方式的更改。 现在，必须实现针对资源的所有建议才能获得分数。 分数也改为 0 - 10 分制。

### <a name="if-i-address-only-three-out-of-four-recommendations-in-a-security-control-will-my-secure-score-change"></a>如果我仅在安全控件中实现四个建议中的三个，安全分数是否会变化？
否。 只有实现了针对单个资源的所有建议，它才会更改。 若要得到控件的最高分数，必须实现针对所有资源的所有建议。

### <a name="is-the-previous-experience-of-the-secure-score-still-available"></a>是否仍然可以使用之前的安全分数体验？ 
是的。 为了顺利完成转换过程，在一段时间内，二者都可以使用。 预计之前的模型将逐步淘汰。 

### <a name="if-a-recommendation-isnt-applicable-to-me-and-i-disable-it-in-the-policy-will-my-security-control-be-fulfilled-and-my-secure-score-updated"></a>如果某个建议不适用于我，并且我在策略中将其禁用，我的安全控件是否可以不受其影响，安全分数是否会更新？
是的。 当建议不适用于你的环境时，我们建议将其禁用。 有关如何禁用特定建议的说明，请参阅[禁用安全策略](https://docs.azure.cn/security-center/tutorial-security-policy#disable-security-policies)。

### <a name="if-a-security-control-offers-me-zero-points-towards-my-secure-score-should-i-ignore-it"></a>如果某个安全控件的安全评分潜在增加分数显示为零分，应该忽略它吗？
在某些情况下，你会看到一个控件的最大分数大于零，但影响为零。 如果修复资源后增加的分数可以忽略不计，则将其四舍五入为零。 请勿忽略这些建议，因为它们仍会带来安全改进。 唯一的例外是“其他最佳实践”控件。 实现这些建议不会提高你的分数，但会提高你的整体安全性。

## <a name="next-steps"></a>后续步骤

本文介绍了安全分数及其引入的安全控件。 有关相关材料，请参阅以下文章：

- [了解建议的不同元素](security-center-recommendations.md)
- [了解如何实现建议](security-center-remediate-recommendations.md)