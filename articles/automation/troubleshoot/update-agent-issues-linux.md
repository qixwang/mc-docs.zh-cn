---
title: 在 Azure 自动化更新管理中排查 Linux 更新代理问题
description: 了解如何使用更新管理解决方案排查和解决 Linux Windows 更新代理的问题。
services: automation
author: WenJason
ms.author: v-jay
origin.date: 12/03/2019
ms.date: 05/25/2020
ms.topic: conceptual
ms.service: automation
ms.subservice: update-management
manager: digimobile
ms.openlocfilehash: ae52ba5457777ab29edf0557514dce03437685b3
ms.sourcegitcommit: 981a75a78f8cf74ab5a76f9e6b0dc5978387be4b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/22/2020
ms.locfileid: "83801923"
---
# <a name="troubleshoot-linux-update-agent-issues"></a>排查 Linux 更新代理问题

在更新管理中，有多种原因会导致计算机不显示为就绪（正常）状态。 可以检查 Linux 混合 Runbook 辅助角色代理的运行状况来确定根本问题。 下面是计算机的三种就绪状态：

* 迁移就绪性：混合 Runbook 辅助角色已部署，上次看到它的时间不超过 1 小时。
* 离线：混合 Runbook 辅助角色已部署，上次看到它的时间已超过 1 小时。
* 未配置：未找到或未完成加入混合 Runbook 辅助角色。

> [!NOTE]
> 可能需要在经过轻微的延迟之后，Azure 门户才显示当前计算机状态。

