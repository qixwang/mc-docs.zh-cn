---
title: 我在 Azure 自动化中的第一个 PowerShell 工作流 Runbook
description: 本教程指导使用 PowerShell 工作流创建、测试和发布一个简单的文本 Runbook。
keywords: powershell 工作流, powershell 工作流示例, 工作流 powershell
services: automation
ms.subservice: process-automation
origin.date: 09/24/2018
ms.date: 05/11/2020
ms.topic: conceptual
ms.openlocfilehash: 29298ec3372f8374362401955101be2d5f0789e7
ms.sourcegitcommit: 7443ff038ea8afe511f7419d9c550d27fb642246
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/09/2020
ms.locfileid: "83001634"
---
# <a name="my-first-powershell-workflow-runbook"></a>我的第一个 PowerShell 工作流 Runbook

> [!div class="op_single_selector"]
> * [图形](automation-first-runbook-graphical.md)
> * [PowerShell](automation-first-runbook-textual-powershell.md)
> * [PowerShell 工作流](automation-first-runbook-textual.md)
> * [Python](automation-first-runbook-textual-python2.md)

本教程指导在 Azure 自动化中创建 [PowerShell 工作流 Runbook](automation-runbook-types.md#powershell-workflow-runbooks)。 从一个简单的 Runbook 开始，你将测试和发布该 Runbook，同时了解如何跟踪 Runbook 作业的状态。 然后，通过修改 Runbook 来实际管理 Azure 资源，“启动 Azure 虚拟机”中对此进行了说明。 最后，通过添加 Runbook 参数来使该 Runbook 更稳健。

## <a name="prerequisites"></a>必备条件

要完成本教程，需要：

* Azure 订阅。 如果没有订阅，可注册[试用版](https://www.azure.cn/pricing/1rmb-trial/?WT.mc_id=A261C142F)。
* [自动化帐户](automation-offering-get-started.md) ，用来保存 Runbook 以及向 Azure 资源进行身份验证。 此帐户必须有权启动和停止虚拟机。
* Azure 虚拟机。 由于我们要停止并启动此虚拟机，因此它不应是用于生产的 VM。

## <a name="step-1---create-new-runbook"></a>步骤 1 - 创建新的 Runbook

首先创建一个输出文本 `Hello World` 的简单 Runbook。

1. 在 Azure 门户中，打开自动化帐户。

   通过自动化帐户页面可快速查看此帐户中的资源。 应该已拥有某些资产。 其中的大多数资产都是自动包含在新自动化帐户中的模块。 此外，应准备好与订阅关联的凭据资产。
 
1. 在“过程自动化”下选择“Runbook”，打开 Runbook 列表。  
1. 选择“创建 Runbook”以创建新的 Runbook。 
1. 将该 Runbook 命名为 **MyFirstRunbook-Workflow**。
1. 在本例中，我们将创建一个 [PowerShell 工作流 runbook](automation-runbook-types.md#powershell-workflow-runbooks)。 对于“Runbook 类型”，请选择“PowerShell 工作流”。  
1. 单击“创建”以创建 Runbook 并打开文本编辑器  。

## <a name="step-2---add-code-to-the-runbook"></a>步骤 2 - 将代码添加到 Runbook

可以直接将代码键入 Runbook 中，或者通过“库”控件选择 cmdlet、Runbook 和资产，并使用任何相关的参数将它们添加到 Runbook。 对于本教程，请直接在 Runbook 中键入代码。

1. Runbook 目前为空，只有必需的 `Workflow` 关键字、Runbook 名称以及括住整个工作流的大括号。

   ```powershell
   Workflow MyFirstRunbook-Workflow
   {
   }
   ```

1. 在大括号中键入 `Write-Output "Hello World"`。

   ```powershell
   Workflow MyFirstRunbook-Workflow
   {
   Write-Output "Hello World"
   }
   ```

1. 通过单击“保存”  保存 Runbook。

## <a name="step-3---test-the-runbook"></a>步骤 3 - 测试 Runbook

在发布 Runbook 以使其可在生产环境中使用之前，应该对其进行测试，以确保它能正常工作。 测试 Runbook 会运行其草稿版，可以交互方式查看其输出。

1. 选择“测试窗格”打开“测试”窗格。 
1. 单击“启动”  以启动测试，并测试唯一启用的选项。
1. 请注意已创建一个 [Runbook 作业](automation-runbook-execution.md)，其状态显示在窗格中。

   作业状态一开始为“排队”，表明作业正在等待云中的 Runbook 辅助角色变为可用状态。 在某个辅助角色认领该作业后，其状态会变为“正在启动”。 最后，当 Runbook 开始实际运行时，其状态会变为“正在运行”。

1. Runbook 作业完成后，“测试”窗格会显示其输出。 在本例中，你会看到 `Hello World`。

   ![Hello World](media/automation-first-runbook-textual/test-output-hello-world.png)

1. 关闭测试窗格以返回到画布。

## <a name="step-4---publish-and-start-the-runbook"></a>步骤 4 - 发布和启动 Runbook

你创建的 Runbook 仍处于草稿模式。 必须首先发布此 Runbook，然后才能在生产中运行它。 当发布 Runbook 时，可以用草稿版本覆盖现有的已发布版本。 在本例中，还没有已发布版本，因为刚刚创建 Runbook。

1. 单击“发布”以发布该 Runbook，并在出现提示时单击“是”   。
1. 向左滚动以在“Runbook”页上查看该 Runbook，请注意“创作状态”字段设置为“已发布”。   
1. 向右滚动以查看 **MyFirstRunbook-Workflow** 的页面。

   使用顶部的选项可以立即启动 Runbook、计划将来的启动时间，或创建一个 [Webhook](automation-webhooks.md)，以便可以通过 HTTP 调用启动 Runbook。

1. 选择“启动”，并在出现提示时选择“是”   启动 runbook。

   ![启动 Runbook](media/automation-first-runbook-textual/automation-runbook-controls-start.png)

1. 此时将打开已创建的 Runbook 作业的“作业”窗格。 在这种情况下，请使窗格保持打开，以便可以查看作业进度。

1. 请注意，作业状态显示在“作业摘要”  中。 此状态与测试 runbook 时看到的状态相匹配。

   ![作业摘要](media/automation-first-runbook-textual/job-pane-status-blade-jobsummary.png)

1. 在 Runbook 状态显示为“已完成”后，单击“输出”  。 “输出”页面将打开，可以在其中看到 `Hello World` 消息。

   ![作业摘要](media/automation-first-runbook-textual/job-pane-status-blade-outputtile.png)

1. 关闭“输出”页。

1. 单击“所有日志”打开 Runbook 作业的“流”窗格  。 在输出流中应该只会看到 `Hello World`。 请注意，“流”窗格可能会显示 Runbook 作业的其他流，例如，详细和错误流（如果 Runbook 向其写入数据）。

   ![作业摘要](media/automation-first-runbook-textual/job-pane-status-blade-alllogstile.png)

1. 关闭“流”窗格和“作业”窗格以返回到 MyFirstRunbook 页。
1. 在“资源”下单击“作业”以打开此 Runbook 的“作业”页面。   此页面将列出你的 Runbook 创建的所有作业。 由于只运行了一次作业，因此你应当只会看到一个作业。

   ![作业](media/automation-first-runbook-textual/runbook-control-job-tile.png)

1. 单击作业名称，打开在启动 Runbook 时出现过的同一个“作业”窗格。 使用此窗格查看为 Runbook 创建的任何作业的详细信息。

## <a name="step-5---add-authentication-to-manage-azure-resources"></a>步骤 5 - 添加身份验证来管理 Azure 资源

已经测试并发布 Runbook，但到目前为止它不执行任何有用的操作。 需要让其管理 Azure 资源。 除非它使用订阅凭据进行身份验证，否则不能执行此操作。 身份验证使用 `Connect-AzureRmAccount` cmdlet。

>[!NOTE]
>对于 PowerShell Runbook，**Add-AzureRMAccount** 和 **login-AzureRMAccount** 是 **Connect-AzureRmAccount** 的别名。

1. 导航到 MyFirstRunbook-Workflow 页，通过单击“编辑”打开文本编辑器  。
2. 删除 `Write-Output` 行。
3. 将光标放在大括号之间的空白行上。
4. 键入（或者复制并粘贴）以下代码，对自动化运行方式帐户的身份验证进行处理。

   ```powershell
   # Ensures you do not inherit an AzureRMContext in your runbook
   Disable-AzureRmContextAutosave -Scope Process

   $Conn = Get-AutomationConnection -Name AzureRunAsConnection
   Connect-AzureRmAccount -ServicePrincipal -Tenant $Conn.TenantID `
   -ApplicationId $Conn.ApplicationID -CertificateThumbprint $Conn.CertificateThumbprint -EnvironmentName "AzureChinaCloud"

   $AzureContext = Select-AzureRmSubscription -SubscriptionId $Conn.SubscriptionID
   ```

1. 单击“测试”  窗格，以便测试 Runbook。
1. 单击“启动”以启动测试  。 完成后，你会看到类似于以下内容的输出，显示帐户中的基本信息。 此操作是对凭据有效的确认。

   ![Authenticate](media/automation-first-runbook-textual/runbook-auth-output.png)

## <a name="step-6---add-code-to-start-a-virtual-machine"></a>步骤 6 – 添加用于启动虚拟机的代码

现在 Runbook 正在向 Azure 订阅进行身份验证，你可以管理资源。 让我们添加一个命令，用于启动虚拟机。 可以选取你的 Azure 订阅中的任何 VM，而现在，你需要将该名称硬编码到 Runbook 中。 如果要跨多个订阅管理资源，需要使用 `-AzureRmContext` 参数以及 [Get-AzureRmContext](https://docs.microsoft.com/powershell/module/azurerm.profile/get-azurermcontext) cmdlet。

1. 在 Connect-AzureRmAccount 后面键入 Start-AzureRmVM -Name 'VMName' -ResourceGroupName 'NameofResourceGroup'（提供要启动的虚拟机的名称和资源组名称）   。  

   ```powershell
   workflow MyFirstRunbook-Workflow
   {
   # Ensures that you do not inherit an AzureRMContext in your runbook
   Disable-AzureRmContextAutosave -Scope Process

   $Conn = Get-AutomationConnection -Name AzureRunAsConnection
   Connect-AzureRmAccount -ServicePrincipal -Tenant $Conn.TenantID -ApplicationId $Conn.ApplicationID -CertificateThumbprint $Conn.CertificateThumbprint -EnvironmentName "AzureChinaCloud"

   $AzureContext = Select-AzureRmSubscription -SubscriptionId $Conn.SubscriptionID

   Start-AzureRmVM -Name 'VMName' -ResourceGroupName 'ResourceGroupName' -AzureRmContext $AzureContext
   }
   ```

1. 保存 Runbook，并单击“测试”窗格，以便可以测试 Runbook  。
1. 单击“启动”以启动测试  。 在测试完成后，检查 VM 是否已启动。

## <a name="step-7---add-an-input-parameter-to-the-runbook"></a>步骤 7 - 将输入参数添加到 Runbook

你的 runbook 当前会启动你在 runbook 中硬编码的 VM。 如果可以在启动 runbook 时指定 VM，则此方法将更有用。 将输入参数添加到 Runbook，以提供该功能。

1. 将 `VMName` 和 `ResourceGroupName` 参数的变量添加到 Runbook，并将这些变量与 `Start-AzureRmVM` cmdlet 一起使用，如下所示。

   ```powershell
   workflow MyFirstRunbook-Workflow
   {
    Param(
     [string]$VMName,
     [string]$ResourceGroupName
    )
   # Ensures you do not inherit an AzureRMContext in your runbook
   Disable-AzureRmContextAutosave -Scope Process

   $Conn = Get-AutomationConnection -Name AzureRunAsConnection
   Connect-AzureRmAccount -ServicePrincipal -Tenant $Conn.TenantID -ApplicationId $Conn.ApplicationID -CertificateThumbprint $Conn.CertificateThumbprint -EnvironmentName "AzureChinaCloud"
   Start-AzureRmVM -Name $VMName -ResourceGroupName $ResourceGroupName
   }
   ```

2. 保存 Runbook 并打开“测试”窗格。 现在可以为测试中使用的两个输入变量提供值。
3. 关闭“测试”窗格。
4. 单击“发布”以发布 Runbook 的新版本  。
5. 停止已启动的 VM。
6. 单击“启动”以启动 Runbook **ResourceGroupName** 。 
7. 键入要启动的 VM 的 **VMNAME** 和 **RESOURCEGROUPNAME** 值。

   ![启动 Runbook](media/automation-first-runbook-textual/automation-pass-params.png)

8. 在 Runbook 完成后，验证 VM 是否已启动。

## <a name="next-steps"></a>后续步骤

* 有关 PowerShell 的详细信息（包括语言参考和学习模块），请参阅 [PowerShell 文档](https://docs.microsoft.com/powershell/scripting/overview)。
* 若要开始使用图形 Runbook，请参阅[我的第一个图形 Runbook](automation-first-runbook-graphical.md)。
* 若要开始使用 PowerShell Runbook，请参阅[我的第一个 PowerShell Runbook](automation-first-runbook-textual-powershell.md)。
* 若要详细了解 Runbook 类型、其优点和限制，请参阅 [Azure 自动化 Runbook 类型](automation-runbook-types.md)。
* 有关 PowerShell 脚本支持功能的详细信息，请参阅 [Azure 自动化中的本机 PowerShell 脚本支持](https://azure.microsoft.com/blog/announcing-powershell-script-support-azure-automation-2/)。
