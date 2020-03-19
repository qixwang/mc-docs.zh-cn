---
title: Azure Automation State Configuration 概述
description: 对 Azure Automation State Configuration (DSC) 及其术语和已知问题的概述
keywords: powershell dsc, desired state configuration, powershell dsc azure
services: automation
ms.service: automation
ms.subservice: dsc
author: WenJason
ms.author: v-jay
origin.date: 11/06/2018
ms.date: 03/03/2020
ms.topic: conceptual
manager: digimobile
ms.openlocfilehash: 20ee37508f21a080b19fce88b44c3eb12182997a
ms.sourcegitcommit: 3c98f52b6ccca469e598d327cd537caab2fde83f
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/13/2020
ms.locfileid: "79292624"
---
# <a name="azure-automation-state-configuration-overview"></a>Azure Automation State Configuration 概述

Azure Automation State Configuration 是一种 Azure 服务，允许编写、管理和编译 PowerShell Desired State Configuration (DSC) [配置](https://docs.microsoft.com/powershell/scripting/dsc/configurations/configurations)，导入 [DSC 资源](https://docs.microsoft.com/powershell/scripting/dsc/resources/resources)，并将配置分配给目标节点，所有操作均在云中进行。

## <a name="why-use-azure-automation-state-configuration"></a>为何使用 Azure Automation State Configuration

与在 Azure 之外使用 DSC 相比，Azure Automation State Configuration 具有多项优势。

### <a name="built-in-pull-server"></a>内置拉取服务器

Azure Automation State Configuration 提供了类似于 [Windows 功能 DSC 服务](https://docs.microsoft.com/powershell/scripting/dsc/pull-server/pullserver)的 DSC 拉取服务器，这样目标节点可自动接收配置，符合所需状态，并报告回其符合性。 Azure 自动化中的内置拉取服务器消除了设置和维护你自己的拉取服务器的需要。 Azure 自动化的目标可以是云中或本地的虚拟机，或物理 Windows 或 Linux 计算机。

### <a name="management-of-all-your-dsc-artifacts"></a>管理所有 DSC 项目

Azure Automation State Configuration 向 [PowerShell Desired State Configuration](https://docs.microsoft.com/powershell/scripting/dsc/overview/overview) 提供的管理层与 Azure 自动化为 PowerShell 脚本提供的相同。

从 Azure 门户，或从 PowerShell，你可以管理所有的 DSC 配置、资源和目标节点。

![“Azure 自动化”页的屏幕截图](./media/automation-dsc-overview/azure-automation-blade.png)

### <a name="import-reporting-data-into-azure-monitor-logs"></a>将报表数据导入到 Azure Monitor 日志中

使用 Azure Automation State Configuration 进行管理的节点将详细的报表状态数据发送到内置拉取服务器。 可以将 Azure Automation State Configuration 配置为将此数据发送到 Log Analytics 工作区。 若要了解如何将 State Configuration 状态数据发送到 Log Analytics 工作区，请参阅[将 Azure Automation State Configuration 报表数据转发到 Azure Monitor 日志](automation-dsc-diagnostics.md)。

## <a name="prerequisites"></a>先决条件

在使用 Azure Automation State Configuration (DSC) 时，请考虑以下要求。

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

[Microsoft Hyper-V Server](https://docs.microsoft.com/windows-server/virtualization/hyper-v/hyper-v-server-2016) 独立产品 SKU 不包含 Desired State Configuraion 的实现，因此无法通过 PowerShell DSC 或 Azure 自动化 State Configuration 进行管理。

运行 Linux 的节点支持以下发行版/版本：

DSC Linux 扩展支持[支持的 Linux 发行版](https://github.com/Azure/azure-linux-extensions/tree/master/DSC#4-supported-linux-distributions)下列出的所有 Linux 发行版。

### <a name="dsc-requirements"></a>DSC 要求

对于在 Azure 中运行的所有 Windows 节点，[WMF 5.1](https://docs.microsoft.com/powershell/scripting/wmf/setup/install-configure) 将在载入时安装。  对于运行 Windows Server 2012 和 Windows 7 的节点，[将会启用 WinRM](https://docs.microsoft.com/powershell/scripting/dsc/troubleshooting/troubleshooting#winrm-dependency)。

对于在 Azure 中运行的所有 Linux 节点，[PowerShell DSC for Linux](https://github.com/Microsoft/PowerShell-DSC-for-Linux) 将在载入时安装。

### <a name="network-planning"></a>配置专用网络

如果节点位于专用网络中，则 State Configuration (DSC) 需要以下端口和 URL 才能与自动化通信：

* 端口：只需使用 TCP 443 即可进行出站 Internet 访问。
* 全局 URL：*.azure-automation.cn
* 代理服务： https://\<workspaceId\>.agentsvc.azure-automation.cn

这为托管节点提供网络连接，以与 Azure 自动化通信。
如果要使用在节点之间通信的 DSC 资源（例如 [WaitFor* 资源](https://docs.microsoft.com/powershell/scripting/dsc/reference/resources/windows/waitForAllResource)），则还需要允许节点间通信。
请参阅每个 DSC 资源的文档，以了解这些网络要求。

#### <a name="proxy-support"></a>代理支持

Windows 版本 1809 及更高版本中提供了适用于 DSC 代理的代理支持。
若要配置此选项，请在用于注册节点的 [metaconfiguration 脚本](automation-dsc-onboarding.md#generating-dsc-metaconfigurations)中设置 **ProxyURL** 和 **ProxyCredential** 的值。
代理在以前版本的 Windows 的 DSC 中不可用。

对于 Linux 节点，DSC 代理支持代理，并将利用 http_proxy 变量来确定 URL。

#### <a name="azure-state-configuration-network-ranges-and-namespace"></a>Azure State Configuration 网络范围和命名空间

建议在定义异常时使用列出的地址。 对于 IP 地址，可以下载 [Azure 数据中心 IP 范围](https://www.microsoft.com/download/details.aspx?id=42064)。 此文件每周更新，包含当前部署的范围以及即将对 IP 范围进行的更新。

> [!NOTE]
> Azure 数据中心 IP 地址 XML 文件列出了 Azure 数据中心使用的 IP 地址范围。 文件中包含计算、SQL 和存储范围。
>
>每周都将发布更新的文件。 该文件反映当前已部署的范围和任何即将对 IP 范围进行的更改。 数据中心至少在一周后才会使用文件中显示的新范围。
>
> 建议每周下载新的 XML 文件。 然后，更新网站以正确地标识 Azure 中运行的服务。 Azure ExpressRoute 用户应注意，此文件过去经常在每个月的第一周更新 Azure 空间的边界网关协议 (BGP) 播发。

## <a name="next-steps"></a>后续步骤

- 有关入门信息，请参阅 [Azure Automation State Configuration 入门](automation-dsc-getting-started.md)
- 要了解如何登记节点，请参阅[登记由 Azure Automation State Configuration 管理的计算机](automation-dsc-onboarding.md)
- 若要了解如何编译 DSC 配置，以便将它们分配给目标节点，请参阅[在 Azure Automation State Configuration 中编译配置](automation-dsc-compile.md)
- 有关 PowerShell cmdlet 参考，请参阅 [Azure Automation State Configuration cmdlet](https://docs.microsoft.com/powershell/module/azurerm.automation/#automation)
- 有关定价信息，请参阅 [Azure Automation State Configuration 定价](https://azure.cn/pricing/details/automation/)
- 若要查看在持续部署管道中使用 Azure Automation State Configuration 的示例，请参阅[使用 Azure Automation State Configuration 和 Chocolatey 进行持续部署](automation-dsc-cd-chocolatey.md)