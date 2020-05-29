---
title: Azure 自动化中的更新管理
description: 本文介绍为 Windows 和 Linux 计算机管理更新的更新管理功能。
services: automation
ms.subservice: update-management
origin.date: 05/04/2020
ms.date: 05/25/2020
ms.topic: conceptual
ms.openlocfilehash: 347b3ffde212945c6db524af3318765ec08049e0
ms.sourcegitcommit: 981a75a78f8cf74ab5a76f9e6b0dc5978387be4b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/22/2020
ms.locfileid: "83801884"
---
# <a name="update-management-in-azure-automation"></a>Azure 自动化中的更新管理

可以使用 Azure 自动化中的更新管理，为 Azure、本地环境或其他云环境中的 Windows 和 Linux 计算机管理操作系统更新。 可以快速评估所有代理计算机上可用更新的状态，并管理为服务器安装所需更新的过程。

可使用以下方法为虚拟机 (VM) 启用更新管理：

* 从 [Azure 自动化帐户](automation-onboard-solutions-from-automation-account.md)为一个或多个 Azure 计算机启用。
* 为非 Azure 计算机手动启用。
* 从 Azure 门户中的“虚拟机”页为单个 Azure VM 启用。 
* 从 Azure 门户中的“虚拟机”页为[多个 Azure VM ](manage-update-multi.md)启用，在该页面选择这些 VM 即可。

> [!NOTE]
> 更新管理要求将 Log Analytics 工作区链接到自动化帐户。

[Azure 资源管理器模板](automation-update-management-deploy-template.md)可帮助你将更新管理部署到订阅中新的或现有的自动化帐户和 Log Analytics 工作区。

> [!NOTE]
> 无法使用配置了更新管理的计算机来从 Azure 自动化运行自定义脚本。 该计算机只能运行 Microsoft 签名的更新脚本。 

## <a name="update-management-overview"></a>更新管理概述

由更新管理管理的计算机使用以下配置执行评估和更新部署：

* 用于 Windows 或 Linux 的 Log Analytics 代理
* 用于 Linux 的 PowerShell 所需状态配置 (DSC)
* 自动化混合 Runbook 辅助角色
* 适用于 Windows 计算机的 Microsoft 更新或 Windows Server Update Services (WSUS)

下图说明了更新管理如何评估安全更新程序并将其应用于工作区中的所有连接的 Windows 服务器和 Linux 计算机：

![更新管理工作流](./media/automation-update-management/update-mgmt-updateworkflow.png)

更新管理可用于在同一租户的多个订阅中本机登记计算机。

发布包后，Linux 计算机需要 2 到 3 小时才会显示修补程序以供评估。 对于 Windows 计算机，修补程序发布后需要 12 到 15 小时才会显示以供评估。

在计算机完成更新符合性扫描以后，代理会将信息批量转发到 Azure Monitor 日志。 在 Windows 计算机上，符合性扫描默认情况下每 12 小时运行一次。

除了按扫描计划扫描之外，更新符合性扫描还会在 Log Analytics 代理重启后的 15 分钟内、更新安装前和更新安装后开始执行。

对于 Linux 计算机，符合性扫描默认情况下每个小时执行一次。 如果 Log Analytics 代理重启，符合性扫描会在 15 分钟内开始。

更新管理根据所配置的与之进行同步的源来报告计算机的更新情况。 如果将 Windows 计算机配置为向 WSUS 报告，则结果可能不同于 Microsoft 更新所显示的内容，具体取决于 WSUS 上次通过 Microsoft 更新进行同步的时间。 对于配置为向本地存储库（而非公共存储库）报告的 Linux 计算机来说，此行为也是如此。

