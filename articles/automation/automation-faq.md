---
title: Azure 自动化常见问题解答 | Microsoft Docs
description: 解答有关 Azure 自动化的常见问题。
services: automation
ms.subservice: ''
ms.topic: conceptual
author: WenJason
ms.author: v-jay
origin.date: 02/25/2020
ms.date: 05/25/2020
ms.openlocfilehash: 475f87b878ccf8728b184cfc144024f9ba573fb8
ms.sourcegitcommit: 981a75a78f8cf74ab5a76f9e6b0dc5978387be4b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/22/2020
ms.locfileid: "83801913"
---
# <a name="azure-automation-frequently-asked-questions"></a>Azure 自动化常见问题解答

本常见问题解答文章列出了有关 Azure 自动化的常见问题。 如果你有与 Azure 自动化功能相关的任何其他问题，请转到论坛并发布问题。 当某个问题经常被问到时，我们会将该问题添加到本文中，以便可以轻松快捷地找到该问题。

>[!NOTE]
>本文进行了更新，以便使用新的 Azure PowerShell Az 模块。 你仍然可以使用 AzureRM 模块，至少在 2020 年 12 月之前，它将继续接收 bug 修补程序。 若要详细了解新的 Az 模块和 AzureRM 兼容性，请参阅[新 Azure Powershell Az 模块简介](https://docs.microsoft.com/powershell/azure/new-azureps-module-az?view=azps-3.5.0)。 有关适用于混合 Runbook 辅助角色的 Az 模块安装说明，请参阅安装 [Azure PowerShell 模块](https://docs.microsoft.com/powershell/azure/install-az-ps?view=azps-3.5.0)。

## <a name="update-management-solution"></a>更新管理解决方案

### <a name="can-i-prevent-unexpected-os-level-upgrades"></a>是否可以防止意外进行 OS 级别的升级？

在某些 Linux 分发版中，此类 OS 级别的升级可能会通过包进行。 这可能导致运行更新管理并更改 OS 版本号。 由于更新管理使用相同的方法来更新管理员在 Linux 计算机本地使用的包，因此，此行为是有意设计的。

若要避免通过更新管理部署更新 OS 版本，请使用“排除”功能。

### <a name="why-arent-criticalsecurity-updates-applied"></a>为何不应用关键/安全更新？

将更新部署到 Linux 计算机时，可以选择更新分类。 此选项会筛选符合指定条件的更新。 部署更新时，会在计算机本地应用此筛选器。

由于更新管理在云中执行更新扩充，你可以将更新管理中的某些更新标记为存在安全影响，不过，本地计算机上不会显示该信息。 如果向 Linux 计算机应用关键更新，可能有些更新不会标记为对该计算机具有安全影响，因此不会应用这些更新。 但是，更新管理仍可能报告该计算机不合规，因为其中包含有关更新的其他信息。

按更新分类部署更新在 RTM 版本的 CentOS 上无法正常进行。 若要为 CentOS 正确部署更新，请选择所有分类以确保应用更新。 对于 SUSE，如果首先需要与 zypper（包管理器）或其依赖项相关的安全更新，则仅选择“其他更新”作为分类可能会导致安装其他一些安全更新。 此行为是 zypper 的一项限制。 在某些情况下，可能需要重新运行更新部署，然后通过更新日志验证部署。

### <a name="can-i-deploy-updates-across-azure-tenants"></a>是否可以跨 Azure 租户部署更新？

如果你的计算机需要在向更新管理报告的另一个 Azure 租户中进行修补，则必须使用以下解决方法来计划这些计算机。 可以将 [New-AzAutomationSchedule](https://docs.microsoft.com/powershell/module/Az.Automation/New-AzAutomationSchedule?view=azps-3.7.0) cmdlet 与指定的 `ForUpdateConfiguration` 参数配合使用创建一个计划。 可以使用 [New-AzAutomationSoftwareUpdateConfiguration](https://docs.microsoft.com/powershell/module/Az.Automation/New-AzAutomationSoftwareUpdateConfiguration?view=azps-3.7.0) cmdlet 并将另一租户中的计算机传递到 `NonAzureComputer` 参数。 以下示例演示如何执行此操作。

```azurepowershell
$nonAzurecomputers = @("server-01", "server-02")

$startTime = ([DateTime]::Now).AddMinutes(10)

$sched = New-AzAutomationSchedule -ResourceGroupName mygroup -AutomationAccountName myaccount -Name myupdateconfig -Description test-OneTime -OneTime -StartTime $startTime -ForUpdateConfiguration

New-AzAutomationSoftwareUpdateConfiguration  -ResourceGroupName $rg -AutomationAccountName <automationAccountName> -Schedule $sched -Windows -NonAzureComputer $nonAzurecomputers -Duration (New-TimeSpan -Hours 2) -IncludedUpdateClassification Security,UpdateRollup -ExcludedKbNumber KB01,KB02 -IncludedKbNumber KB100
```