本文介绍如何从 Azure 门户为 Azure 计算机运行故障排除，以及如何为[离线场景](#troubleshoot-offline)下的非 Azure 计算机运行故障排除。 

> [!NOTE]
> 故障排除程序脚本当前不通过代理服务器（如果已配置）路由流量。

## <a name="start-the-troubleshooter"></a>启动“故障排除”

对于 Azure 计算机，请在门户中选择“更新代理就绪性”列下的“故障排除”链接打开“更新代理故障排除”页。  对于非 Azure 计算机，单击该链接会转到本文。 若要对非 Azure 计算机进行故障排除，请参阅“脱机故障排除”部分中的说明。

![VM 列表页](../media/update-agent-issues-linux/vm-list.png)

> [!NOTE]
> 检查要求 VM 处于运行状态。 如果 VM 未运行，会显示“启动 VM”。

在“更新代理故障排除”页上选择“运行检查”，以启动故障排除程序。 故障排除程序使用[运行命令](../../virtual-machines/linux/run-command.md)在计算机上运行一个脚本来验证依赖项。 完成故障排除时，它会返回检查的结果。

![故障排除页](../media/update-agent-issues-linux/troubleshoot-page.png)

完成检查后，窗口中会返回结果。 检查部分提供了每项检查所要查找的内容相关信息。

![更新代理检查页](../media/update-agent-issues-linux/update-agent-checks.png)

## <a name="prerequisite-checks"></a>先决条件检查

### <a name="operating-system"></a>操作系统

操作系统检查将验证混合 Runbook 辅助角色是否正在运行以下操作系统之一。

|操作系统  |注释  |
|---------|---------|
|CentOS 6 (x86/x64) 和 7 (x64)      | Linux 代理必须具有访问更新存储库的权限。 基于分类的修补需要借助“yum”来返回 CentOS 当前没有的安全数据。         |
|Red Hat Enterprise 6 (x86/x64) 和 7 (x64)     | Linux 代理必须具有访问更新存储库的权限。        |
|SUSE Linux Enterprise Server 11 (x86/x64) 和 12 (x64)     | Linux 代理必须具有访问更新存储库的权限。        |
|Ubuntu 14.04 LTS、16.04 LTS 和 18.04 LTS (x86/x64)      |Linux 代理必须具有访问更新存储库的权限。         |

## <a name="monitoring-agent-service-health-checks"></a>监视代理服务运行状况检查

### <a name="log-analytics-agent"></a>Log Analytics 代理

此项检查确保已安装适用于 Linux 的 Log Analytics 代理。 有关如何安装的说明，请参阅[安装适用于 Linux 的代理](../../azure-monitor/learn/quick-collect-linux-computer.md#install-the-agent-for-linux)。

### <a name="log-analytics-agent-status"></a>Log Analytics 代理状态

此项检查确保适用于 Linux 的 Log Analytics 代理正在运行。 如果该代理未在运行，则可以运行以下命令尝试重启该代理。 有关对该代理进行故障排除的详细信息，请参阅[对 Linux 混合 Runbook 辅助角色进行故障排除](hybrid-runbook-worker.md#linux)。

```bash
sudo /opt/microsoft/omsagent/bin/service_control restart
```

### <a name="multihoming"></a>多宿主

此检查可确定代理是否向多个工作区报告。 更新管理不支持多宿主。

### <a name="hybrid-runbook-worker"></a>混合 Runbook 辅助角色

此项检查将验证适用于 Linux 的 Log Analytics 代理是否具有混合 Runbook 辅助角色包。 更新管理需要此包才能工作。 有关详细信息，请参阅[适用于 Linux 的 Log Analytics 代理未运行](hybrid-runbook-worker.md#oms-agent-not-running)。

更新管理从操作终结点下载混合 Runbook 辅助角色包。 因此，如果混合 Runbook 辅助角色未运行且[操作终结点](#operations-endpoint)发生故障，则更新可能会失败。

### <a name="hybrid-runbook-worker-status"></a>混合 Runbook 辅助角色状态

此检查可确保混合 Runbook 辅助角色在计算机上运行。 如果混合 Runbook 辅助角色正常运行，则以下示例中的进程应该存在。


```bash
nxautom+   8567      1  0 14:45 ?        00:00:00 python /opt/microsoft/omsconfig/modules/nxOMSAutomationWorker/DSCResources/MSFT_nxOMSAutomationWorkerResource/automationworker/worker/main.py /var/opt/microsoft/omsagent/state/automationworker/oms.conf rworkspace:<workspaceId> <Linux hybrid worker version>
nxautom+   8593      1  0 14:45 ?        00:00:02 python /opt/microsoft/omsconfig/modules/nxOMSAutomationWorker/DSCResources/MSFT_nxOMSAutomationWorkerResource/automationworker/worker/hybridworker.py /var/opt/microsoft/omsagent/state/automationworker/worker.conf managed rworkspace:<workspaceId> rversion:<Linux hybrid worker version>
nxautom+   8595      1  0 14:45 ?        00:00:02 python /opt/microsoft/omsconfig/modules/nxOMSAutomationWorker/DSCResources/MSFT_nxOMSAutomationWorkerResource/automationworker/worker/hybridworker.py /var/opt/microsoft/omsagent/<workspaceId>/state/automationworker/diy/worker.conf managed rworkspace:<workspaceId> rversion:<Linux hybrid worker version>
```

## <a name="connectivity-checks"></a>连接性检查

### <a name="general-internet-connectivity"></a>一般 Internet 连接

此检查可确保计算机拥有 Internet 访问权限。

### <a name="registration-endpoint"></a>注册终结点

此项检查确定混合 Runbook 辅助角色是否可与 Log Analytics 工作区中的 Azure 自动化正常通信。

代理和防火墙配置必须允许混合 Runbook 辅助角色代理与注册终结点通信。 有关要打开的地址和端口的列表，请参阅[网络规划](../automation-hybrid-runbook-worker.md#network-planning)。

### <a name="operations-endpoint"></a>操作终结点

此项检查确定 Log Analytics 代理是否可以与作业运行时数据服务正常通信。

代理和防火墙配置必须允许混合 Runbook 辅助角色代理与作业运行时数据服务通信。 有关要打开的地址和端口的列表，请参阅[网络规划](../automation-hybrid-runbook-worker.md#network-planning)。

### <a name="log-analytics-endpoint-1"></a>Log Analytics 终结点 1

此检查会验证计算机是否可以访问 Log Analytics 代理所需的终结点。

### <a name="log-analytics-endpoint-2"></a>Log Analytics 终结点 2

此检查会验证计算机是否可以访问 Log Analytics 代理所需的终结点。

### <a name="log-analytics-endpoint-3"></a>Log Analytics 终结点 3

此检查会验证计算机是否可以访问 Log Analytics 代理所需的终结点。

## <a name="troubleshoot-offline"></a><a name="troubleshoot-offline"></a>脱机进行故障排除

可以通过在本地运行脚本，在混合 Runbook 辅助角色上脱机使用故障排除。 可在脚本中心内找到Python 脚本 [update_mgmt_health_check.py](https://gallery.technet.microsoft.com/scriptcenter/Troubleshooting-utility-3bcbefe6)。 以下示例显示了此脚本的输出示例：

```output
Debug: Machine Information:   Static hostname: LinuxVM2
         Icon name: computer-vm
           Chassis: vm
        Machine ID: 00000000000000000000000000000000
           Boot ID: 00000000000000000000000000000000
    Virtualization: microsoft
  Operating System: Ubuntu 16.04.5 LTS
            Kernel: Linux 4.15.0-1025-azure
      Architecture: x86-64


Passed: Operating system version is supported

Passed: Microsoft Monitoring agent is installed

Debug: omsadmin.conf file contents:
        WORKSPACE_ID=00000000-0000-0000-0000-000000000000
        AGENT_GUID=00000000-0000-0000-0000-000000000000
        LOG_FACILITY=local0
        CERTIFICATE_UPDATE_ENDPOINT=https://00000000-0000-0000-0000-000000000000.oms.opinsights.azure.cn/ConfigurationService.Svc/RenewCertificate
        URL_TLD=opinsights.azure.cn
        DSC_ENDPOINT=https://scus-agentservice-prod-1.azure-automation.cn/Accounts/00000000-0000-0000-0000-000000000000/Nodes\(AgentId='00000000-0000-0000-0000-000000000000'\)
        OMS_ENDPOINT=https://00000000-0000-0000-0000-000000000000.ods.opinsights.azure.cn/OperationalData.svc/PostJsonDataItems
        AZURE_RESOURCE_ID=/subscriptions/00000000-0000-0000-0000-000000000000/resourcegroups/myresourcegroup/providers/microsoft.compute/virtualmachines/linuxvm2
        OMSCLOUD_ID=0000-0000-0000-0000-0000-0000-00
        UUID=00000000-0000-0000-0000-000000000000


Passed: Microsoft Monitoring agent is running

Passed: Machine registered with log analytics workspace:['00000000-0000-0000-0000-000000000000']

Passed: Hybrid worker package is present

Passed: Hybrid worker is running

Passed: Machine is connected to internet

Passed: TCP test for {scus-agentservice-prod-1.azure-automation.cn} (port 443) succeeded

Passed: TCP test for {eus2-jobruntimedata-prod-su1.azure-automation.cn} (port 443) succeeded

Passed: TCP test for {00000000-0000-0000-0000-000000000000.ods.opinsights.azure.cn} (port 443) succeeded

Passed: TCP test for {00000000-0000-0000-0000-000000000000.oms.opinsights.azure.cn} (port 443) succeeded

Passed: TCP test for {ods.systemcenteradvisor.com} (port 443) succeeded

```

## <a name="next-steps"></a>后续步骤

若要排查混合 Runbook 辅助角色的其他问题，请参阅[排查混合 Runbook 辅助角色的问题](hybrid-runbook-worker.md)。
