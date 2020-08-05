---
title: 修正不符合条件的 Azure 自动化 State Configuration 服务器
description: 本文介绍如何根据需要将配置重新应用到配置状态已偏离目标的服务器。
services: automation
ms.service: automation
ms.subservice: dsc
author: WenJason
ms.author: v-jay
ms.topic: conceptual
origin.date: 07/17/2019
ms.date: 08/10/2020
manager: digimobile
ms.openlocfilehash: 98c52b8bc11e5f7b063b3fe26efb652cf12d0938
ms.sourcegitcommit: e6b216b180734783219378410e13192e314a4497
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/05/2020
ms.locfileid: "87788335"
---
# <a name="remediate-noncompliant-azure-automation-state-configuration-servers"></a>修正不符合条件的 Azure 自动化 State Configuration 服务器

向 Azure 自动化 State Configuration 注册服务器时，配置模式设置为 `ApplyOnly`、`ApplyandMonitor` 或 `ApplyAndAutoCorrect`。 如果模式未设置为 `ApplyAndAutoCorrect`，则因任何原因偏离符合状态的服务器将保持在不符合条件的状态下，直到它们被手动更正。

Azure 计算提供了一项名为“Run 命令”的功能，客户可通过它在虚拟机中运行脚本。
本文档提供了手动更正配置偏离情况时此功能的示例脚本。

## <a name="correct-drift-of-windows-virtual-machines-using-powershell"></a>使用 PowerShell 更正 Windows 虚拟机的偏离情况

可使用 `Run` 命令功能更正 Windows 虚拟机的偏离情况。 请参阅[使用 Run 命令在 Windows VM 中运行 PowerShell 脚本](../virtual-machines/windows/run-command.md)。

若要强制 Azure 自动化 State Configuration 节点下载并应用最新配置，请使用 [Update-DscConfiguration](https://docs.microsoft.com/powershell/module/psdesiredstateconfiguration/update-dscconfiguration) cmdlet。

```powershell
Update-DscConfiguration -Wait -Verbose
```

## <a name="correct-drift-of-linux-virtual-machines"></a>更正 Linux 虚拟机的偏离情况

在 Linux 虚拟机中，没有使用 `Run` 命令的选项。 只能通过重复注册过程来更正这些计算机的偏离。 

对于 Azure节点，可通过 Azure 门户或 Az module cmdlet 来纠正偏离情况。 有关此过程的详细信息，可参阅[通过 Azure 门户启用虚拟机](automation-dsc-onboarding.md#enable-a-vm-using-azure-portal)。

对于混合节点，可使用 Python 脚本来纠正偏离情况。 请参阅[通过 Linux 计算机执行 DSC 操作](https://github.com/Microsoft/PowerShell-DSC-for-Linux#performing-dsc-operations-from-the-linux-computer)。

## <a name="next-steps"></a>后续步骤

- 有关 PowerShell cmdlet 参考，请参阅 [Az.Automation](https://docs.microsoft.com/powershell/module/az.automation/?view=azps-3.7.0#automation)。
- 若要查看在持续部署管道中使用 Azure Automation State Configuration 的示例，请参阅[使用 Chocolatey 设置持续部署](automation-dsc-cd-chocolatey.md)。
