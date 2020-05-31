---
title: 在 Azure 自动化更新管理中排查 Windows 更新代理问题
description: 了解如何使用更新管理解决方案排查和解决 Windows 更新代理的问题。
services: automation
author: WenJason
ms.author: v-jay
origin.date: 01/16/2020
ms.date: 05/25/2020
ms.topic: conceptual
ms.service: automation
ms.subservice: update-management
manager: digimobile
ms.openlocfilehash: ea256425d517b1940e1b2fc0db74c25a0a1dc6c0
ms.sourcegitcommit: 981a75a78f8cf74ab5a76f9e6b0dc5978387be4b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/22/2020
ms.locfileid: "83801834"
---
# <a name="troubleshoot-windows-update-agent-issues"></a>排查 Windows 更新代理问题

在更新管理中，有多种原因会导致计算机不显示为就绪（正常）状态。 可以检查 Windows 混合 Runbook 辅助角色代理的运行状况来确定根本问题。 下面是计算机的三种就绪状态：

* 迁移就绪性：混合 Runbook 辅助角色已部署，上次看到它的时间不超过 1 小时。
* 离线：混合 Runbook 辅助角色已部署，上次看到它的时间已超过 1 小时。
* 未配置：未找到或未完成加入混合 Runbook 辅助角色。

> [!NOTE]
> 可能需要在经过轻微的延迟之后，Azure 门户才显示当前计算机状态。

