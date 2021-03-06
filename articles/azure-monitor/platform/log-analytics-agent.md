---
title: Log Analytics 代理概述
description: 本主题可帮助你了解如何使用 Log Analytics 收集数据并监视托管在 Azure、本地或其他云环境中的计算机。
author: Johnnytechn
ms.subservice: logs
ms.topic: conceptual
origin.date: 12/24/2019
ms.author: v-johya
ms.date: 07/17/2020
ms.openlocfilehash: d4402c62ddc196682ce90616e71634693f3abcd8
ms.sourcegitcommit: b5794af488a336d84ee586965dabd6f45fd5ec6d
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/01/2020
ms.locfileid: "87508411"
---
# <a name="log-analytics-agent-overview"></a>Log Analytics 代理概述
Azure Log Analytics 代理用于跨任意云中的虚拟机和本地计算机进行全面的管理。 Windows 和 Linux 代理将收集的数据从不同来源发送到 Azure Monitor 中的 Log Analytics 工作区，以及监视解决方案中定义的任何唯一日志或指标。 Log Analytics 代理还支持 Azure Monitor 中的见解和其他服务，例如 [Azure 安全中心](/security-center/)和 [Azure 自动化](../../automation/automation-intro.md)。

本文提供该代理的详细概述、系统和网络要求以及不同的部署方法。

> [!NOTE]
> 你可能还会看到 Log Analytics 代理称为 Microsoft Monitoring Agent (MMA) 或 OMS Linux 代理。

> [!NOTE]
> Azure 诊断扩展是可用于从计算资源的来宾操作系统收集监视数据的代理之一。 有关不同代理的说明以及选择适合需求的代理的指南，请参阅 [Azure Monitor 代理概述](agents-overview.md)。

## <a name="comparison-to-azure-diagnostics-extension"></a>与 Azure 诊断扩展的比较
Azure Monitor 中的 [Azure 诊断扩展](diagnostics-extension-overview.md)也可用于从 Azure 虚拟机的来宾操作系统收集监视数据。 根据自己的需求，你可以选择使用任一代理或两者。 如需详细了解 Azure Monitor 代理的比较，请参阅 [Azure Monitor 代理概述](agents-overview.md)。 

需要考虑的主要区别是：

