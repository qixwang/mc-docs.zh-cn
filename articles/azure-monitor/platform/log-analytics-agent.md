---
title: Log Analytics 代理概述
description: 本主题可帮助你了解如何使用 Log Analytics 收集数据并监视托管在 Azure、本地或其他云环境中的计算机。
author: lingliw
manager: digimobile
ms.subservice: logs
ms.topic: conceptual
origin.date: 12/24/2019
ms.date: 12/31/2019
ms.author: v-lingwu
ms.openlocfilehash: 8fcb08526236b4db2b19a61c474e75350a35a31f
ms.sourcegitcommit: c1ba5a62f30ac0a3acb337fb77431de6493e6096
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2020
ms.locfileid: "79452448"
---
# <a name="log-analytics-agent-overview"></a>Log Analytics 代理概述
之所以开发 Azure Log Analytics 代理，是为了跨任意云中的虚拟机、本地计算机以及通过 [System Center Operations Manager](https://docs.microsoft.com/system-center/scom/) 监视的计算机进行全面的管理。 Windows 和 Linux 代理将从不同源收集的数据发送到 Azure Monitor 中的 Log Analytics 工作区，并发送在监视解决方案中定义的任何特有日志或指标。 Log Analytics 代理还支持 Azure Monitor 中的见解和其他服务，例如 [Azure 安全中心](/azure/security-center/)和 [Azure 自动化](../../automation/automation-intro.md)。

本文提供该代理的详细概述、系统和网络要求以及不同的部署方法。

> [!NOTE]
> 可能还会看到称为 Microsoft Monitoring Agent (MMA) 或 OMS Linux 代理的 Log Analytics 代理。

> [!NOTE]
> Azure 诊断扩展是一个代理，适用于从计算资源的来宾操作系统中收集监视数据。 请参阅 [Azure Monitor 代理概述](agents-overview.md)，了解不同代理的说明，以及如何按要求选择适当的代理。

## <a name="comparison-to-azure-diagnostics-extension"></a>与 Azure 诊断扩展比较
Azure Monitor 中的 [Azure 诊断扩展](diagnostics-extension-overview.md)还可以用来从 Azure 虚拟机的来宾操作系统收集监视数据。 可以根据要求选择使用任一项，或者二者都使用。 请参阅 [Azure Monitor 代理概述](agents-overview.md)，详细比较 Azure Monitor 代理。 

需要考虑的主要差异为：

- Azure 诊断扩展只能与 Azure 虚拟机配合使用。 Log Analytics 代理可以与 Azure 中的、其他云中的和本地的虚拟机配合使用。
- Azure 诊断扩展将数据发送到 Azure 存储、[Azure Monitor 指标](data-platform-metrics.md)（仅限 Windows）和事件中心。 Log Analytics 代理将数据收集到 [Azure Monitor 日志](data-platform-logs.md)中。
- [Azure 安全中心](/azure/security-center/)等其他服务需要 Log Analytics 代理。

## <a name="costs"></a>成本
Log Analytics 代理不会产生费用，但可能会产生数据引入费用。 请查看[通过 Azure Monitor 日志管理使用情况和成本](manage-cost-storage.md)，详细了解在 Log Analytics 工作区中收集的数据的定价。

## <a name="data-collected"></a>收集的数据
下表列出了在配置 Log Analytics 工作区后即可从所有连接的代理收集的数据的类型。 请查看“Azure Monitor 监视哪些内容？”，了解一系列可以使用 Log Analytics 代理来收集其他数据类型的见解和解决方。

| 数据源 | 说明 |
| --- | --- |
| [Windows 事件日志](data-sources-windows-events.md) | 发送到 Windows 事件日志记录系统的信息。 |
| [Syslog](data-sources-syslog.md)                     | 发送到 Linux 事件日志记录系统的信息。 |
| [“性能”](data-sources-performance-counters.md)  | 数值，用于度量操作系统和工作负载的不同方面的性能。 |
| [IIS 日志](data-sources-iis-logs.md)                 | 在来宾操作系统上运行的 IIS 网站的使用情况信息。 |
| [自定义日志](data-sources-custom-logs.md)           | Windows 和 Linux 计算机上的文本文件中的事件。 |

## <a name="data-destinations"></a>数据目标
Log Analytics 代理将数据发送到 Azure Monitor 中的 Log Analytics 工作区。 Windows 代理可以是多宿主的，用于将数据发送到多个工作区和 System Center Operations Manager 管理组。 Linux 代理只能发送到单个目标。

## <a name="other-services"></a>其他服务
适用于 Linux 和 Windows 的代理不仅可连接到 Azure Monitor，还支持使用 Azure 自动化来托管混合 Runbook 辅助角色和其他服务。 有关混合 Runbook 辅助角色的详细信息，请参阅 [Azure 自动化混合 Runbook 辅助角色](../../automation/automation-hybrid-runbook-worker.md)。  

## <a name="installation-and-configuration"></a>安装和配置

使用 Log Analytics 代理收集数据时，需要了解以下各项以规划代理部署：

* 若要从 Windows 代理收集数据，可[将每个代理配置为向一个或多个工作区报告](agent-windows.md)，即使它目前正在向 System Center Operations Manager 管理组报告。 Windows 代理最多可向四个工作区报告。
* Linux 代理不支持多宿主，只能向一个工作区报告。

如果使用 System Center Operations Manager 2012 R2 或更高版本：

* 向管理组报告的 Linux 计算机必须配置为直接向 Log Analytics 工作区报告。 如果 Linux 计算机已直接向工作区报告，而你想要使用 Operations Manager 来监视这些计算机，请遵循[向 Operations Manager 管理组报告](agent-manage.md#configure-agent-to-report-to-an-operations-manager-management-group)的步骤。
* 可以在 Windows 计算机上安装 Log Analytics Windows 代理，并使其向与某个工作区集成的 Operations Manager 以及另一个工作区报告。



## <a name="supported-windows-operating-systems"></a>支持的 Windows 操作系统

Windows 代理官方支持以下版本的 Windows 操作系统：

* Windows Server 2019
* Windows Server 2008 SP2 (x64)、2008 R2、2012、2012 R2、2016，版本 1709 和 1803
* Windows 7 SP1、Windows 8 企业版和专业版，以及 Windows 10 企业版和专业版

>[!NOTE]
>虽然 Windows 的 Log Analytics 代理旨在支持服务器监视方案，但我们认识到，你可能会运行 Windows 客户端来支持为服务器操作系统配置和优化的工作负荷。 此代理确实支持 Windows 客户端，但是，我们的监视解决方案并不专注于客户端监视方案，除非明确指出。

## <a name="supported-linux-operating-systems"></a>受支持的 Linux 操作系统

本部分提供有关受支持的 Linux 分发版的详细信息。

从 2018 年 8 月之后发布的版本开始，我们对支持模型进行了以下更改：  

* 仅支持服务器版本，不支持客户端版本。  
* 重点支持 [Azure Linux 认可的发行版](../../virtual-machines/linux/endorsed-distros.md)。 请注意，Azure Linux 认可的新发行版/版本与 Log Analytics Linux 代理支持的发行版/版本之间可能存在一定的延迟。
* 列出的每个主版本支持所有的次版本。
* 超出制造商终止支持日期的版本不受支持。  
* 不支持新版本的 AMI。  
* 默认仅支持运行 SSL 1.x 的版本。

>[!NOTE]
>如果使用的是当前不受支持且与我们的支持模型不一致的发行版或版本，我们建议对此存储库创建分支，并接受 Microsoft 支持不会为已分支的代理版本提供帮助。

* Amazon Linux 2017.09 (x64)
* CentOS Linux 6 (x86/x64) 和 7 (x64)  
* Oracle Linux 6 和 7 (x86/x64) 
* Red Hat Enterprise Linux Server 6 (x86/x64) 和 7 (x64)
* Debian GNU/Linux 8 和 9 (x86/x64)
* Ubuntu 14.04 LTS (x86/x64)、16.04 LTS (x86/x64) 和 18.04 LTS (x64)
* SUSE Linux Enterprise Server 12 (x64) 和 15 (x64)

>[!NOTE]
>仅 x86_x64 平台（64 位）支持 OpenSSL 1.1.0，任何平台均不支持早于 1.x 版本的 OpenSSL。
>

### <a name="agent-prerequisites"></a>代理先决条件

下表重点介绍了要安装代理的受支持 Linux 发行版所需的包。

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

为了确保传输到 Azure Monitor 日志的数据的安全性，强烈建议将代理配置为至少使用传输层安全性 (TLS) 1.2。 我们发现旧版 TLS/安全套接字层 (SSL) 容易受到攻击，尽管目前出于向后兼容，这些协议仍可正常工作，但我们**不建议使用**。  有关其他信息，请查看[使用 TLS 1.2 安全地发送数据](data-security.md#sending-data-securely-using-tls-12)。 

## <a name="network-firewall-requirements"></a>网络防火墙要求

下面的信息列出了实现 Linux 和 Windows 代理与 Azure Monitor 日志通信所必需的代理和防火墙配置信息。  

|代理资源|端口 |方向 |绕过 HTTPS 检查|
|------|---------|--------|--------|   
|*.ods.opinsights.chinacloudapi.cn |端口 443 |出站|是 |  
|*.oms.opinsights.chinacloudapi.cn|端口 443 |出站|是 |  
|*.blob.core.chinacloudapi.cn |端口 443 |出站|是 |  
|*.azure-automation.cn |端口 443 |出站|是 |  


如果计划使用 Azure 自动化混合 Runbook 辅助角色连接到自动化服务并在其中注册以在环境中使用 Runbook 或管理解决方案，则它必须可以访问“针对混合 Runbook 辅助角色配置网络”中所述的端口号和 URL。 

Windows 和 Linux 代理支持使用 HTTPS 协议通过代理服务器或 Log Analytics 网关与 Azure Monitor 进行通信。  并同时支持匿名身份验证和基本身份验证（用户名/密码）。  对于直接连接到服务的 Windows 代理，代理配置在安装过程中指定，或[在部署后](agent-manage.md#update-proxy-settings)从控制面板或使用 PowerShell 指定。  

对于 Linux 代理，代理服务器在安装过程中指定，或者[在安装后](agent-manage.md#update-proxy-settings)通过修改 proxy.conf 配置文件来指定。  Linux 代理的代理配置值具有以下语法：

`[protocol://][user:password@]proxyhost[:port]`

> [!NOTE]
> 如果代理服务器无需进行身份验证，Linux 代理仍要求提供伪用户名/密码。 这可以是任何用户名或密码。

|properties| 说明 |
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

* 查看[数据源](../../azure-monitor/platform/agent-data-sources.md)，了解可用于从 Windows 或 Linux 系统收集数据的数据源。 

* 了解[日志查询](../../azure-monitor/log-query/log-query-overview.md)以便分析从数据源和解决方案中收集的数据。 

* 了解[监视解决方案](../../azure-monitor/insights/solutions.md)如何将功能添加到 Azure Monitor，以及如何将数据收集到 Log Analytics 工作区中。




