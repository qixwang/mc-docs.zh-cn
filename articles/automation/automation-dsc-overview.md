---
title: Azure Automation State Configuration 概述
description: 概述 Azure Automation State Configuration 及其术语和已知问题
keywords: powershell dsc, desired state configuration, powershell dsc azure
services: automation
ms.service: automation
ms.subservice: dsc
author: WenJason
ms.author: v-jay
origin.date: 11/06/2018
ms.date: 05/25/2020
ms.topic: conceptual
manager: digimobile
ms.openlocfilehash: 356cf3d3513f3adea41b9298ac70c73c0e89b572
ms.sourcegitcommit: 981a75a78f8cf74ab5a76f9e6b0dc5978387be4b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/22/2020
ms.locfileid: "83801168"
---
# <a name="azure-automation-state-configuration-overview"></a>Azure Automation State Configuration 概述

Azure Automation State Configuration 是一项 Azure 配置管理服务，可用于为任何云或本地数据中心的节点编写、管理和编译 PowerShell Desired State Configuration (DSC) [配置](https://docs.microsoft.com/powershell/scripting/dsc/configurations/configurations)。 该服务还导入 [DSC 资源](https://docs.microsoft.com/powershell/scripting/dsc/resources/resources)，并将配置分配给目标节点，所有操作均在云中进行。 可以通过在“配置管理”下选择“State configuration (DSC)”来访问 Azure 门户中的 Azure Automation State Configuration。 

可以使用 Azure Automation State Configuration 管理各种计算机：

- Azure 虚拟机
- Azure 虚拟机（经典）
- 位于本地或 Azure 以外的云中的物理/虚拟 Windows 计算机
- 位于本地、Azure 或 Azure 以外的云中的物理/虚拟 Linux 计算机

如果你未准备好从云管理计算机配置，可以使用 Azure Automation State Configuration 作为仅限报告的终结点。 使用此功能可以通过 DSC 设置（推送）配置，以及在 Azure 自动化中查看报告详细信息。

> [!NOTE]
> 如果安装的 Azure VM Desired State Configuration 扩展版本高于 2.70，则无需支付额外的费用即可使用 Azure Automation State Configuration 来管理 Azure VM。 有关详细信息，请参阅[**自动化定价页**](https://azure.cn/pricing/details/automation/)。

## <a name="why-use-azure-automation-state-configuration"></a>为何使用 Azure Automation State Configuration

与在 Azure 之外使用 DSC 相比，Azure Automation State Configuration 具有多项优势。 使用此服务可从中心的安全位置跨数千台计算机快速轻松进行缩放。 可以轻松启用计算机、为其分配声明性配置并查看显示每台计算机是否符合指定的所需状态的报告。

适用于 DSC 的 Azure Automation State Configuration 服务类似于 PowerShell 脚本中的 Azure 自动化 Runbook。 换句话说，Azure 自动化以帮助你管理 PowerShell 脚本的相同方式帮助你管理 DSC 配置。 

### <a name="built-in-pull-server"></a>内置拉取服务器

Azure Automation State Configuration 提供了类似于 [Windows 功能 DSC 服务](https://docs.microsoft.com/powershell/scripting/dsc/pull-server/pullserver)的 DSC 拉取服务器。 目标节点可自动接收配置、符合所需状态以及报告其符合性。 Azure 自动化中的内置拉取服务器消除了设置和维护你自己的拉取服务器的需要。 Azure 自动化的目标可以是云中或本地的虚拟机，或物理 Windows 或 Linux 计算机。

### <a name="management-of-all-your-dsc-artifacts"></a>管理所有 DSC 项目

Azure Automation State Configuration 向 [PowerShell Desired State Configuration](https://docs.microsoft.com/powershell/scripting/dsc/overview/overview) 提供的管理层与它为 PowerShell 脚本提供的相同。 可以通过 Azure 门户或 PowerShell 管理所有的 DSC 配置、资源和目标节点。

![“Azure 自动化”页的屏幕截图](./media/automation-dsc-overview/azure-automation-blade.png)

### <a name="import-of-reporting-data-into-azure-monitor-logs"></a>将报表数据导入到 Azure Monitor 日志中

使用 Azure Automation State Configuration 进行管理的节点将详细的报表状态数据发送到内置拉取服务器。 可以将 Azure Automation State Configuration 配置为将此数据发送到 Log Analytics 工作区。 请参阅[将 Azure Automation State Configuration 报表数据转发到 Azure Monitor 日志](automation-dsc-diagnostics.md)。

## <a name="prerequisites-for-using-azure-automation-state-configuration"></a>使用 Azure Automation State Configuration 的先决条件

使用 Azure Automation State Configuration 时，请考虑本部分中的要求。

### <a name="operating-system-requirements"></a>操作系统要求

运行 Windows 的节点支持以下版本：

- Windows Server 2019
- Windows Server 2016
- Windows Server 2012R2
- Windows Server 2012
- Windows Server 2008 R2 SP1
- Windows 10
- Windows 8.1
- Windows 7

>[!NOTE]
>[Microsoft Hyper-V Server](https://docs.microsoft.com/windows-server/virtualization/hyper-v/hyper-v-server-2016) 独立产品 SKU 未包含 DSC 的实现。 因此，无法通过 PowerShell DSC 或 Azure Automation State Configuration 对它进行管理。

对于运行 Linux 的节点，DSC Linux 扩展支持在[支持的 Linux 发行版](https://github.com/Azure/azure-linux-extensions/tree/master/DSC#4-supported-linux-distributions)下列出的所有 Linux 发行版。

### <a name="dsc-requirements"></a>DSC 要求

将会针对在 Azure 中运行的所有 Windows 节点安装 [WMF 5.1](https://docs.microsoft.com/powershell/scripting/wmf/setup/install-configure)（在计算机已启用的情况下）。 对于运行 Windows Server 2012 和 Windows 7 的节点，会启用 [WinRM](https://docs.microsoft.com/powershell/scripting/dsc/troubleshooting/troubleshooting#winrm-dependency)。

将会针对在 Azure 中运行的所有 Linux 节点安装 [PowerShell DSC for Linux](https://github.com/Microsoft/PowerShell-DSC-for-Linux)（在计算机已启用的情况下）。

### <a name="configuration-of-private-networks"></a><a name="network-planning"></a>专用网络的配置

如果节点位于专用网络中，则需要以下端口和 URL。 这些资源为托管节点提供网络连接，并允许 DSC 与 Azure 自动化进行通信。

* 端口：出站 Internet 访问只需 TCP 443
* 全局 URL：* **.azure-automation.cn**
* 代理服务：**https://\<workspaceId\>.agentsvc.azure-auto**

如果要使用在节点之间通信的 DSC 资源（例如 [WaitFor* 资源](https://docs.microsoft.com/powershell/scripting/dsc/reference/resources/windows/waitForAllResource)），则还需允许节点间流量。 请参阅每个 DSC 资源的文档，以了解这些网络要求。

#### <a name="proxy-support"></a>代理支持

Windows 版本 1809 及更高版本中提供了适用于 DSC 代理的代理支持。 启用此选项的方法是：在用来注册节点的 [metaconfiguration 脚本](automation-dsc-onboarding.md#generate-dsc-metaconfigurations)中设置 `ProxyURL` 和 `ProxyCredential` 的值。

>[!NOTE]
>Azure Automation State Configuration 不为旧版 Windows 提供 DSC 代理支持。

对于 Linux 节点，DSC 代理程序支持代理，并使用 `http_proxy` 变量来确定 URL。

#### <a name="azure-automation-state-configuration-network-ranges-and-namespace"></a>Azure Automation State Configuration 网络范围和命名空间

建议在定义异常时使用列出的地址。 对于 IP 地址，可以下载 [Azure 数据中心 IP 范围](https://www.microsoft.com/download/details.aspx?id=42064)。 此文件每周更新，包含当前部署的范围以及即将对 IP 范围进行的更新。

> [!NOTE]
> Azure 数据中心 IP 地址 XML 文件列出了 Azure 数据中心使用的 IP 地址范围。 文件中包含计算、SQL 和存储范围。
>
>每周都将发布更新的文件。 该文件反映当前已部署的范围和任何即将对 IP 范围进行的更改。 数据中心至少在一周后才会使用文件中显示的新范围。 建议每周下载新的 XML 文件。 然后，更新网站以正确标识 Azure 中运行的服务。 

Azure ExpressRoute 用户应注意，此文件过去经常在每个月的第一周更新 Azure 空间的边界网关协议 (BGP) 播发。

## <a name="next-steps"></a>后续步骤

- 若要开始在 Azure Automation State Configuration 中使用 DSC，请参阅 [Azure Automation State Configuration 入门](automation-dsc-getting-started.md)。
- 若要了解如何启用节点，请参阅[允许计算机通过 Azure Automation State Configuration 进行管理](automation-dsc-onboarding.md)。
- 若要了解如何编译 DSC 配置，以便将它们分配给目标节点，请参阅[在 Azure 自动化 State Configuration 中编译配置](automation-dsc-compile.md)。
- 有关 PowerShell cmdlet 参考，请参阅 [Azure 自动化 State Configuration cmdlet](https://docs.microsoft.com/powershell/module/azurerm.automation/#automation)。
- 有关定价信息，请参阅 [Azure Automation State Configuration 定价](https://azure.cn/pricing/details/automation/)。
- 若要查看在持续部署管道中使用 Azure Automation State Configuration 的示例，请参阅[使用 Azure Automation State Configuration 和 Chocolatey 进行持续部署](automation-dsc-cd-chocolatey.md)。
