---
title: Azure 安全中心的威胁防护
description: 本主题介绍由 Azure 安全中心的威胁防护功能保护的资源
services: security-center
documentationcenter: na
author: memildin
manager: rkarlin
ms.assetid: 33c45447-3181-4b75-aa8e-c517e76cd50d
ms.service: security-center
ms.topic: conceptual
ms.date: 05/12/2020
ms.author: v-tawe
origin.date: 03/15/2020
ms.openlocfilehash: 99adbe8ecbfe5cc0f3ecba10ce674aa8ffea1cda
ms.sourcegitcommit: 134afb420381acd8d6ae56b0eea367e376bae3ef
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/15/2020
ms.locfileid: "83423071"
---
# <a name="threat-protection-in-azure-security-center"></a>Azure 安全中心的威胁防护

安全中心检测到环境中任何区域出现威胁时，它会生成警报。 这些警报描述了受影响资源的详细信息、建议的修正步骤，以及在某些情况下用于触发逻辑应用响应的选项。

Azure 安全中心的威胁防护为你的环境提供了全面的防御：

* **Azure 计算资源的威胁防护**：Windows 计算机、Linux 计算机、Azure 应用服务和 Azure 容器

* **Azure 数据资源的威胁防护**：SQL 数据库和 SQL 数据仓库、Azure 存储和 Azure Cosmos DB

* **Azure 服务层的威胁防护**：Azure 网络层、Azure 管理层（Azure 资源管理器）（预览版）和 Azure Key Vault（预览版）

<!-- not available-->




## <a name="threat-protection-for-windows-machines"></a>Windows 计算机的威胁防护 <a name="windows-machines"></a>

Azure 安全中心与 Azure 服务集成，以监视和保护基于 Windows 的计算机。 安全中心以一种易于使用的格式显示所有这些服务的警报和修正建议。