> [!NOTE]
> 若要正确地向服务进行报告，更新管理要求启用某些 URL 和端口。 若要了解有关这些要求的详细信息，请参阅[网络配置](/automation/automation-hybrid-runbook-worker#network-planning)。

可以通过创建计划的部署，在需要更新的计算机上部署和安装软件更新。 归类为“可选”的更新不包括在 Windows 计算机的部署范围内。 只有必需的更新会包括在部署范围内。

计划的部署定义可接收适用更新的目标计算机。 它通过显式指定某些计算机或选择[计算机组](/azure-monitor/platform/computer-groups)来执行此操作，该计算机组基于一组特定计算机（或基于[ Azure 查询](automation-update-management-query-logs.md)，该查询基于指定的条件动态选择 Azure VM）的日志搜索。 这些组与[范围配置](/azure-monitor/insights/solution-targeting)不同，后者用于控制接收配置以启用更新管理的计算机的目标。 这可以防止它们执行和报告更新符合性，并且可安装已批准的必需更新。

定义部署时，你也可以指定一个计划以批准和设置可以安装更新的时段。 此时段称为维护时段。 如果你需要重启并且选择了适当的重启选项，则保留 20 分钟的维护时段范围以用于重启。 如果修补时间比预期长，并且维护时段中的时间少于 20 分钟，则不会进行重启。

通过 Azure 自动化中的 runbook 安装更新。 无法查看这些 runbook，它们不需要任何配置。 更新部署在创建时，它会创建一个计划，在指定的时间为包括在内的计算机启动主更新 runbook。 此主 Runbook 会在每个代理上启动一个子 Runbook 来安装必需的更新。

目标计算机会按更新部署中指定的日期和时间，以并行方式执行部署。 在安装之前，会运行扫描来验证更新是否仍然是必需的。 对于 WSUS 客户端计算机，如果更新未在 WSUS 中获得批准，则更新部署会失败。
不支持在多个 Log Analytics 工作区（也称为“多宿主”）中对计算机注册更新管理。

## <a name="clients"></a>客户端

### <a name="supported-client-types"></a>支持的客户端类型

下表列出了适用于更新评估的支持的操作系统。 修补需要混合 Runbook 辅助角色。 有关混合 Runbook 辅助角色要求的信息，请参阅[部署 Windows 混合 Runbook 辅助角色](automation-windows-hrw-install.md)和[部署 Linux 混合 Runbook 辅助角色](automation-linux-hrw-install.md)。

> [!NOTE]
> 中国区域不支持对 Linux 计算机的更新评估。 

|操作系统  |注释  |
|---------|---------|
|Windows Server 2019 (Datacenter/Datacenter Core/Standard)<br><br>Windows Server 2016 (Datacenter/Datacenter Core/Standard)<br><br>Windows Server 2012 R2 (Datacenter/Standard)<br><br>Windows Server 2012 || 
|Windows Server 2008 R2（RTM 和 SP1 Standard）| 更新管理仅支持对此操作系统进行评估。 不支持修补，因为 Windows Server 2008 R2 不支持[混合 Runbook 辅助角色](automation-windows-hrw-install.md)。 |
|CentOS 6 (x86/x64) 和 7 (x64)      | Linux 代理需要访问更新存储库的权限。 基于分类的修补需要借助 `yum` 来返回 CentOS 在其 RTM 版本中没有的安全数据。 若要详细了解 CentOS 上基于分类的修补信息，请参阅 [Linux 上的更新分类](automation-view-update-assessments.md#linux-2)。          |
|Red Hat Enterprise 6 (x86/x64) 和 7 (x64)     | Linux 代理需要访问更新存储库的权限。        |
|SUSE Linux Enterprise Server 11 (x86/x64) 和 12 (x64)     | Linux 代理需要访问更新存储库的权限。        |
|Ubuntu 14.04 LTS、16.04 LTS 和 18.04 (x86/x64)      |Linux 代理需要访问更新存储库的权限。         |

> [!NOTE]
> 可以通过更新管理来管理 Azure 虚拟机规模集。 更新管理适用于实例本身，而非基础映像。 你需要以增量方法计划更新，这样所有 VM 实例才不会同时更新。

### <a name="unsupported-client-types"></a>不支持的客户端类型

下表列出了不受支持的操作系统：

|操作系统  |注释  |
|---------|---------|
|Windows 客户端     | 不支持客户端操作系统（例如 Windows 7 和 Windows 10）。        |
|Windows Server 2016 Nano Server     | 不支持。       |
|Azure Kubernetes 服务节点 | 不支持。 使用[将安全更新和内核更新应用于 Azure Kubernetes 服务 (AKS) 中的 Linux 节点](../aks/node-updates-kured.md)中介绍的修补过程|

### <a name="client-requirements"></a>客户端要求

以下信息介绍操作系统特定的客户端要求。 有关其他指南，请参阅[网络规划](#ports)。

#### <a name="windows"></a>Windows

Windows 代理必须配置为与 WSUS 服务器通信或需要有权访问 Microsoft 更新。 有关如何安装适用于 Windows 的 Log Analytics 代理的信息，请参阅[将 Windows 计算机连接到 Azure Monitor](../azure-monitor/platform/agent-windows.md)。

可以将更新管理与 Microsoft Endpoint Configuration Manager 配合使用。 若要了解有关集成方案的详细信息，请参阅[将 Configuration Manager 与更新管理集成](updatemgmt-mecmintegration.md#configuration)。 Configuration Manager 环境中由站点管理的 Windows 服务器需要[适用于 Windows 的 Log Analytics 代理](../azure-monitor/platform/agent-windows.md)。 

默认情况下，从 Azure 市场部署的 Windows VM 设置为从 Windows 更新服务接收自动更新。 添加 Windows VM 到工作区时，此行为不会更改。 如果不主动通过更新管理管理更新，则应用默认行为（即自动应用更新）。

> [!NOTE]
> 可以修改组策略，使计算机重启只能由用户而非系统来执行。 如果更新管理在没有用户的手动交互的情况下无权重启计算机时，托管的计算机可能会停滞。 有关详细信息，请参阅[为自动更新配置组策略设置](https://docs.microsoft.com/windows-server/administration/windows-server-update-services/deploy/4-configure-group-policy-settings-for-automatic-updates)。

#### <a name="linux"></a>Linux

对于 Linux，计算机需要有权访问专用或公共的更新存储库。 需要 TLS 1.1 或 TLS 1.2 才能与更新管理进行交互。 更新管理不支持配置为向多个 Azure Log Analytics 工作区报告的适用于 Linux 的 Log Analytics 代理。 计算机还必须安装 Python 2.x。

> [!NOTE]
> 中国区域不支持对 Linux 计算机的更新评估。 

有关如何安装适用于 Linux 的 Log Analytics 代理以及下载最新版本的信息，请参阅[适用于 Linux 的 Log Analytics 代理](../azure-monitor/platform/agent-linux.md)。 

## <a name="permissions"></a>权限

若要创建和管理更新部署，需要特定的权限。 若要了解这些权限，请参阅[基于角色的访问 - 更新管理](automation-role-based-access-control.md#update-management-permissions)。

## <a name="update-management-components"></a>更新管理组件

更新管理使用本部分中介绍的资源。 启用更新管理时，这些资源会自动添加到自动化帐户。

### <a name="hybrid-runbook-worker-groups"></a>混合 Runbook 辅助角色组

启用更新管理以后，任何直接连接到 Log Analytics 工作区的 Windows 计算机都会自动配置为混合 Runbook 辅助角色，为支持更新管理的 runbook 提供支持。

更新管理管理的每台 Windows 计算机都会作为自动化帐户的一个“系统混合辅助角色组”列在“混合辅助角色组”窗格中。 组使用 `Hostname FQDN_GUID` 命名约定。 不能在帐户中通过 Runbook 将这些组作为目标进行操作。 如果你尝试进行此操作，则尝试将失败。 这些组仅用于为更新管理提供支持。

如果对更新管理和混合 Runbook 辅助角色组成员身份使用同一帐户，则可以将 Windows 计算机添加到自动化帐户中的混合 Runbook 辅助角色组，为自动化 runbook 提供支持。 此功能是在 7.2.12024.0 版本的混合 Runbook 辅助角色中添加的。

### <a name="management-packs"></a>管理包

如果 Operations Manager 管理组已[连接到 Log Analytics 工作区](../azure-monitor/platform/om-agents.md)，则 Operations Manager 中将安装以下管理包。 这些管理包也会安装在直接连接的 Windows 计算机上，用于更新管理。 你不需要对这些管理包进行配置或管理。

* Microsoft System Center Advisor Update Assessment Intelligence Pack (Microsoft.IntelligencePacks.UpdateAssessment)
* Microsoft.IntelligencePack.UpdateAssessment.Configuration (Microsoft.IntelligencePack.UpdateAssessment.Configuration)
* 更新部署 MP

> [!NOTE]
> 如果 Operations Manager 1807 或 2019 管理组连接到 Log Analytics 工作区，且管理组中配置了代理以收集日志数据，则你需要在 Microsoft.IntelligencePacks.AzureAutomation.HybridAgent.Init 规则中替代参数 `IsAutoRegistrationEnabled` 并将其设置为 True。

若要详细了解管理包的更新，请参阅[将 Operations Manager 连接到 Azure Monitor 日志](../azure-monitor/platform/om-agents.md)。

> [!NOTE]
> 若要更新管理使用 Log Analytics 代理完全管理计算机，必须更新为适用于 Windows 的 Log Analytics 代理或适用于 Linux 的 Log Analytics 代理。 若要了解如何更新代理，请参阅[如何升级 Operations Manager 代理](https://docs.microsoft.com/system-center/scom/deploy-upgrade-agents)。 在使用 Operations Manager 的环境中，必须运行 System Center Operations Manager 2012 R2 UR 14 或更高版本。

## <a name="data-collection"></a>数据收集

### <a name="supported-sources"></a>支持的源

下表介绍了更新管理支持的连接的源：

| 连接的源 | 支持 | 说明 |
| --- | --- | --- |
| Windows 代理 |是 |“更新管理”从 Windows 代理收集有关系统更新的信息，并开始安装必需的更新。 |
| Linux 代理 |是 |“更新管理”从 Linux 代理收集有关系统更新的信息，然后开始在受支持的发行版上安装必需的更新。 |
| Operations Manager 管理组 |是 |“更新管理”从已连接的管理组中的代理收集有关系统更新的信息。<br/><br/>从 Operations Manager 代理到 Azure Monitor 日志的直接连接不是必需的。 数据将从管理组转发到 Log Analytics 工作区。 |

### <a name="collection-frequency"></a>收集频率

更新管理使用以下规则扫描托管计算机以获取数据。 可能需要 30 分钟到 6 小时，仪表板才会显示托管计算机提供的已更新数据。

* 每台 Windows 计算机 - 更新管理每天对每台计算机进行两次扫描。 它每隔 15 分钟就会查询 Windows API 以获取上次更新时间，确定状态是否已更改。 如果状态已更改，更新管理开始执行一次符合性扫描。

* 每台 Linux 计算机 - 更新管理每小时扫描一次。

对于使用更新管理的计算机，Azure Monitor 日志的平均数据使用量约为每月 25 MB。 此值只是一个近似值，且随时可能随环境而更改。 建议你监视环境以跟踪实际使用量。 有关分析数据使用量的详细信息，请参阅[管理使用量和成本](../azure-monitor/platform/manage-cost-storage.md)。

## <a name="network-planning"></a><a name="ports"></a> 网络规划

更新管理特别需要以下地址。 与以下地址的通信通过端口 443 进行。

|Azure 中国云  |
|---------|---------|
|*.ods.opinsights.azure.cn    |
|*.oms.opinsights.azure.cn     |
|*.blob.core.chinacloudapi.cn |
|*.azure-automation.cn |

对于 Windows 计算机，还必须允许流向 Windows 更新所需的任何终结点的流量。 可以在[与 HTTP/代理相关的问题](https://docs.microsoft.com/windows/deployment/update/windows-update-troubleshooting#issues-related-to-httpproxy)中找到所需终结点的更新列表。 如果你有本地 [Windows 更新服务器](https://docs.microsoft.com/windows-server/administration/windows-server-update-services/plan/plan-your-wsus-deployment)，则还必须允许流向 [WSUS 密钥](https://docs.microsoft.com/windows/deployment/update/waas-wu-settings#configuring-automatic-updates-by-editing-the-registry)中指定的服务器的流量。

若要详细了解混合 Runbook 辅助角色所需的端口，请参阅[混合 Runbook 辅助角色的更新管理地址](automation-hybrid-runbook-worker.md#update-management-addresses-for-hybrid-runbook-worker)。

建议在定义异常时使用列出的地址。 对于 IP 地址，可以下载 [Azure 数据中心 IP 范围](https://www.microsoft.com/download/details.aspx?id=42064)。 此文件每周更新一次，反映当前已部署的范围和任何即将对 IP 范围进行的更改。

按照[连接无法访问 Internet 的计算机](../azure-monitor/platform/gateway.md)中的说明，配置无 Internet 访问权限的计算机。

## <a name="update-classifications"></a>更新分类

下表定义了更新管理支持的 Windows 更新的分类。 

|分类  |说明  |
|---------|---------|
|关键更新     | 解决关键、非安全相关错误的特定问题的更新。        |
|安全更新     | 产品特定、安全相关问题的更新。        |
|更新汇总     | 一起打包以便于部署的一组累积修补程序。        |
|功能包     | 在产品版本以外发布的新产品功能。        |
|服务包     | 应用于应用程序的一组累积修补程序。        |
|定义更新     | 对病毒或其他定义文件的更新。        |
|工具     | 可帮助完成一个或多个任务的实用工具或功能。        |
|更新     | 对当前已安装的应用程序或文件的更新。        |

下表定义了支持的 Linux 更新的分类。

|分类  |说明  |
|---------|---------|
|关键和安全更新     | 特定问题或产品特定、安全相关问题的更新。         |
|其他更新     | 本质上不是关键更新或不是安全更新的所有其他更新。        |

对于 Linux，更新管理可以区分云中的关键更新和安全更新，同时显示因云中数据扩充而产生的评估数据。 为了进行修补，更新管理依赖于计算机上提供的分类数据。 与其他分发版不同，CentOS 在 RTM 版本中没有这些信息。 如果你已将 CentOS 计算机配置为返回以下命令的安全数据，则更新管理能够基于分类进行修补。

```bash
sudo yum -q --security check-update
```

当前没有受支持的方法可用来在 CentOS 上提供原生分类数据。 目前，只能尽力为可能自己实现了此功能的客户提供支持。 

## <a name="integrate-update-management-with-configuration-manager"></a>将更新管理与 Configuration Manager 集成

已经投资购买了 Microsoft Endpoint Configuration Manager 来管理电脑、服务器和移动设备的客户还依赖 Configuration Manager 的优势和成熟度来帮助他们管理软件更新。 若要了解如何将更新管理与 Configuration Manager 集成，请参阅[将 Configuration Manager 与更新管理集成](updatemgmt-mecmintegration.md)。

## <a name="third-party-updates-on-windows"></a>Windows 上的第三方更新

更新管理依赖于本地配置的更新存储库（WSUS 或 Windows 更新）来更新受支持的 Windows 系统。 通过 [System Center Updates Publisher](https://docs.microsoft.com/configmgr/sum/tools/updates-publisher) 之类的工具，你可使用 WSUS 导入和发布自定义更新。 在这种情况下，允许更新管理使用第三方软件来更新使用 Configuration Manager 作为其更新存储库的计算机。 若要了解如何配置 Updates Publisher，请参阅[安装 Updates Publisher](https://docs.microsoft.com/configmgr/sum/tools/install-updates-publisher)。

## <a name="enable-update-management"></a>启用更新管理

Azure [资源管理器模板](automation-update-management-deploy-template.md)可帮助你将更新管理部署到订阅中新的或现有的自动化帐户和 Azure Monitor Log Analytics 工作区。 它不配置应管理的计算机的范围，此操作在使用模板后作为单独的步骤执行。

可以通过以下方式启用更新管理并选择要管理的计算机：

* [通过虚拟机](automation-onboard-solutions-from-vm.md)。
* [通过 Azure 自动化帐户](automation-onboard-solutions-from-automation-account.md)。

## <a name="next-steps"></a>后续步骤

查看 Azure 自动化[常见问题解答](automation-faq.md)，了解关于更新管理的常见问题。
