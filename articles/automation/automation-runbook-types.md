---
title: Azure 自动化 Runbook 类型
description: 介绍了可以在 Azure 自动化中使用的各种 Runbook 类型，以及在确定要使用的类型时的注意事项。
services: automation
ms.subservice: process-automation
origin.date: 03/05/2019
ms.date: 05/11/2020
ms.topic: conceptual
ms.openlocfilehash: dda81a4b780ee355b5b352d2848b26942f9377fb
ms.sourcegitcommit: 7443ff038ea8afe511f7419d9c550d27fb642246
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/09/2020
ms.locfileid: "83001592"
---
# <a name="azure-automation-runbook-types"></a>Azure 自动化 Runbook 类型

Azure 自动化流程自动化服务支持多种类型的 Runbook，如下表中所定义的那样。 若要了解流程自动化环境，请参阅 [Azure 自动化中的 Runbook 执行](automation-runbook-execution.md)。

| 类型 | 说明 |
|:--- |:--- |
| [图形](#graphical-runbooks)|基于 Windows PowerShell 的图形 Runbook，完全在 Azure 门户中的图形编辑器中创建和编辑。 |
| [图形 PowerShell 工作流](#graphical-runbooks)|基于 Windows PowerShell 工作流的图形 Runbook，完全在 Azure 门户中的图形编辑器中创建和编辑。 |
| [PowerShell](#powershell-runbooks) |基于 Windows PowerShell 脚本的文本 Runbook。 |
| [PowerShell 工作流](#powershell-workflow-runbooks)|基于 Windows PowerShell 工作流脚本的文本 Runbook。 |
| [Python](#python-runbooks) |基于 Python 脚本的文本 Runbook。 |

在确定要用于特定 Runbook 的类型时，请注意以下事项。

* 无法将 Runbook 从图形转换为文本类型，反之亦然。
* 将不同类型的 Runbook 用作子 Runbook 时存在各种限制。 有关详细信息，请参阅 [Azure 自动化中的子 Runbook](automation-child-runbooks.md)。

## <a name="graphical-runbooks"></a>图形 Runbook

可以在 Azure 门户中使用图形编辑器创建和编辑图形 Runbook 与图形 PowerShell 工作流 Runbook。 但是，不能通过其他工具创建或编辑此类型的 Runbook。 图形 Runbook 的主要功能：

* 可以导出到自动化帐户中的文件，然后导入到另一个自动化帐户中。 
* 生成 PowerShell 代码。 
* 在导入过程中，可以转换为图形 PowerShell 工作流 runbook，或者由该工作流 runbook 转换而来。 

### <a name="advantages"></a>优点

* 使用可视化的“插入-链接-配置”创作模型。
* 关注数据在流程中如何流动。
* 直观展示管理流程。
* 能够以子 Runbook 的形式包括其他 Runbook，以创建高级工作流。
* 促进模块化编程。

### <a name="limitations"></a>限制

* 无法在 Azure 门户外创建或编辑。
* 可能需要使用包含 PowerShell 代码的代码活动，才能执行复杂逻辑。
* 无法转换为[文本格式](automation-runbook-types.md)之一，也无法将文本 Runbook 转换为图形格式。 
* 不能查看或直接编辑由图形工作流创建的 PowerShell 代码。 可以查看你在任何代码活动中创建的代码。
* 无法在 Linux 混合 Runbook 辅助角色中运行 Runbook。 请参阅[使用混合 Runbook 辅助角色使数据中心或云端的资源实现自动化](automation-hybrid-runbook-worker.md)。

## <a name="powershell-runbooks"></a>PowerShell Runbook

基于 Windows PowerShell 的 PowerShell Runbook。 可以在 Azure 门户中使用文本编辑器直接编辑 Runbook 的代码。  还可以使用任何脱机文本编辑器，以便 [导入 Runbook](manage-runbooks.md) 到 Azure 自动化中。

### <a name="advantages"></a>优点

* 通过 PowerShell 代码来实现所有复杂的逻辑，没有 PowerShell 工作流的各种额外的复杂操作。
* 与 PowerShell 工作流 Runbook 相比，启动速度更快，因为它们在运行前不需要经过编译。
* 在 Azure 中和适用于 Windows 和 Linux 的混合 Runbook 辅助角色上运行。

### <a name="limitations"></a>限制

* 你必须熟悉 PowerShell 脚本。
* Runbook 无法使用[并行处理](automation-powershell-workflow.md#parallel-processing)并行执行多个操作。
* 存在错误时，Runbook 无法使用[检查点](automation-powershell-workflow.md#checkpoints)来恢复 Runbook。
* 使用用于创建新作业的 Start-AzureAutomationRunbook cmdlet 只能将 PowerShell 工作流 Runbook 和图形 Runbook 包括为子 Runbook。

### <a name="known-issues"></a>已知问题

下面是 PowerShell Runbook 的当前已知问题：

* PowerShell Runbook 无法检索未加密且值为 null 的[变量资产](automation-variables.md)。
* PowerShell Runbook 无法检索名称中包含 `*~*` 的变量资产。
* 在 PowerShell Runbook 中，处于循环状态的 [Get-Process](https://docs.microsoft.com/powershell/module/microsoft.powershell.management/get-process?view=powershell-7) 操作在经历大约 80 次迭代后可能会崩溃。
* 如果 PowerShell Runbook 尝试一次性将大量数据写入输出流中，则可能会发生故障。 通常，在处理大型对象时，可以通过让 runbook 只输出所需信息来避免出现这种问题。 例如，可以像 `Get-Process | Select ProcessName, CPU` 一样让 cmdlet 仅输出必需的参数，而不是使用不带限制的 `Get-Process`。

## <a name="powershell-workflow-runbooks"></a>PowerShell 工作流 Runbook

PowerShell 工作流 Runbook 是基于 [Windows PowerShell 工作流](automation-powershell-workflow.md)的文本 Runbook。 可以在 Azure 门户中使用文本编辑器直接编辑 Runbook 的代码。 还可以使用任何脱机文本编辑器，以便 [导入 Runbook](manage-runbooks.md) 到 Azure 自动化中。

### <a name="advantages"></a>优点

* 通过 PowerShell 工作流代码实现所有复杂的逻辑。
* 出现错误时，请使用[检查点](automation-powershell-workflow.md#checkpoints)来恢复操作。
* 使用[并行处理](automation-powershell-workflow.md#parallel-processing)来并行执行多个操作。
* 能够以子 Runbook 的形式包括其他图形 Runbook 和 PowerShell 工作流 Runbook，以创建高级工作流。

### <a name="limitations"></a>限制

* 你必须熟悉 PowerShell 工作流。
* Runbook 还必须处理与 PowerShell 工作流相关的其他复杂问题，例如[反序列化的对象](automation-powershell-workflow.md#code-changes)。
* 与 PowerShell Runbook 相比，Runbook 需要更长时间来启动，因为它们在运行前需要进行编译。
* 若要以子 Runbook 的形式包括 PowerShell Runbook，只能使用 `Start-AzAutomationRunbook` cmdlet。
* 无法在 Linux 混合 Runbook 辅助角色中运行 Runbook。

## <a name="python-runbooks"></a>Python Runbook

在 Python 2 下编译 Python runbook。 可以在 Azure 门户中使用文本编辑器直接编辑 Runbook 的代码。 还可以使用脱机文本编辑器，[将 Runbook 导入](manage-runbooks.md)到 Azure 自动化中。

### <a name="advantages"></a>优点

* 使用功能强大的 Python 库。
* 可以在 Azure 中运行，也可以在 Linux 混合 Runbook 辅助角色上运行。 在安装了 [python2.7](https://www.python.org/downloads/release/latest/python2) 的情况下，支持 Windows 混合 Runbook 辅助角色。

### <a name="limitations"></a>限制

* 你必须熟悉 Python 脚本。
* 目前仅支持 Python 2。 任何特定于 Python 3 的函数都会失败。
* 若要使用第三方库，必须[将包导入到](python-packages.md)自动化帐户中。

## <a name="next-steps"></a>后续步骤

* 若要详细了解图形 Runbook 创作，请参阅 [Azure 自动化中的图形创作](automation-graphical-authoring-intro.md)。
* 若要了解 Runbook 的 PowerShell 和 PowerShell 工作流之间的差异，请参阅[了解 Windows PowerShell 工作流](automation-powershell-workflow.md)。
* 有关如何创建或导入 Runbook 的详细信息，请参阅[在 Azure 自动化中管理 Runbook](manage-runbooks.md)。
* 若要详细了解 PowerShell，包括语言参考和学习模块，请参阅 [PowerShell 文档](https://docs.microsoft.com/powershell/scripting/overview)。
