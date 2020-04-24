---
title: 修正不符合的 Azure Automation State Configuration 服务器
description: 如何将配置根据需要重新应用到配置状态已偏移的服务器
services: automation
ms.service: automation
ms.subservice: dsc
author: WenJason
ms.author: v-jay
ms.topic: conceptual
origin.date: 07/17/2019
ms.date: 08/26/2019
manager: digimobile
ms.openlocfilehash: e0c2cb4d5b137b7b0d11e74d164bafbfbb0e2c69
ms.sourcegitcommit: c1ba5a62f30ac0a3acb337fb77431de6493e6096
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2020
ms.locfileid: "69993818"
---
# <a name="remediate-non-compliant-dsc-servers"></a>修正不符合 DSC 的服务器

当服务器注册到 Azure Automation State Configuration 时，“配置模式”会设置为 ApplyOnly、ApplyandMonitor 或 ApplyAndAutoCorrect。
如果未将模式设置为 AutoCorrect，则由于任何原因偏离符合状态的服务器会始终保留不符号状态，除非手动纠正它们。

Azure 计算提供名为“运行命令”的功能，允许客户在虚拟机中运行脚本。
本文档提供手动纠正配置偏移时需要用到的此功能的示例脚本。

## <a name="correct-drift-of-windows-virtual-machines-using-powershell"></a>使用 PowerShell 纠正 Windows 虚拟机的偏移

有关如何在 Windows 虚拟机上使用“运行命令”功能的分步说明，请参阅文档页：[使用“运行命令”在 Windows VM 中运行 PowerShell 脚本](/virtual-machines/windows/run-command)。

若要强制 Azure Automation State Configuration 节点下载并应用最新配置，请使用 `Update-DscConfiguration` cmdlet。
有关详细信息，请参阅 cmdlet 文档：[Update-DscConfiguration](https://docs.microsoft.com/powershell/module/psdesiredstateconfiguration/update-dscconfiguration)。

```powershell
Update-DscConfiguration -Wait -Verbose
```

## <a name="correct-drift-of-linux-virtual-machines"></a>纠正 Linux 虚拟机的偏移

目前未为 Linux 服务器提供类似的功能。
唯一选项是重复注册过程。
对于 Azure 节点，可以通过门户或 Az 自动化 cmdlet 来纠正偏移。
有关此过程的详细信息记录在[载入由 Azure Automation State Configuration 管理的计算机](/automation/automation-dsc-onboarding#azure-portal)页中。
对于混合节点，可以通过提供的 Python 脚本来纠正偏移。
请参阅[用于 Linux 存储库的 PowerShell DSC](https://github.com/Microsoft/PowerShell-DSC-for-Linux#performing-dsc-operations-from-the-linux-computer) 中的文档。

## <a name="next-steps"></a>后续步骤

- 有关 PowerShell cmdlet 参考，请参阅 [Azure Automation State Configuration cmdlet](https://docs.microsoft.com/powershell/module/azurerm.automation/#automation)
- 若要查看在持续部署管道中使用 Azure Automation State Configuration 的示例，请参阅[使用 Azure Automation State Configuration 和 Chocolatey 进行持续部署](automation-dsc-cd-chocolatey.md)