* **Microsoft Defender ATP** <a name="windows-atp"></a> - 安全中心通过与 Microsoft Defender 高级威胁防护 (ATP) 集成来扩展其云工作负载保护平台。 它们共同提供了全面的终结点检测和响应 (EDR) 功能。

    > [!IMPORTANT]
    > 使用安全中心的 Windows 服务器会自动启用 Microsoft Defender ATP 传感器。

    Microsoft Defender ATP 检测到威胁时，它会触发警报。 警报显示在“安全中心”仪表板上。 从仪表板，你可以转到 Microsoft Defender ATP 控制台并执行详细调查以发现攻击范围。 有关 Microsoft Defender ATP 的详细信息，请参阅[将服务器载入到 Microsoft Defender ATP 服务](https://docs.microsoft.com/windows/security/threat-protection/microsoft-defender-atp/configure-server-endpoints)。

* **故障转储分析** <a name="windows-dump"></a> - 当软件出现故障时，故障转储可捕获发生故障时的部分内存。

    故障可能是由恶意软件造成的，或可能包含恶意软件。 为避免被安全产品检测到，各种形式的恶意软件都会使用无文件攻击，这可避免写入磁盘或对写入磁盘的软件组件进行加密。 使用传统的基于磁盘的方法很难检测到这种类型的攻击。

    但是，通过使用内存分析，可以检测到这种攻击。 通过分析故障转储中的内存，安全中心可以检测到攻击所使用的技术。 例如，攻击可能试图利用软件中的漏洞，访问机密数据以及隐秘地持久保存在受到攻击的计算机中。 安全中心可以在对主机性能产生最小影响的情况下执行此操作。

    有关故障转储分析警报的详细信息，请参阅[警报引用表](alerts-reference.md#alerts-windows)。

* **无文件攻击检测** <a name="windows-fileless"></a> - 针对终结点的无文件攻击很常见。 为了避免检测，无文件攻击会将恶意有效负载注入内存中。 攻击者的有效负载会持久保存在受影响进程的内存中，并执行各种恶意活动。

    借助无文件攻击检测，自动内存取证技术可以识别无文件攻击工具包、技术和行为。 该解决方案会在运行时定期扫描计算机，并直接从关键安全类型或成员进程的内存中提取见解。

    它可找到利用、代码注入以及执行恶意有效负载的证据。 无文件攻击检测会生成详细的安全警报，以加速警报会审、关联和下游响应时间。 此方法补充了基于事件的 EDR 解决方案，可提供更大的检测范围。

    有关无文件攻击检测警报的详细信息，请参阅[警报引用表](alerts-reference.md#alerts-windows)。

> [!TIP]
> 可以通过下载 [Azure 安全中心 Playbook：安全警报](https://gallery.technet.microsoft.com/Azure-Security-Center-f621a046)来模拟 Windows 警报。






## <a name="threat-protection-for-linux-machines"></a>Linux 计算机的威胁防护 <a name="linux-machines"></a>

安全中心通过使用 auditd（最常见的 Linux 审核框架之一）从 Linux 计算机收集审核记录。 auditd 位于主线内核中。 

* **Linux auditd 警报与 Microsoft Monitoring Agent (MMA) 集成** <a name="linux-auditd"></a> - auditd 系统包含内核级别子系统，该子系统负责监视系统调用。 它按指定的规则集筛选调用，并将其消息写入套接字。 安全中心集成了 Microsoft Monitoring Agent (MMA) 中 auditd 包中的功能。 通过这种集成，无需任何先决条件即可在所有受支持的 Linux 发行版中收集 auditd 事件。  

    通过使用 Linux MMA 代理，可收集和扩充 auditd 记录，并将其聚合到事件中。 安全中心会持续添加使用 Linux 信号检测云和本地 Linux 计算机上的恶意行为的新分析。 与 Windows 功能类似，这些分析涵盖可疑进程、可疑登录尝试、内核模块加载以及其他活动。 这些活动可能表明计算机正在受到攻击或已被破坏。  

    有关 Linux 警报的列表，请参阅[警报引用表](alerts-reference.md#alerts-linux)。

> [!TIP]
> 可以通过下载 [Azure 安全中心 Playbook：Linux 检测](https://gallery.technet.microsoft.com/Azure-Security-Center-0ac8a5ef)来模拟 Linux 警报。





## <a name="threat-protection-for-azure-app-service"></a>Azure 应用服务的威胁防护 <a name="app-services"></a>

> [!NOTE]
> 该服务当前在 Azure 政府和主权云区域中不可用。

安全中心使用云规模来识别针对应用服务上运行的应用程序的攻击。 攻击者通过探测 Web 应用程序来找出并利用弱点。 在路由到特定环境之前，对在 Azure 中运行的应用程序的请求会经历多个网关，并在其中进行检查和记录。 然后，此数据将用于识别漏洞和攻击者，以及用于学习以后将使用的新模式。

通过使用 Azure 作为云提供商提供的可见性，安全中心可分析应用服务内部日志，以识别针对多个目标的攻击方法。 例如，方法包含广泛的扫描和分布式攻击。 这种类型的攻击通常来自 IP 的一个很小的子集，并且显示了爬网到多个主机上的类似终结点的模式。 攻击会搜索易受攻击的页面或插件，且无法从单个主机的角度进行识别。

如果运行的是基于 Windows 的应用服务计划，则安全中心还具有对基础沙盒和 VM 的访问权限。 连同上面提到的日志数据一起，基础结构可以说明问题，从在野外环境中持续不断的新攻击到在客户计算机中的入侵。 因此，即使安全中心是在 Web 应用已被利用后才进行部署的，它也能检测到持续不断的攻击。

有关 Azure 应用服务警报的列表，请参阅[警报引用表](alerts-reference.md#alerts-azureappserv)。






## <a name="threat-protection-for-azure-containers"></a>Azure 容器的威胁防护 <a name="azure-containers"></a>

> [!NOTE]
> 该服务当前在 Azure 政府和主权云区域中不可用。

安全中心为容器化环境提供实时威胁防护，并针对可疑活动生成警报。 可以使用此信息快速补救安全问题，并提高容器的安全性。

安全中心提供不同级别的威胁防护： 

* **主机级别** - 安全中心的代理（在标准层上提供，有关详细信息，请参阅[定价](security-center-pricing.md)）监视 Linux 中的可疑活动。 对于源自节点或在其上运行的容器的可疑活动，代理会触发警报。 此类活动的示例包括 Web shell 检测以及与已知可疑 IP 地址的连接。

    为了更深入地了解容器化环境的安全性，代理会监视特定于容器的分析。 它将针对诸如特权容器创建、对 API 服务器的可疑访问以及在 Docker 容器中运行的安全外壳 (SSH) 服务器等事件触发警报。

    >[!IMPORTANT]
    > 如果你选择不在主机上安装代理，则只会收到威胁防护权益和安全警报的子集。 你仍将收到与网络分析以及与恶意服务器进行通信相关的警报。

    有关主机级别警报的列表，请参阅[警报引用表](alerts-reference.md#alerts-containerhost)。


* 在 AKS 群集级别，威胁防护基于对 Kubernetes 审核日志的分析。 若要启用此无代理监视，请在“定价和设置”页上将 Kubernetes 选项添加到订阅中（请参阅[定价](security-center-pricing.md)） 。 为了在此级别生成警报，安全中心会使用 AKS 检索到的日志来监视由 AKS 管理的服务。 此级别事件示例包括公开 Kubernetes 仪表板、创建高特权角色以及创建敏感装载。

    >[!NOTE]
    > 安全中心为在订阅设置上启用 Kubernetes 选项后发生的 Azure Kubernetes 服务操作和部署生成安全警报。 

    有关 AKS 群集级别警报的列表，请参阅[警报引用表](alerts-reference.md#alerts-akscluster)。

此外，我们的全球安全研究人员团队会持续监视威胁趋势。 他们会在发现漏洞时添加特定于容器的警报。

> [!TIP]
> 可以按照[此博客文章](https://techcommunity.microsoft.com/t5/azure-security-center/how-to-demonstrate-the-new-containers-features-in-azure-security/ba-p/1011270)中的说明来模拟容器警报。








## <a name="threat-protection-for-sql-database-and-sql-data-warehouse"></a>SQL 数据库和 SQL 数据仓库的威胁防护 <a name="data-sql"></a>

Azure SQL 数据库的高级威胁防护可检测异常活动，指出有人在访问或利用数据库时的异常行为和可能有害的尝试。

如果出现可疑数据库活动、潜在漏洞或 SQL 注入攻击，以及异常数据库访问和查询模式，你将看到警报。

Azure SQL 数据库和 SQL 的高级威胁防护是适用于高级 SQL 安全功能的[高级数据安全 (ADS)](https://docs.azure.cn/sql-database/sql-database-advanced-data-security) 统一包的一部分，涵盖 Azure SQL 数据库、Azure SQL 数据库托管实例、Azure SQL 数据仓库数据库和 Azure 虚拟机上的 SQL 服务器。

有关详细信息，请参阅：

* [如何为 Azure SQL 数据库启用高级威胁防护](https://docs.azure.cn/sql-database/sql-database-threat-detection-overview)
* [如何为 Azure 虚拟机上的 SQL 服务器启用高级威胁防护](security-center-iaas-advanced-data.md)
* [SQL 数据库和 SQL 数据仓库的威胁防护警报列表](alerts-reference.md#alerts-sql-db-and-warehouse)




## <a name="threat-protection-for-azure-storage"></a>Azure 存储的威胁防护 <a name="azure-storage"></a>


适用于存储的高级威胁防护（当前仅 Blob 存储可用）会检测试图访问或利用存储帐户的异常或可能有害的企图。 这一层保护使你无需成为安全专家便可以解决威胁，并可帮助你管理安全监视系统。

有关详细信息，请参阅：

<!-- not available-->
* [Azure 存储的威胁防护警报列表](alerts-reference.md#alerts-azurestorage)


> [!TIP]
> 可以按照[此博客文章](https://techcommunity.microsoft.com/t5/azure-security-center/validating-atp-for-azure-storage-detections-in-azure-security/ba-p/1068131)中的说明来模拟 Azure 存储警报。




## <a name="threat-protection-for-azure-cosmos-db"></a>Azure Cosmos DB 威胁防护 <a name="cosmos-db"></a>

Azure Cosmos DB 警报是因访问或攻击 Azure Cosmos DB 帐户的异常且可能有害的尝试而生成的。

有关详细信息，请参阅：

<!-- not available-->
* [Azure Cosmos DB 的威胁防护警报列表（预览）](alerts-reference.md#alerts-azurecosmos)




## <a name="threat-protection-for-azure-network-layer"></a>Azure 网络层的威胁防护 <a name="network-layer"></a>


某些网络配置可能会限制安全中心生成有关可疑网络活动的警报。 若要使安全中心生成网络警报，请确保：

- 虚拟机具有一个公共 IP 地址（或位于具有公共 IP 地址的负载均衡器上）。

- 外部 IDS 解决方案不会阻止虚拟机的网络出口流量。

- 在可疑通信发生的整个时段内，已为虚拟机分配了相同的 IP 地址。 这也适用于作为托管服务的一部分而创建的 VM（例如 AKS、Databricks）。

有关 Azure 网络层警报的列表，请参阅[警报引用表](alerts-reference.md#alerts-azurenetlayer)。




## <a name="threat-protection-for-azure-management-layer-azure-resource-manager-preview"></a>Azure 管理层（Azure 资源管理器）的威胁防护（预览）<a name ="management-layer"></a>

基于 Azure 资源管理器的安全中心保护层当前处于预览阶段。

安全中心通过使用 Azure 资源管理器事件（其被视为 Azure 的控制平面）来提供额外一层保护。 通过分析 Azure 资源管理器记录，安全中心可检测 Azure 订阅环境中的异常或可能有害的操作。

有关 Azure 资源管理器（预览版）警报的列表，请参阅[警报引用表](alerts-reference.md#alerts-azureresourceman)。



>[!NOTE]
> 先前的分析中有部分由 Microsoft Cloud App Security 提供支持。 若要从这些分析中获益，必须激活 Cloud App Security 许可证。 如果你具有 Cloud App Security 许可证，则默认情况下将启用这些警报。 禁用警报的方法如下：
>
> 1. 在“安全中心”边栏选项卡中，选择“安全策略” 。 对于要更改的订阅，请选择“编辑设置”。
> 2. 选择“威胁检测”。
> 3. 在“启用集成”下，清除“允许 Microsoft Cloud App Security 访问我的数据”，然后选择“保存”  。

>[!NOTE]
>安全中心会将与安全相关的客户数据存储在与其资源相同的地区中。启用 Cloud App Security 后，此信息将根据 Cloud App Security 的地理位置规则进行存储。 有关详细信息，请参阅[非区域性服务的数据存储](https://azuredatacentermap.azurewebsites.net/)。








## <a name="threat-protection-for-azure-key-vault-preview"></a>Azure Key Vault 的威胁防护（预览版）<a name="azure-keyvault"></a>

> [!NOTE]
> 该服务当前在 Azure 政府和主权云区域中不可用。

Azure 密钥保管库是一种云服务，用于保护加密密钥和机密（例如证书、连接字符串和密码）。 

Azure 安全中心包含适用于 Azure Key Vault 的 Azure 本机高级威胁防护，提供额外的安全情报层。 安全中心可检测在访问或利用 Key Vault 帐户时的异常行为和可能有害的尝试。 这一层保护使你无需成为安全专家也无需管理第三方安全监视系统便可以解决威胁。  

当发生异常活动时，安全中心会显示警报，并有选择地通过电子邮件将其发送给订阅管理员。 这些警报包含可疑活动的详细信息以及有关如何进行调查和修正威胁的建议。 

有关 Azure Key Vault 警报的列表，请参阅[警报引用表](alerts-reference.md#alerts-azurekv)。





## <a name="threat-protection-for-other-microsoft-services"></a>其他 Microsoft 服务的威胁防护 <a name="alerts-other"></a>

### <a name="threat-protection-for-azure-waf"></a>Azure WAF 的威胁防护 <a name="azure-waf"></a>

Azure 应用程序网关提供的 Web 应用程序防火墙 (WAF) 可以对 Web 应用程序进行集中保护，避免其受到常见的攻击和漏洞伤害。

Web 应用程序正逐渐成为利用常见已知漏洞的恶意攻击的目标。 应用程序网关 WAF 基于开放 Web 应用程序安全项目中的核心规则集 3.0 或 2.2.9。 WAF 会自动更新，以防止出现新的漏洞。 

如果你具有 Azure WAF 的许可证，则无需进行其他配置即可将 WAF 警报流式传输到安全中心。 有关 WAF 生成的警报的详细信息，请参阅 [Web 应用程序防火墙 CRS 规则组和规则](../web-application-firewall/ag/application-gateway-crs-rulegroups-rules.md?tabs=owasp31#crs911-31)。


<!-- DDoS Protection is not available -->


## <a name="next-steps"></a>后续步骤
若要从这些威胁防护功能中了解有关安全警报的详细信息，请参阅以下文章：

* [所有 Azure 安全中心警报的引用表](alerts-reference.md)
* [Azure 安全中心中的安全警报](security-center-alerts-overview.md)
* [在 Azure 安全中心内管理和响应安全警报](security-center-managing-and-responding-alerts.md)