- Azure 诊断扩展只能在 Azure 中的虚拟机中使用。 Log Analytics 代理可在 Azure、其他云和本地中的虚拟机中使用。
- Azure 诊断扩展将数据发送到 Azure 存储、[Azure Monitor 指标](data-platform-metrics.md)（仅限 Windows）和事件中心。 Log Analytics 代理将数据收集到 [Azure Monitor 日志](data-platform-logs.md)中。
- [解决方案](../monitor-reference.md#insights-and-core-solutions)和其他服务（如 [Azure 安全中心](/security-center/)）需要 Log Analytics 代理。
<!-- Not available in MC: vminsights-overview.md -->

## <a name="costs"></a>成本
Log Analytics 代理不收取任何费用，但引入的数据可能产生费用。 请查看[使用 Azure Monitor 日志管理使用情况和成本](manage-cost-storage.md)，获取有关 Log Analytics 工作区中收集的数据定价的详细信息。

## <a name="data-collected"></a>收集的数据
下表列出了在配置 Log Analytics 工作区后即可从所有连接的代理收集的数据的类型。 请参阅 [Azure Monitor 监视的内容是什么？](../monitor-reference.md)，获取使用 Log Analytics 代理收集其他类型的数据的见解、解决方案和其他解决方案的列表。

| 数据源 | 说明 |
| --- | --- |
| [Windows 事件日志](data-sources-windows-events.md) | 发送到 Windows 事件日志记录系统的信息。 |
| [Syslog](data-sources-syslog.md)                     | 发送到 Linux 事件日志记录系统的信息。 |
| [“性能”](data-sources-performance-counters.md)  | 测量操作系统和工作负载不同方面性能的数值。 |
| [IIS 日志](data-sources-iis-logs.md)                 | 在来宾操作系统上运行的 IIS 网站的使用情况信息。 |
| [自定义日志](data-sources-custom-logs.md)           | Windows 和 Linux 计算机上的文本文件中的事件。 |

## <a name="data-destinations"></a>数据目标
Log Analytics 代理将数据发送到 Azure Monitor 中的 Log Analytics 工作区。 Windows 代理可以是多宿主的，将数据发送到多个工作区和 System Center Operations Manager 管理组。 Linux 代理只能发送到单个目标。

## <a name="other-services"></a>其他服务
适用于 Linux 和 Windows 的代理不仅可连接到 Azure Monitor，还支持使用 Azure 自动化来托管混合 Runbook 辅助角色和其他服务（例如[更新管理](../../automation/automation-update-management.md)和 [Azure 安全中心](../../security-center/security-center-intro.md)）。 有关混合 Runbook 辅助角色的详细信息，请参阅 [Azure 自动化混合 Runbook 辅助角色](../../automation/automation-hybrid-runbook-worker.md)。  
<!-- Not available in MC: automation/change-tracking.md -->

## <a name="installation-and-configuration"></a>安装和配置

使用 Log Analytics 代理收集数据时，需要了解以下内容，以便规划代理部署：

* 要从 Windows 代理收集数据，可以[配置每个代理以向一个或多个工作区报告](agent-windows.md)，即使它向 System Center Operations Manager 管理组报告也是如此。 Windows 代理最多可向四个工作区报告。
* Linux 代理不支持多宿主，只能向单个工作区报告。
* Windows 代理支持 [FIPS 140 标准](https://docs.microsoft.com/windows/security/threat-protection/fips-140-validation)，但 Linux 代理不支持。  

<!-- Not available in China: System Center Operations Manager  connect to a Log Analytics workspace -->

可通过多种方法安装 Log Analytics 代理并将计算机连接到 Azure Monitor，具体取决于你的要求。 下表详细介绍了每种方法，以便用户确定组织中最适用的方法。

|Source | 方法 | 说明|
|-------|-------------|-------------|
|Azure VM| [通过 Azure 门户手动安装](../../azure-monitor/learn/quick-collect-azurevm.md?toc=/azure/azure-monitor/toc.json) | 指定要从 Log Analytics 工作区部署的 VM。 |
| | 使用 Azure CLI 或 Azure 资源管理器模板通过适用于 [Windows](../../virtual-machines/extensions/oms-windows.md) 或 [Linux](../../virtual-machines/extensions/oms-linux.md) 的 Log Analytics VM 扩展进行安装 | 该扩展在 Azure 虚拟机上安装 Log Analytics 代理，并将虚拟机注册到现有的 Azure Monitor 工作区中。 |
| | [Azure 安全中心自动预配](../../security-center/security-center-enable-data-collection.md) | Azure 安全中心可在所有受支持的 Azure VM 以及任何新建的 Azure VM 中预配 Log Analytics 代理（如果支持），以监视安全漏洞和威胁。 如果启用，将预配任何没有安装代理的新的或现有 VM。 |
| 混合 Windows 计算机| [手动安装](agent-windows.md) | 从命令行安装 Microsoft Monitoring Agent. |
| | [Azure 自动化 DSC](agent-windows.md#install-the-agent-using-dsc-in-azure-automation) | 通过 Azure 自动化 DSC 自动执行安装。 |
| | [具有 Azure Stack 的资源管理器模板](https://github.com/Azure/AzureStack-QuickStart-Templates/tree/master/MicrosoftMonitoringAgent-ext-win) | 如果已在数据中心部署了 Azure Stack，请使用 Azure 资源管理器模板。| 
| 混合 Linux 计算机| [手动安装](../../azure-monitor/learn/quick-collect-linux-computer.md)|调用 GitHub 上托管的包装器脚本安装 Linux 代理。 | 
<!-- Not available in MC: vminsights-enable-overview.md, om-agents.md-->

## <a name="supported-windows-operating-systems"></a>支持的 Windows 操作系统

Windows 代理官方支持以下版本的 Windows 操作系统：

* Windows Server 2019
* Windows Server 2016、版本 1709 和 1803
* Windows Server 2012、2012 R2
* Windows Server 2008 SP2 (x64)、2008 R2
* Windows 10 Enterprise（包括多会话）和 Pro
* Windows 8 企业版和专业版 
* Windows 7 SP1

>[!NOTE]
>虽然适用于 Windows 的 Log Analytics 代理旨在支持服务器监视方案，但我们意识到你可以运行 Windows 客户端以支持为服务器操作系统配置和优化的工作负载。 该代理确实支持 Windows 客户端，但我们的监视解决方案不会专注于客户端监视方案，除非明确说明。

## <a name="supported-linux-operating-systems"></a>受支持的 Linux 操作系统

本部分提供有关受支持的 Linux 分发版的详细信息。

从 2018 年 8 月之后发布的版本开始，我们对支持模型进行了以下更改：  

* 仅支持服务器版本，不支持客户端版本。  
* 将支持重点放在任何 [Azure Linux 认可的发行版](../../virtual-machines/linux/endorsed-distros.md)。 请注意，新的发行版/版本被 Azure Linux 认可和其受 Log Analytics Linux 代理支持，这两者之间可能存在一些延迟。
* 列出的每个主版本支持所有的次版本。
* 超出制造商终止支持日期的版本不受支持。  
* 不支持新版本的 AMI。  
* 默认仅支持运行 SSL 1.x 的版本。

>[!NOTE]
>如果使用的是当前不受支持且与我们的支持模型不一致的发行版或版本，我们建议对此存储库创建分支，并接受 Microsoft 支持不会为已分支的代理版本提供帮助。


### <a name="python-2-requirement"></a>Python 2 要求
 Log Analytics 代理需要 Python 2。 如果虚拟机使用的发行版默认情况下不包括 Python 2，则必须进行安装。 以下示例命令将在不同的发行版上安装 Python 2。

 - Red Hat、CentOS、Oracle：`yum install -y python2`
 - Ubuntu、Debian：`apt-get install -y python2`
 - SUSE: `zypper install -y python2`

Python2 可执行文件必须使用以下命令将“python”设置为别名：

```
alternatives --set python /usr/sbin/python2
```

### <a name="supported-distros"></a>支持的发行版

Linux 代理官方支持以下版本的 Linux 操作系统：

* Amazon Linux 2017.09 (x64)
* CentOS Linux 6 (x64) 和 7 (x64)  
* Oracle Linux 6 和 7 (x64) 
* Red Hat Enterprise Linux Server 6 (x64)、7 (x64) 和 8 (x64)
* Debian GNU/Linux 8 和 9 (x64)
* Ubuntu 14.04 LTS (x86/x64)、16.04 LTS (x64) 和 18.04 LTS (x64)
* SUSE Linux Enterprise Server 12 (x64) 和 15 (x64)

>[!NOTE]
>仅 x86_x64 平台（64 位）支持 OpenSSL 1.1.0，任何平台均不支持早于 1.x 版本的 OpenSSL。
>

### <a name="agent-prerequisites"></a>代理必备组件

下表突出显示了支持的 Linux 发行版所需的包，将在该 Linux 发行版上安装代理。

|所需程序包 |说明 |最低版本 |
|-----------------|------------|----------------|
|Glibc |    GNU C 库 | 2.5-12 
|Openssl    | OpenSSL 库 | 1.0.x 或 1.1.x |
|Curl | cURL Web 客户端 | 7.15.5 |
|Python-ctype | | 
|PAM | 可插入验证模块 | | 

>[!NOTE]
>收集 Syslog 消息时需要 rsyslog 或 syslog ng。 不支持将 Red Hat Enterprise Linux 版本 5、CentOS 和 Oracle Linux 版本 (sysklog) 上的默认 syslog 守护程序用于 syslog 事件收集。 要从这些发行版的此版本中收集 syslog 数据，应安装并配置 rsyslog 守护程序以替换 sysklog。

## <a name="tls-12-protocol"></a>TLS 1.2 协议

为了确保传输到 Azure Monitor 日志的数据的安全性，我们强烈建议你将代理配置为至少使用传输层安全性 (TLS) 1.2。 我们发现旧版 TLS/安全套接字层 (SSL) 容易受到攻击，尽管目前出于向后兼容，这些协议仍可正常工作，但我们**不建议使用**。  有关其他信息，请查看[使用 TLS 1.2 安全地发送数据](data-security.md#sending-data-securely-using-tls-12)。 


## <a name="sha-2-code-signing-support-requirement-for-windows"></a>适用于 Windows 的 SHA-2 代码签名支持要求
Windows 代理将于 2020 年 8 月 17 日开始以独占方式使用 SHA-2 签名。 此更改将影响使用旧版 OS 上的 Log Analytics 代理作为任何 Azure 服务（Azure Monitor、Azure 自动化、Azure 更新管理、Azure 更改跟踪、Azure 安全中心、Azure Sentinel、Windows Defender ATP）一部分的客户。 除非你在旧版 OS 版本（Windows 7、Windows Server 2008 R2 和 Windows Server 2008）上运行代理，否则更改不需要任何客户操作。 在 2020 年 8 月 17 日之前，在旧版 OS 版本上运行的客户必须在其计算机上执行以下操作，否则其代理会停止将数据发送到其 Log Analytics 工作区：

1. 为 OS 安装最新服务器包。 必需的服务包版本包括：
    - Windows 7 SP1
    - Windows Server 2008 SP2
    - Windows Server 2008 R2 SP1

2. 要为 OS 安装 SHA-2 签名 Windows 更新，请参阅[适用于 Windows 和 WSUS 的 2019 SHA-2 代码签名支持要求](https://support.microsoft.com/help/4472027/2019-sha-2-code-signing-support-requirement-for-windows-and-wsus)
3. 更新到 Windows 代理的最新版本（版本 10.20.18029）。
4. 建议将代理配置为[使用 TLS 1.2](agent-windows.md#configure-agent-to-use-tls-12)。 


## <a name="network-requirements"></a>网络要求
适用于 Linux 和 Windows 的代理通过 TCP 端口 443 与 Azure Monitor 服务进行出站通信；如果计算机通过防火墙或代理服务器连接以通过 Internet 进行通信，请查看以下要求来了解所需的网络配置。 如果 IT 安全策略不允许网络上的计算机连接到 Internet，则可以设置 [Log Analytics 网关](gateway.md)并将代理配置为通过该网关连接到 Azure Monitor 日志。 然后，代理可以接收配置信息，并发送根据已在工作区中启用的数据收集规则和监视解决方案收集的数据。

![Log Analytics 代理通信示意图](./media/log-analytics-agent/log-analytics-agent-01.png)

下表列出了实现 Linux 和 Windows 代理与 Azure Monitor 日志通信所必需的代理和防火墙配置信息。

### <a name="firewall-requirements"></a>防火墙要求

|代理资源|端口 |方向 |绕过 HTTPS 检查|
|------|---------|--------|--------|   
|*.ods.opinsights.azure.cn |端口 443 |出站|是 |  
|*.oms.opinsights.azure.cn |端口 443 |出站|是 |  
|*.blob.core.chinacloudapi.cn |端口 443 |出站|是 |  
|*.azure-automation.cn |端口 443 |出站|是 |  

<!-- Correct in China: *.ods.opinsights.azure.cn, *.oms.opinsights.azure.cn, *.blob.core.chinacloudapi.cn, *.azure-automation.cn -->

如果计划使用 Azure 自动化混合 Runbook 辅助角色连接到自动化服务并在其中注册以在环境中使用 Runbook 或管理解决方案，则它必须可以访问[针对混合 Runbook 辅助角色配置网络](../../automation/automation-hybrid-runbook-worker.md#network-planning)中所述的端口号和 URL。 

### <a name="proxy-configuration"></a>代理配置

Windows 和 Linux 代理支持使用 HTTPS 协议通过代理服务器或 Log Analytics 网关与 Azure Monitor 服务进行通信。  并同时支持匿名身份验证和基本身份验证（用户名/密码）。  对于直接连接到服务的 Windows 代理，代理配置在安装过程中指定，或[在部署后](agent-manage.md#update-proxy-settings)从控制面板或使用 PowerShell 指定。  

对于 Linux 代理，代理服务器在安装过程中指定，或者[在安装后](agent-manage.md#update-proxy-settings)通过修改 proxy.conf 配置文件来指定。  Linux 代理的代理配置值具有以下语法：

`[protocol://][user:password@]proxyhost[:port]`

> [!NOTE]
> 如果代理服务器无需进行身份验证，Linux 代理仍要求提供伪用户名/密码。 这可以是任何用户名或密码。

|属性| 说明 |
|--------|-------------|
|协议 | https |
|user | 用于代理身份验证的可选用户名 |
|password | 用于代理身份验证的可选密码 |
|proxyhost | 代理服务器/Log Analytics 网关的地址或 FQDN |
|port | 代理服务器/Log Analytics 网关的可选端口号 |

例如： `https://user01:password@proxy01.contoso.com:30443`

> [!NOTE]
> 如果密码中使用了特殊字符（如“\@”），则会收到代理连接错误，因为值解析不正确。  若要解决此问题，请使用 [URLDecode](https://www.urldecoder.org/) 等工具在 URL 中对密码进行编码。  



## <a name="next-steps"></a>后续步骤

* 查看[数据源](agent-data-sources.md)，了解可用于从 Windows 或 Linux 系统收集数据的数据源。 
* 了解[日志查询](../log-query/log-query-overview.md)以便分析从数据源和解决方案中收集的数据。 
* 了解[监视解决方案](../insights/solutions.md)如何将功能添加到 Azure Monitor，以及如何将数据收集到 Log Analytics 工作区中。

