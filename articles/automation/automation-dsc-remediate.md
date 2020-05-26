---
title: 修正不合规的 Azure Automation State Configuration 服务器
description: 如何将配置根据需要重新应用到配置状态已偏移的服务器
services: automation
ms.service: automation
ms.subservice: dsc
author: WenJason
ms.author: v-jay
ms.topic: conceptual
origin.date: 07/17/2019
ms.date: 05/25/2020
manager: digimobile
ms.openlocfilehash: 15b2bfe5ba868db2287ea3db448709446ac3ba70
ms.sourcegitcommit: 981a75a78f8cf74ab5a76f9e6b0dc5978387be4b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/22/2020
ms.locfileid: "83801296"
---
# <a name="remediate-noncompliant-dsc-servers"></a>修正不合规的 DSC 服务器

当服务器注册到 Azure Automation State Configuration 时，“配置模式”会设置为 `ApplyOnly`、`ApplyandMonitor` 或 `ApplyAndAutoCorrect`。 如果该模式未设置为 `ApplyAndAutoCorrect`，则由于任何原因偏离会状态的服务器会始终保持不合规状态，直到对其进行手动更正为止。

Azure 计算提供名为“运行命令”的功能，允许客户在虚拟机中运行脚本。
本文档提供手动纠正配置偏移时需要用到的此功能的示例脚本。

## <a name="correct-drift-of-windows-virtual-machines-using-powershell"></a>使用 PowerShell 纠正 Windows 虚拟机的偏移

有关如何在 Windows 虚拟机上使用“运行命令”功能的分步说明，请参阅文档页：[使用“运行命令”在 Windows VM 中运行 PowerShell 脚本](/virtual-machines/windows/run-command)。

若要强制 Azure Automation State Configuration 节点下载并应用最新配置，请使用 [Update-DscConfiguration](https://docs.microsoft.com/powershell/module/psdesiredstateconfiguration/update-dscconfiguration) cmdlet。

```powershell
Update-DscConfiguration -Wait -Verbose
```

## <a name="correct-drift-of-linux-virtual-machines"></a>纠正 Linux 虚拟机的偏移

目前未为 Linux 服务器提供类似的功能。
唯一选项是重复注册过程。
对于 Azure 节点，可以通过 Azure 门户或 Az 模块 cmdlet 来纠正偏移。 有关此过程的详细信息将在[加入 Azure Automation State Configuration 管理的计算机](automation-dsc-onboarding.md#enable-a-vm-using-azure-portal)中进行说明。
对于混合节点，可以使用提供的 Python 脚本来纠正偏移。
请参阅 [PowerShell DSC for Linux 存储库](https://github.com/Microsoft/PowerShell-DSC-for-Linux#performing-dsc-operations-from-the-linux-computer)。

## <a name="next-steps"></a>后续步骤

- 有关 PowerShell cmdlet 参考，请参阅 [Azure Automation State Configuration cmdlet](https://docs.microsoft.com/powershell/module/azurerm.automation/#automation)。
- 若要查看在持续部署管道中使用 Azure Automation State Configuration 的示例，请参阅[使用 Azure Automation State Configuration 和 Chocolatey 进行持续部署](automation-dsc-cd-chocolatey.md)。
