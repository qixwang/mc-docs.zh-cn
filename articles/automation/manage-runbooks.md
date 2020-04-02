---
title: 在 Azure 自动化中管理 Runbook
description: 本文介绍如何在 Azure 自动化中管理 Runbook。
services: automation
ms.subservice: process-automation
origin.date: 02/14/2019
ms.date: 03/30/2020
ms.topic: conceptual
ms.openlocfilehash: c2c82adc6bec0277ac918d257775a48a7b5fcef9
ms.sourcegitcommit: 90d01d08faf8adb20083363a8e4e5aab139cd9b2
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/26/2020
ms.locfileid: "80290369"
---
# <a name="manage-runbooks-in-azure-automation"></a>在 Azure 自动化中管理 Runbook

可以通过以下方法将 Runbook 添加到 Azure 自动化：[创建新的 Runbook](#create-a-runbook) 或从文件中[导入现有 Runbook](#import-a-runbook)。 本文介绍如何通过文件创建和导入 Runbook。

>[!NOTE]
>本文进行了更新，以便使用新的 Azure PowerShell Az 模块。 你仍然可以使用 AzureRM 模块，至少在 2020 年 12 月之前，它将继续接收 bug 修补程序。 若要详细了解新的 Az 模块和 AzureRM 兼容性，请参阅[新 Azure Powershell Az 模块简介](https://docs.microsoft.com/powershell/azure/new-azureps-module-az?view=azps-3.5.0)。 有关适用于混合 Runbook 辅助角色的 Az 模块安装说明，请参阅安装 [Azure PowerShell 模块](https://docs.microsoft.com/powershell/azure/install-az-ps?view=azps-3.5.0)。 

## <a name="create-a-runbook"></a>创建 runbook

可以使用其中一个 Azure 门户或 Windows PowerShell 在 Azure 自动化中创建一个新的 Runbook。 一旦创建了 Runbook，便可以利用[了解 PowerShell 工作流](automation-powershell-workflow.md)和 [Azure 自动化中的图形创作](automation-graphical-authoring-intro.md)中的信息对其进行编辑。

### <a name="create-a-runbook-in-the-azure-portal"></a>在 Azure 门户中创建 Runbook

1. 在 Azure 门户中，打开自动化帐户。
2. 从中心内，在“过程自动化”下选择“Runbook”，打开 Runbook 列表。  
3. 单击“创建 Runbook”  。
4. 为 Runbook 键入一个名称并选择其[类型](automation-runbook-types.md)。 Runbook 名称必须以字母开头，可以包含字母、数字、下划线和短划线。
5. 单击“创建”  以创建 Runbook 并打开编辑器。

### <a name="create-a-runbook-with-powershell"></a>通过 PowerShell 创建 Runbook

可以使用 [New-AzAutomationRunbook](https://docs.microsoft.com/powershell/module/az.automation/new-azautomationrunbook?view=azps-3.5.0) cmdlet 创建空的 [PowerShell 工作流 Runbook](automation-runbook-types.md#powershell-workflow-runbooks)。 使用 `Type` 参数指定为 `New-AzAutomationRunbook` 定义的 runbook 类型之一。

以下示例演示了如何创建新的空 runbook。

```azurepowershell
New-AzAutomationRunbook -AutomationAccountName MyAccount `
-Name NewRunbook -ResourceGroupName MyResourceGroup -Type PowerShell
```

## <a name="import-a-runbook"></a>导入 Runbook

可以在 Azure 自动化中创建新的 Runbook，方法是导入 PowerShell 脚本或 PowerShell 工作流 ( **.ps1**)、导出的图形 Runbook ( **.graphrunbook**)，或 Python 2 脚本 ( **.py**)。  必须指定在导入期间创建的 [Runbook 类型](automation-runbook-types.md)，并考虑以下注意事项。

* 不包含工作流的 **.ps1** 文件可以导入到 [PowerShell Runbook](automation-runbook-types.md#powershell-runbooks) 中，也可以导入到 [PowerShell 工作流 Runbook](automation-runbook-types.md#powershell-workflow-runbooks) 中。 如果将其导入到 PowerShell 工作流 Runbook 中，则它将转换为工作流。 在这种情况下，Runbook 中将包含注释来描述所做的更改。

* 包含 PowerShell 工作流的 **.ps1** 文件只能导入到 [PowerShell 工作流 Runbook](automation-runbook-types.md#powershell-workflow-runbooks)中。 如果该文件包含多个 PowerShell 工作流，则导入将失败。 必须将每个工作流保存到各自的文件中，并分别导入每个工作流。

* 包含 PowerShell 工作流的 **.ps1** 文件不应导入到 [PowerShell runbook](automation-runbook-types.md#powershell-runbooks) 中，因为 PowerShell 脚本引擎无法识别该文件。

* **.graphrunbook** 文件只能导入到新的[图形 runbook](automation-runbook-types.md#graphical-runbooks) 中。 请注意，只能基于 **.graphrunbook** 文件创建图形 runbook。

### <a name="import-a-runbook-from-a-file-with-the-azure-portal"></a>使用 Azure 门户通过文件导入 Runbook

可通过以下过程将脚本文件导入 Azure 自动化。

> [!NOTE]
> 只能通过门户将 **.ps1** 文件导入 PowerShell 工作流 Runbook。

1. 在 Azure 门户中，打开自动化帐户。
2. 从中心内，在“过程自动化”下选择“Runbook”，打开 Runbook 列表。  
3. 单击“导入 Runbook”。 
4. 单击“Runbook 文件”  ，并选择要导入的文件。
5. 如果“名称”  字段已启用，则可以更改 runbook 名称。 名称必须以字母开头，可以包含字母、数字、下划线和短划线。
6. 将自动选择 [Runbook 类型](automation-runbook-types.md)，但可以在考虑适用的限制后更改该类型。
7. 单击**创建**。 新的 runbook 会出现在自动化帐户的 runbook 列表中。
8. 必须先 [发布 Runbook](#publish-a-runbook) ，才能运行它。

> [!NOTE]
> 在导入图形 Runbook 或图形 PowerShell 工作流 Runbook 后，可以将其转换为其他类型。 但是，无法将这些图形 Runbook 之一转换为文本 Runbook。

### <a name="import-a-runbook-from-a-script-file-with-windows-powershell"></a>使用 Windows PowerShell 从脚本文件导入 Runbook

可以使用 [Import-AzAutomationRunbook](https://docs.microsoft.com/powershell/module/az.automation/import-azautomationrunbook?view=azps-3.5.0) cmdlet 将脚本文件导入为 PowerShell 工作流草稿 Runbook。 如果 Runbook 已存在，除非对该 cmdlet 使用 `Force` 参数，否则导入将失败。

以下示例演示了如何将脚本文件导入到 Runbook 中。

```azurepowershell
$automationAccountName =  "AutomationAccount"
$runbookName = "Sample_TestRunbook"
$scriptPath = "C:\Runbooks\Sample_TestRunbook.ps1"
$RGName = "ResourceGroup"

Import-AzAutomationRunbook -Name $runbookName -Path $scriptPath `
-ResourceGroupName $RGName -AutomationAccountName $automationAccountName `
-Type PowerShellWorkflow
```

## <a name="test-a-runbook"></a>测试 Runbook

测试 Runbook 时，将执行[草稿版](#publish-a-runbook)，并会完成其执行的任何操作。 不会创建作业历史记录，但会在“测试输出”窗格中显示[“输出”](automation-runbook-output-and-messages.md#output-stream)与[“警告和错误”](automation-runbook-output-and-messages.md#message-streams)流。 仅当 [VerbosePreference 变量](automation-runbook-output-and-messages.md#preference-variables)设置为“Continue”时，才会在“输出”窗格中显示发送到[详细流](automation-runbook-output-and-messages.md#message-streams)的消息。

即使草稿版正在运行，该 Runbook 也仍会正常执行，并针对环境中的资源执行任何操作。 因此，只能在非生产资源中测试 Runbook。

测试每种 [Runbook 类型](automation-runbook-types.md)的过程是相同的。 并且，在 Azure 门户中使用文本编辑器进行测试和使用图形编辑器进行测试没有区别。

1. 在[文本编辑器](automation-edit-textual-runbook.md)或[图形编辑器](automation-graphical-authoring-intro.md)中打开 Runbook 的草稿版本。
1. 单击“测试”按钮以打开“测试”页  。
1. 如果 Runbook 有参数，参数会在左窗格中列出，可以在其中提供用于测试的值。
1. 如果要在[混合 Runbook 辅助角色](automation-hybrid-runbook-worker.md)上运行测试，则将“运行设置”  更改为“混合辅助角色”  并选择目标组的名称。  **Azure** ，以在云中运行测试。
1. 单击“开始”  按钮以开始测试。
1. 在测试期间，可以使用“输出”窗格下方的按钮来停止或暂停 [PowerShell 工作流](automation-runbook-types.md#powershell-workflow-runbooks)或[图形](automation-runbook-types.md#graphical-runbooks) runbook。 暂停 Runbook 时，该 Runbook 会完成它在被暂停之前正在进行的活动。 暂停 Runbook 后，可以将它停止或重启。
1. 在输出窗格中检查 Runbook 的输出。

## <a name="publish-a-runbook"></a>发布 Runbook

创建或导入新的 Runbook 时，必须先将其发布，才能导入。 Azure 自动化中的每个 Runbook 都有草稿版和已发布版。 只有已发布版才能用来运行，只有草稿版才能用来编辑。 已发布版不受对草稿版所做的任何更改的影响。 当草稿版可以使用时，可以发布草稿版，这样草稿版就会覆盖当前已发布版。

### <a name="publish-a-runbook-in-the-azure-portal"></a>在 Azure 门户中发布 Runbook

1. 在 Azure 门户中打开 Runbook。
2. 单击“编辑”。 
3. 单击“发布”  ，然后单击“是”来响应验证消息  。

### <a name="publish-a-runbook-using-powershell"></a>使用 PowerShell 发布 Runbook

可以通过 Windows PowerShell 使用 [Publish-AzAutomationRunbook](https://docs.microsoft.com/powershell/module/Az.Automation/Publish-AzAutomationRunbook?view=azps-3.5.0) cmdlet 发布 Runbook。 以下示例展示了如何发布示例 Runbook。

```azurepowershell
$automationAccountName =  "AutomationAccount"
$runbookName = "Sample_TestRunbook"
$RGName = "ResourceGroup"

Publish-AzAutomationRunbook -AutomationAccountName $automationAccountName `
-Name $runbookName -ResourceGroupName $RGName
```

## <a name="next-steps"></a>后续步骤

* 若要深入了解使用文本编辑器编辑 PowerShell 和 PowerShell 工作流 Runbook，请参阅[在 Azure 自动化中编辑文本 Runbook](automation-edit-textual-runbook.md)。
* 若要详细了解图形 Runbook 创作，请参阅 [Azure 自动化中的图形创作](automation-graphical-authoring-intro.md)。