本文介绍如何从 Azure 门户为 Azure 计算机运行故障排除程序，以及如何为[离线场景](#troubleshoot-offline)下的非 Azure 计算机运行故障排除程序。 

> [!NOTE]
> 故障排除程序脚本现在包含对 Windows Server Update Services (WSUS) 以及自动下载密钥和安装密钥的检查。 

## <a name="start-the-troubleshooter"></a>启动“故障排除”

对于 Azure 计算机，可以通过在门户中选择“更新代理就绪性”列下的“故障排除”链接，来启动“更新代理故障排除”页。  对于非 Azure 计算机，单击该链接会转到本文。 请参阅有关对 Azure 计算机进行脱机故障排除的[说明](#troubleshoot-offline)。

![虚拟机更新管理列表的屏幕截图](../media/update-agent-issues/vm-list.png)

> [!NOTE]
> 若要检查混合 Runbook 辅助角色的运行状况，VM 必须正在运行。 如果 VM 没有运行，屏幕上会显示“启动 VM”按钮。

在“更新代理故障排除”页上选择“运行检查”，以启动故障排除程序。 故障排除程序使用[运行命令](../../virtual-machines/windows/run-command.md)在计算机上运行脚本以验证依赖项。 完成故障排除时，它会返回检查的结果。

![“更新代理故障排除”页的屏幕截图](../media/update-agent-issues/troubleshoot-page.png)

结果准备就绪后会显示在该页上。 检查部分显示每个检查中包含的内容。

![“更新代理故障排除”检查的屏幕截图](../media/update-agent-issues/update-agent-checks.png)

## <a name="prerequisite-checks"></a>先决条件检查

### <a name="operating-system"></a>操作系统

操作系统检查将验证混合 Runbook 辅助角色是否正在运行下表中所示的操作系统之一。

|操作系统  |注释  |
|---------|---------|
|Windows Server 2012 和更高版本 |需要 .NET Framework 4.6 或更高版本。 （[下载 .NET Framework](https://docs.microsoft.com/dotnet/framework/install/guide-for-developers)。）<br/> 需要 Windows PowerShell 5.1。  （[下载 Windows Management Framework 5.1](https://www.microsoft.com/download/details.aspx?id=54616)。）        |

### <a name="net-462"></a>.NET 4.6.2

.NET Framework 检查将验证系统中是否安装了 [.NET Framework 4.6.2](https://www.microsoft.com/en-us/download/details.aspx?id=53345) 或更高版本。

### <a name="wmf-51"></a>WMF 5.1

WMF 检查将验证系统中是否装有所需版本的 Windows Management Framework (WMF)，即 [Windows Management Framework 5.1](https://www.microsoft.com/download/details.aspx?id=54616)。

### <a name="tls-12"></a>TLS 1.2

此项检查用于确定是否使用 TLS 1.2 加密通信。 该平台不再支持 TLS 1.0。 使用 TLS 1.2 来与更新管理通信。

## <a name="connectivity-checks"></a>连接性检查

### <a name="registration-endpoint"></a>注册终结点

此检查确定代理是否可以与代理服务正确通信。

代理和防火墙配置必须允许混合 Runbook 辅助角色代理与注册终结点通信。 有关要打开的地址和端口的列表，请参阅[网络规划](../automation-hybrid-runbook-worker.md#network-planning)。

### <a name="operations-endpoint"></a>操作终结点

此检查用于确定代理是否可以与作业运行时数据服务正确通信。

代理和防火墙配置必须允许混合 Runbook 辅助角色代理与作业运行时数据服务通信。 有关要打开的地址和端口的列表，请参阅[网络规划](../automation-hybrid-runbook-worker.md#network-planning)。

## <a name="vm-service-health-checks"></a>VM 服务运行状况检查

### <a name="monitoring-agent-service-status"></a>监视代理服务的状态

此项检查确定适用于 Windows 的 Log Analytics 代理 (`healthservice`) 是否正在计算机上运行。 若要详细了解如何对服务进行故障排除，请参阅[适用于 Windows 的 Log Analytics 代理未运行](hybrid-runbook-worker.md#mma-not-running)。

若要重新安装适用于 Windows 的 Log Analytics 代理，请参阅[安装适用于 Windows 的代理](../../azure-monitor/learn/quick-collect-windows-computer.md#install-the-agent-for-windows)。

### <a name="monitoring-agent-service-events"></a>监视代理服务事件

此项检查确定过去 24 小时内计算机上的 Operations Manager 日志中是否出现过任何 4502 事件。

若要详细了解此事件，请参阅 [Operations Manager 日志中的事件 4502](hybrid-runbook-worker.md#event-4502)。

## <a name="access-permissions-checks"></a>访问权限检查

> [!NOTE]
> 故障排除程序当前不通过代理服务器（如果已配置）路由流量。

### <a name="crypto-folder-access"></a>Crypto 文件夹访问

Crypto 文件夹访问检查确定本地系统帐户是否有权访问 C:\ProgramData\Microsoft\Crypto\RSA。

## <a name="troubleshoot-offline"></a><a name="troubleshoot-offline"></a>脱机进行故障排除

可以通过在本地运行脚本，在混合 Runbook 辅助角色上脱机使用故障排除程序。 从 PowerShell 库获取以下脚本：[Troubleshoot-WindowsUpdateAgentRegistration](https://www.powershellgallery.com/packages/Troubleshoot-WindowsUpdateAgentRegistration)。 若要运行该脚本，必须安装 WMF 4.0 或更高版本。 若要下载最新版本的 PowerShell，请参阅[安装各种版本的 PowerShell](https://docs.microsoft.com/powershell/scripting/install/installing-powershell)。

此脚本的输出如以下示例所示：

```output
RuleId                      : OperatingSystemCheck
RuleGroupId                 : prerequisites
RuleName                    : Operating System
RuleGroupName               : Prerequisite Checks
RuleDescription             : The Windows Operating system must be version 6.2.9200 (Windows Server 2012) or higher
CheckResult                 : Passed
CheckResultMessage          : Operating System version is supported
CheckResultMessageId        : OperatingSystemCheck.Passed
CheckResultMessageArguments : {}

RuleId                      : DotNetFrameworkInstalledCheck
RuleGroupId                 : prerequisites
RuleName                    : .NET Framework 4.5+
RuleGroupName               : Prerequisite Checks
RuleDescription             : .NET Framework version 4.5 or higher is required
CheckResult                 : Passed
CheckResultMessage          : .NET Framework version 4.5+ is found
CheckResultMessageId        : DotNetFrameworkInstalledCheck.Passed
CheckResultMessageArguments : {}

RuleId                      : WindowsManagementFrameworkInstalledCheck
RuleGroupId                 : prerequisites
RuleName                    : WMF 5.1
RuleGroupName               : Prerequisite Checks
RuleDescription             : Windows Management Framework version 4.0 or higher is required (version 5.1 or higher is preferable)
CheckResult                 : Passed
CheckResultMessage          : Detected Windows Management Framework version: 5.1.17763.1
CheckResultMessageId        : WindowsManagementFrameworkInstalledCheck.Passed
CheckResultMessageArguments : {5.1.17763.1}

RuleId                      : AutomationAgentServiceConnectivityCheck1
RuleGroupId                 : connectivity
RuleName                    : Registration endpoint
RuleGroupName               : connectivity
RuleDescription             :
CheckResult                 : Failed
CheckResultMessage          : Unable to find Workspace registration information in registry
CheckResultMessageId        : AutomationAgentServiceConnectivityCheck1.Failed.NoRegistrationFound
CheckResultMessageArguments : {}

RuleId                      : AutomationJobRuntimeDataServiceConnectivityCheck
RuleGroupId                 : connectivity
RuleName                    : Operations endpoint
RuleGroupName               : connectivity
RuleDescription             : Proxy and firewall configuration must allow Automation Hybrid Worker agent to communicate with eus2-jobruntimedata-prod-su1.azure-automation.cn
CheckResult                 : Passed
CheckResultMessage          : TCP Test for eus2-jobruntimedata-prod-su1.azure-automation.cn (port 443) succeeded
CheckResultMessageId        : AutomationJobRuntimeDataServiceConnectivityCheck.Passed
CheckResultMessageArguments : {eus2-jobruntimedata-prod-su1.azure-automation.cn}

RuleId                      : MonitoringAgentServiceRunningCheck
RuleGroupId                 : servicehealth
RuleName                    : Monitoring Agent service status
RuleGroupName               : VM Service Health Checks
RuleDescription             : HealthService must be running on the machine
CheckResult                 : Failed
CheckResultMessage          : Log Analytics for Windows service (HealthService) is not running
CheckResultMessageId        : MonitoringAgentServiceRunningCheck.Failed
CheckResultMessageArguments : {Log Analytics agent for Windows, HealthService}

RuleId                      : MonitoringAgentServiceEventsCheck
RuleGroupId                 : servicehealth
RuleName                    : Monitoring Agent service events
RuleGroupName               : VM Service Health Checks
RuleDescription             : Event Log must not have event 4502 logged in the past 24 hours
CheckResult                 : Failed
CheckResultMessage          : Log Analytics agent for Windows service Event Log (Operations Manager) does not exist on the machine
CheckResultMessageId        : MonitoringAgentServiceEventsCheck.Failed.NoLog
CheckResultMessageArguments : {Log Analytics agent for Windows, Operations Manager, 4502}

RuleId                      : CryptoRsaMachineKeysFolderAccessCheck
RuleGroupId                 : permissions
RuleName                    : Crypto RSA MachineKeys Folder Access
RuleGroupName               : Access Permission Checks
RuleDescription             : SYSTEM account must have WRITE and MODIFY access to 'C:\\ProgramData\\Microsoft\\Crypto\\RSA\\MachineKeys'
CheckResult                 : Passed
CheckResultMessage          : Have permissions to access C:\\ProgramData\\Microsoft\\Crypto\\RSA\\MachineKeys
CheckResultMessageId        : CryptoRsaMachineKeysFolderAccessCheck.Passed
CheckResultMessageArguments : {C:\\ProgramData\\Microsoft\\Crypto\\RSA\\MachineKeys}

RuleId                      : TlsVersionCheck
RuleGroupId                 : prerequisites
RuleName                    : TLS 1.2
RuleGroupName               : Prerequisite Checks
RuleDescription             : Client and Server connections must support TLS 1.2
CheckResult                 : Passed
CheckResultMessage          : TLS 1.2 is enabled by default on the Operating System.
CheckResultMessageId        : TlsVersionCheck.Passed.EnabledByDefault
CheckResultMessageArguments : {}
```

## <a name="next-steps"></a>后续步骤

[排查混合 Runbook 辅助角色问题](hybrid-runbook-worker.md)
