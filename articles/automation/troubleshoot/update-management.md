---
title: 排查 Azure 自动化更新管理的问题
description: 了解如何排查和解决 Azure 自动化中“更新管理”解决方案的问题。
services: automation
author: WenJason
ms.author: v-jay
origin.date: 03/17/2020
ms.date: 05/25/2020
ms.topic: conceptual
ms.service: automation
manager: digimobile
ms.openlocfilehash: c7e31481c1a60b9f172031165515fa968ad4c5fb
ms.sourcegitcommit: 981a75a78f8cf74ab5a76f9e6b0dc5978387be4b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/22/2020
ms.locfileid: "83801896"
---
# <a name="troubleshoot-issues-with-the-update-management-solution"></a>排查“更新管理”解决方案的问题

本文描述了在使用“更新管理”解决方案时可能遇到的问题。 对于混合 Runbook 辅助角色代理，可使用代理故障排除程序来确定底层问题。 若要详细了解该故障排除程序，请参阅[排查 Windows 更新代理问题](update-agent-issues.md)和[排查 Linux 更新代理问题](update-agent-issues-linux.md)。 有关其他加入问题，请参阅[排查解决方案加入问题](onboarding.md)。

>[!NOTE]
>如果在虚拟机 (VM) 上加入解决方案时发现了问题，请检查本地计算机上的“应用程序和服务日志”中的“Operations Manager”日志。  查看事件 ID 为 4502 的事件以及包含 `Microsoft.EnterpriseManagement.HealthService.AzureAutomation.HybridAgent` 的事件详细信息。

>[!NOTE]
>本文进行了更新，以便使用新的 Azure PowerShell Az 模块。 你仍然可以使用 AzureRM 模块，至少在 2020 年 12 月之前，它将继续接收 bug 修补程序。 若要详细了解新的 Az 模块和 AzureRM 兼容性，请参阅[新 Azure Powershell Az 模块简介](https://docs.microsoft.com/powershell/azure/new-azureps-module-az?view=azps-3.5.0)。 有关适用于混合 Runbook 辅助角色的 Az 模块安装说明，请参阅安装 [Azure PowerShell 模块](https://docs.microsoft.com/powershell/azure/install-az-ps?view=azps-3.5.0)。

## <a name="scenario-you-receive-the-error-failed-to-enable-the-update-solution"></a>方案：收到“无法启用更新解决方案”错误

### <a name="issue"></a>问题

尝试在自动化帐户中启用“更新管理”解决方案时收到以下错误：

```error
Error details: Failed to enable the Update solution
```

### <a name="cause"></a>原因

出现该错误的原因可能如下：

* 可能未按要求正确配置 Log Analytics 代理的网络防火墙。 这种情况可能导致代理在解析 DNS URL 时失败。

* 解决方案目标配置不当，且计算机未按预期接收更新。

* 你可能还会注意到，计算机在“合规性”下显示了 `Non-compliant` 状态。 同时，“代理桌面分析”报告代理的状态为 `Disconnected`。

### <a name="resolution"></a>解决方法

* 运行适用于 [Windows](update-agent-issues.md#troubleshoot-offline) 或 [Linux](update-agent-issues-linux.md#troubleshoot-offline) 的故障排除程序，具体取决于你的 OS。

* 参阅[网络配置](../automation-hybrid-runbook-worker.md#network-planning)，了解必须允许哪些地址和端口才能让“更新管理”正常运行。  

* 参阅[网络配置](../../azure-monitor/platform/log-analytics-agent.md#network-firewall-requirements)，了解必须允许哪些地址和端口才能让 Log Analytics 代理正常运行。

* 检查范围配置问题。 [范围配置](../automation-onboard-solutions-from-automation-account.md#scope-configuration)确定要为解决方案配置哪些计算机。 如果你的计算机显示在工作区中但未显示在“更新管理”门户中，则需要将范围配置设置为以这些计算机为目标。 若要了解范围配置，请参阅[在工作区中加入计算机](../automation-onboard-solutions-from-automation-account.md#onboard-machines-in-the-workspace)。

* 按照[删除 Windows 混合 Runbook 辅助角色](../automation-windows-hrw-install.md#remove-windows-hybrid-runbook-worker)或[删除 Linux 混合 Runbook 辅助角色](../automation-linux-hrw-install.md#remove-linux-hybrid-runbook-worker)中的步骤删除辅助角色配置。 

## <a name="scenario-superseded-update-indicated-as-missing-in-update-management"></a>方案：被取代的更新在“更新管理”中指示为缺失

### <a name="issue"></a>问题

对于自动化帐户，旧更新即使已被取代也会显示为缺失。 被取代的更新是你不需要安装的更新，因为有一个较新的更新已可用于更正相同的漏洞。 “更新管理”会忽略被取代的更新并使之不适用，以便使用取代的更新。 如需相关问题的信息，请参阅[更新被取代](https://docs.microsoft.com/windows/deployment/update/windows-update-troubleshooting#the-update-is-not-applicable-to-your-computer)。

### <a name="cause"></a>原因

被取代的更新未正确指示为“已拒绝”以使其被视为不适用。

### <a name="resolution"></a>解决方法

当被取代的更新完全不适用时，应将该更新的批准状态更改为 `Declined`。 若要更改所有更新的批准状态：

1. 在自动化帐户中，选择“更新管理”查看计算机状态。 参阅[查看更新评估](../manage-update-multi.md#view-an-update-assessment)。

2. 检查被取代的更新，确保它完全不适用。 

3. 除非对更新有疑问，否则请将更新标记为已拒绝。 

4. 选择“计算机”，在“合规性”列中强制重新扫描合规性。  参阅[管理多个计算机的更新](../manage-update-multi.md)。

5. 针对其他被取代的更新重复上述步骤。

6. 运行清理向导，以删除已拒绝的更新中的文件。 

7. 对于 Windows Server Update Services (WSUS)，请手动清除所有被取代的更新以刷新基础结构。

8. 定期重复此过程以更正显示问题，并最大程度地减少用于“更新管理”的磁盘空间量。

## <a name="scenario-machines-dont-show-up-in-the-portal-under-update-management"></a><a name="nologs"></a>场景：计算机未显示在门户中的“更新管理”下

### <a name="issue"></a>问题

计算机出现以下症状：

* 计算机在 VM 的“更新管理”视图中显示 `Not configured`。

* Azure 自动化帐户的“更新管理”视图中缺少你的计算机。

* 你的计算机在“合规性”下显示为 `Not assessed`。 在 Azure Monitor 日志中可以看到混合 Runbook 辅助角色的检测信号数据，但却看不到“更新管理”的数据。

### <a name="cause"></a>原因

此问题可能是由本地配置问题或不当的范围配置造成的。 可能的具体原因如下：

* 可能需要重新注册并重新安装混合 Runbook 辅助角色。

* 可能在工作区中定义了配额，现已达到该配额，从而阻止了进一步存储数据。

### <a name="resolution"></a>解决方法

1. 运行适用于 [Windows](update-agent-issues.md#troubleshoot-offline) 或 [Linux](update-agent-issues-linux.md#troubleshoot-offline) 的故障排除程序，具体取决于你的 OS。

2. 请确保你的计算机向正确的工作区报告。 有关如何进行这方面验证的指导，请参阅[验证代理与 Log Analytics 的连接](../../azure-monitor/platform/agent-windows.md#verify-agent-connectivity-to-log-analytics)。 此外，请确保此工作区已链接到你的 Azure 自动化帐户。 若要确认，请转到你的自动化帐户，并选择“相关资源”下的“链接的工作区”。 

3. 确保计算机显示在已链接到你的自动化帐户的 Log Analytics 工作区中。 在 Log Analytics 工作区中运行以下查询。

   ```kusto
   Heartbeat
   | summarize by Computer, Solutions
   ```

4. 如果在查询结果中未看到你的计算机，这表示该计算机最近未签入。 可能存在本地配置问题，应该[重新安装代理](../../azure-monitor/learn/quick-collect-windows-computer.md#install-the-agent-for-windows)。 

5. 如果你的计算机显示在查询结果中，请检查范围配置问题。 [范围配置](../automation-onboard-solutions-from-automation-account.md#scope-configuration)确定要为解决方案配置哪些计算机。 

6. 如果你的计算机显示在工作区中但未显示在“更新管理”中，则必须将范围配置设置为将该计算机作为目标。 若要了解如何执行此操作，请参阅[在工作区中加入计算机](../automation-onboard-solutions-from-automation-account.md#onboard-machines-in-the-workspace)。

7. 在工作区中运行此查询。

   ```kusto
   Operation
   | where OperationCategory == 'Data Collection Status'
   | sort by TimeGenerated desc
   ```

8. 如果收到 `Data collection stopped due to daily limit of free data reached. Ingestion status = OverQuota` 结果，则表示已达到工作区中定义的配额，从而阻止了数据保存。 在工作区中，转到“使用情况和预估成本”下的“数据量管理”，然后更改或删除配额。 

9. 如果问题仍未解决，请按照[部署 Windows 混合 Runbook 辅助角色](../automation-windows-hrw-install.md)中的步骤重新安装适用于 Windows 的混合辅助角色。 对于 Linux，请按照[部署 Linux 混合 Runbook 辅助角色](../automation-linux-hrw-install.md)中的步骤操作。

## <a name="scenario-unable-to-register-automation-resource-provider-for-subscriptions"></a><a name="rp-register"></a>场景：无法为订阅注册自动化资源提供程序

### <a name="issue"></a>问题

在自动化帐户中使用解决方案时发生以下错误：

```error
Error details: Unable to register Automation Resource Provider for subscriptions
```

### <a name="cause"></a>原因

自动化资源提供程序未注册到订阅中。

### <a name="resolution"></a>解决方法

若要注册自动化资源提供程序，请在 Azure 门户中执行以下步骤。

1. 在门户底部的 Azure 服务列表中选择“所有服务”，然后在“常规”服务组中选择“订阅”。 

2. 选择订阅。

3. 在“设置”下，选择“资源提供程序”。 

4. 在资源提供程序列表中，验证 Microsoft.Automation 资源提供程序是否已注册。

5. 如果该资源提供程序未列出，请按照[解决资源提供程序注册错误](/azure-resource-manager/resource-manager-register-provider-errors)中的步骤注册 Microsoft.Automation 提供程序。

## <a name="scenario-scheduled-update-with-a-dynamic-schedule-missed-some-machines"></a><a name="scheduled-update-missed-machines"></a>场景：使用动态计划的计划更新遗漏了某些计算机

### <a name="issue"></a>问题

包含在更新预览中的计算机未全部显示在执行计划的运行期间已修补的计算机的列表中。

### <a name="cause"></a>原因

此问题可能是由下面的某一原因导致的：

* 未为注册的自动化资源提供程序配置动态查询范围内定义的订阅。

* 执行计划时计算机不可用或没有适当的标记。

### <a name="resolution"></a>解决方法

#### <a name="subscriptions-not-configured-for-registered-automation-resource-provider"></a>未为注册的自动化资源提供程序配置订阅

如果未为自动化资源提供程序配置订阅，则无法查询或提取有关该订阅中的计算机的信息。 使用以下步骤验证针对订阅的注册。

1. 在 [Azure 门户](/azure-resource-manager/management/resource-providers-and-types#azure-portal)中访问 Azure 服务列表。

2. 选择“所有服务”，然后在“常规”服务组中选择“订阅”。  

3. 找到在部署范围内定义的订阅。

4. 在“设置”下，选择“资源提供程序”。 

5. 验证是否注册了 Microsoft.Automation 资源提供程序。

6. 如果该资源提供程序未列出，请按照[解决资源提供程序注册错误](/azure-resource-manager/resource-manager-register-provider-errors)中的步骤注册 Microsoft.Automation 提供程序。

#### <a name="machines-not-available-or-not-tagged-correctly-when-schedule-executed"></a>执行计划时计算机不可用或未正确标记

如果为自动化资源提供程序配置了订阅，但运行带有指定[动态组](../automation-update-management-groups.md)的更新计划时遗漏了某些计算机，请使用以下过程。

1. 在 Azure 门户中，打开自动化帐户，然后选择“更新管理”。

2. 检查[“更新管理”历史记录](/automation/manage-update-multi#view-results-of-an-update-deployment)，确定运行更新部署的确切时间。 

3. 对于你怀疑被“更新管理”遗漏的计算机，请使用 Azure Resource Graph (ARG) [查找计算机更改](/governance/resource-graph/how-to/get-resource-changes#find-detected-change-events-and-view-change-details)。 

4. 搜索在运行更新部署之前相当长的一段时间（例如一天）内发生的更改。

5. 检查在此时间段内对计算机进行的任何系统性更改（例如“删除”或“更新”更改）的搜索结果。 这些更改可能会改变计算机状态或标记，导致在部署更新时不会在计算机列表中选择这些计算机。

6. 根据需要调整计算机和资源设置，以更正计算机状态或标记问题。

7. 重新运行更新计划，以确保带有指定动态组的部署包含所有计算机。

## <a name="scenario-expected-machines-dont-appear-in-preview-for-dynamic-group"></a><a name="machines-not-in-preview"></a>场景：所需的计算机未显示在动态组的预览中

### <a name="issue"></a>问题

所选动态组范围内的虚拟机未显示在 Azure 门户预览列表中。 此列表包括针对所选范围的 ARG 查询检索到的所有计算机。 这些范围会被筛选，以便筛选出安装了混合 Runbook 辅助角色并且你有权访问的计算机。 

### <a name="cause"></a>原因
 
此问题的可能原因如下：

* 你在所选范围内没有适当的访问权限。
* ARG 查询未检索到所需的计算机。
* 计算机上未安装混合 Runbook 辅助角色。

### <a name="resolution"></a>解决方法 

#### <a name="incorrect-access-on-selected-scopes"></a>在所选范围内没有适当的访问权限

Azure 门户仅显示你在给定范围内对其拥有写入访问权限的计算机。 如果你在某个范围内没有适当的访问权限，请参阅[教程：使用 RBAC 和 Azure 门户授予用户对 Azure 资源的访问权限](/role-based-access-control/quickstart-assign-role-user-portal)。

#### <a name="arg-query-doesnt-return-expected-machines"></a>ARG 查询未返回所需的计算机

按照以下步骤确定查询是否正常运行。

1. 在 Azure 门户的“Resource Graph 资源管理器”边栏选项卡中，运行一个 ARG 查询，其格式如下所示。 此查询模拟你在“更新管理”中创建动态组时选择的筛选器。 请参阅[在“更新管理”中使用动态组](/automation/automation-update-management-groups)。 

    ```kusto
    where (subscriptionId in~ ("<subscriptionId1>", "<subscriptionId2>") and type =~ "microsoft.compute/virtualmachines" and properties.storageProfile.osDisk.osType == "<Windows/Linux>" and resourceGroup in~ ("<resourceGroupName1>","<resourceGroupName2>") and location in~ ("<location1>","<location2>") )
    | project id, location, name, tags = todynamic(tolower(tostring(tags)))
    | where  (tags[tolower("<tagKey1>")] =~ "<tagValue1>" and tags[tolower("<tagKey2>")] =~ "<tagValue2>") // use this if "All" option selected for tags
    | where  (tags[tolower("<tagKey1>")] =~ "<tagValue1>" or tags[tolower("<tagKey2>")] =~ "<tagValue2>") // use this if "Any" option selected for tags
    | project id, location, name, tags
    ```

   以下是示例：

    ```kusto
    where (subscriptionId in~ ("20780d0a-b422-4213-979b-6c919c91ace1", "af52d412-a347-4bc6-8cb7-4780fbb00490") and type =~ "microsoft.compute/virtualmachines" and properties.storageProfile.osDisk.osType == "Windows" and resourceGroup in~ ("testRG","withinvnet-2020-01-06-10-global-resources-chinaeast2") and location in~ ("chinaeast2","chinanorth","chinanorth2") )
    | project id, location, name, tags = todynamic(tolower(tostring(tags)))
    | where  (tags[tolower("ms-resource-usage")] =~ "azure-cloud-shell" and tags[tolower("temp")] =~ "temp")
    | project id, location, name, tags
    ```
 
2. 检查查询结果中是否列出了你要查找的计算机。 

3. 如果未列出这些计算机，原因可能是在动态组中选择的筛选器存在问题。 请根据需要调整组配置。

#### <a name="hybrid-runbook-worker-not-installed-on-machines"></a>计算机上未安装混合 Runbook 辅助角色

计算机显示在 ARG 查询结果中，但仍未显示在动态组预览中。 对于这种情况，原因可能是未将计算机指定为混合辅助角色，因此无法运行“Azure 自动化”和“更新管理”作业。 若要确保你希望看到的计算机已设置为混合 Runbook 辅助角色：

1. 在 Azure 门户中，转到未正确显示的计算机的自动化帐户。

2. 在“流程自动化” 下选择“混合辅助角色组”。

3. 选择“系统混合辅助角色组”选项卡。

4. 验证该计算机是否存在混合辅助角色。

5. 如果计算机未设置为混合辅助角色，请按照[使用混合 Runbook 辅助角色为数据中心或云中的资源实现自动化](/automation/automation-hybrid-runbook-worker)中的说明进行调整。

6. 将计算机加入混合 Runbook 辅助角色组。

7. 对于未在预览中显示的所有计算机，请重复上述步骤。

## <a name="scenario-components-for-update-management-solution-enabled-while-vm-continues-to-show-as-being-configured"></a><a name="components-enabled-not-working"></a>场景：“更新管理”解决方案的组件已启用，而 VM 持续显示为正在配置

### <a name="issue"></a>问题

在加入 15 分钟后继续在虚拟机上看到以下消息：

```error
The components for the 'Update Management' solution have been enabled, and now this virtual machine is being configured. Please be patient, as this can sometimes take up to 15 minutes.
```

### <a name="cause"></a>原因

出现该错误的原因可能如下：

* 与自动化帐户的通信受到阻止。

* 不同的源计算机 ID 有完全相同的计算机名。 如果具有特定计算机名的 VM 是在不同的资源组中创建的，并且该 VM 向订阅中的同一 Log Analytics 代理工作区报告，则会发生这种情况。

* 要加入的 VM 映像可能来自某个克隆的计算机，但尚未通过系统准备工具 (sysprep) 准备该计算机以使其安装有适用于 Windows 的 Log Analytics 代理。

### <a name="resolution"></a>解决方法

若要帮助确定 VM 出现的具体问题，请在已链接到你的自动化帐户的 Log Analytics 工作区中运行以下查询。

```
Update
| where Computer contains "fillInMachineName"
| project TimeGenerated, Computer, SourceComputerId, Title, UpdateState 
```

#### <a name="communication-with-automation-account-blocked"></a>与自动化帐户的通信受到阻止

参阅[网络规划](../automation-update-management.md#ports)，了解必须允许哪些地址和端口才能使“更新管理”正常工作。

#### <a name="duplicate-computer-name"></a>重复的计算机名

将 VM 重命名，确保其名称在环境中唯一。

#### <a name="onboarded-image-from-cloned-machine"></a>从克隆的计算机加入了映像

如果使用克隆的映像，不同的计算机名将有相同的源计算机 ID。 在这种情况下：

1. 在 Log Analytics 工作区中，从范围配置 `MicrosoftDefaultScopeConfig-Updates` 的已保存搜索中删除 VM（如果已显示）。 已保存的搜索位于工作区的“常规”下。

2. 运行以下 cmdlet。

    ```azurepowershell
    Remove-Item -Path "HKLM:\software\microsoft\hybridrunbookworker" -Recurse -Force
    ```

3. 运行 `Restart-Service HealthService` 以重启运行状况服务。 此操作会重新创建密钥并生成新的 UUID。

4. 如果这种方法不起作用，请先对映像运行 Sysprep，然后安装 MMA。

## <a name="scenario-you-receive-a-linked-subscription-error-when-you-create-an-update-deployment-for-machines-in-another-azure-tenant"></a><a name="multi-tenant"></a>场景：在为另一个 Azure 租户中的计算机创建更新部署时收到链接订阅错误

### <a name="issue"></a>问题

尝试为另一个 Azure 租户中的计算机创建更新部署时遇到以下错误：

```error
The client has permission to perform action 'Microsoft.Compute/virtualMachines/write' on scope '/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/resourceGroupName/providers/Microsoft.Automation/automationAccounts/automationAccountName/softwareUpdateConfigurations/updateDeploymentName', however the current tenant '00000000-0000-0000-0000-000000000000' is not authorized to access linked subscription '00000000-0000-0000-0000-000000000000'.
```

### <a name="cause"></a>原因

如果你创建一个更新部署，其中的 Azure VM 属于另一个租户并且已包含在一个更新部署中，则会发生此错误。

### <a name="resolution"></a>解决方法

使用以下解决方法来计划这些项。 可以将 [New-AzAutomationSchedule](https://docs.microsoft.com/powershell/module/az.automation/new-azautomationschedule?view=azps-3.7.0) cmdlet 与 `ForUpdateConfiguration` 参数配合使用创建一个计划。 然后，使用 [New-AzAutomationSoftwareUpdateConfiguration](https://docs.microsoft.com/powershell/module/Az.Automation/New-AzAutomationSoftwareUpdateConfiguration?view=azps-3.7.0) cmdlet 并将另一租户中的计算机传递到 `NonAzureComputer` 参数。 以下示例介绍如何执行此操作：

```azurepowershell
$nonAzurecomputers = @("server-01", "server-02")

$startTime = ([DateTime]::Now).AddMinutes(10)

$s = New-AzAutomationSchedule -ResourceGroupName mygroup -AutomationAccountName myaccount -Name myupdateconfig -Description test-OneTime -OneTime -StartTime $startTime -ForUpdateConfiguration

New-AzAutomationSoftwareUpdateConfiguration  -ResourceGroupName $rg -AutomationAccountName $aa -Schedule $s -Windows -AzureVMResourceId $azureVMIdsW -NonAzureComputer $nonAzurecomputers -Duration (New-TimeSpan -Hours 2) -IncludedUpdateClassification Security,UpdateRollup -ExcludedKbNumber KB01,KB02 -IncludedKbNumber KB100
```

## <a name="scenario-unexplained-reboots"></a><a name="node-reboots"></a>场景：未予以说明即重启

### <a name="issue"></a>问题

即使已将“重启控制”选项设置为“永不重启”，但在安装更新后，计算机仍然重启。 

### <a name="cause"></a>原因

可以通过多个注册表项来修改 Windows 更新，其中的任何一个都可以修改重启行为。

### <a name="resolution"></a>解决方法

查看[通过编辑注册表配置自动更新](https://docs.microsoft.com/windows/deployment/update/waas-wu-settings#configuring-automatic-updates-by-editing-the-registry)和[用于管理重启的注册表项](https://docs.microsoft.com/windows/deployment/update/waas-restart#registry-keys-used-to-manage-restart)中列出的注册表项，以确保正确配置计算机。

## <a name="scenario-machine-shows-failed-to-start-in-an-update-deployment"></a><a name="failed-to-start"></a>场景：计算机在更新部署中显示“无法启动”

### <a name="issue"></a>问题

计算机显示 `Failed to start` 状态。 查看计算机的具体详细信息时看到以下错误：

```error
Failed to start the runbook. Check the parameters passed. RunbookName Patch-MicrosoftOMSComputer. Exception You have requested to create a runbook job on a hybrid worker group that does not exist.
```

### <a name="cause"></a>原因

存在以下任一原因时，可能出现此错误：

* 计算机不再存在。
* 计算机已关闭且不可访问。
* 计算机出现网络连接问题，因此该计算机上的混合辅助角色不可访问。
* 对 MMA 的某项更新更改了源计算机 ID。
* 如果在自动化帐户中达到了 2000 个并发作业的限制，则更新运行会受到限制。 每个部署被视为一个作业，更新部署中的每台计算机算作一个作业。 当前在自动化帐户中运行的任何其他自动化作业或更新部署都会在计算作业数是否达到限制时计入。

### <a name="resolution"></a>解决方法

如果适用，请为更新部署使用[动态组](../automation-update-management-groups.md)。 此外，还可以执行以下步骤。

1. 验证该计算机是否仍然存在并可访问。 
2. 如果该计算机不存在，请编辑部署并删除该计算机。
3. 查看[网络规划](../automation-update-management.md#ports)部分中列出的“更新管理”所需的端口和地址，然后验证计算机是否满足这些要求。
4. 使用混合 Runbook 辅助角色代理故障排除程序验证与混合 Runbook 辅助角色的连接。 若要了解有关故障排除程序的详细信息，请参阅[排查更新代理问题](update-agent-issues.md)。
5. 在 Log Analytics 中运行以下查询，查找环境中其源计算机 ID 已更改的计算机。 查找 `Computer` 值相同但 `SourceComputerId` 值不同的计算机。

   ```kusto
   Heartbeat | where TimeGenerated > ago(30d) | distinct SourceComputerId, Computer, ComputerIP
   ```

6. 找到受影响的计算机后，编辑针对这些计算机的更新部署，然后删除并重新添加这些部署，使 `SourceComputerId` 反映正确的值。

## <a name="scenario-updates-are-installed-without-a-deployment"></a><a name="updates-nodeployment"></a>场景：在没有部署的情况下安装了更新

### <a name="issue"></a>问题

在“更新管理”中注册 Windows 计算机时，发现在没有部署的情况下安装了更新。

### <a name="cause"></a>原因

在 Windows 上，只要有可用的更新，就会立即自动安装这些更新。 此行为可能会在你未计划将某个更新部署到计算机的情况下导致混淆。

### <a name="resolution"></a>解决方法

`HKEY_LOCAL_MACHINE\SOFTWARE\Policies\Microsoft\Windows\WindowsUpdate\AU` 注册表项默认设置为 4：`auto download and install`。

对于“更新管理”客户端，我们建议将此项设置为 3：`auto download but do not auto install`。

有关详细信息，请参阅[配置自动更新](https://docs.microsoft.com/windows/deployment/update/waas-wu-settings#configure-automatic-updates)。

## <a name="scenario-machine-is-already-registered-to-a-different-account"></a><a name="machine-already-registered"></a>场景：计算机已注册到其他帐户

### <a name="issue"></a>问题

看到以下错误消息：

```error
Unable to Register Machine for Patch Management, Registration Failed with Exception System.InvalidOperationException: {"Message":"Machine is already registered to a different account."}
```

### <a name="cause"></a>原因

计算机已加入到“更新管理”的另一工作区。

### <a name="resolution"></a>解决方法

1. 按照[计算机未显示在门户中的“更新管理”下](#nologs)中的步骤确保计算机向正确的工作区报告。
2. 通过[删除混合 Runbook 组](../automation-windows-hrw-install.md#remove-a-hybrid-worker-group)清理计算机上的项目，然后重试。

## <a name="scenario-machine-cant-communicate-with-the-service"></a><a name="machine-unable-to-communicate"></a>场景：计算机无法与服务通信

### <a name="issue"></a>问题

收到以下错误消息之一：

```error
Unable to Register Machine for Patch Management, Registration Failed with Exception System.Net.Http.HttpRequestException: An error occurred while sending the request. ---> System.Net.WebException: The underlying connection was closed: An unexpected error occurred on a receive. ---> System.ComponentModel.Win32Exception: The client and server can't communicate, because they do not possess a common algorithm
```

```error
Unable to Register Machine for Patch Management, Registration Failed with Exception Newtonsoft.Json.JsonReaderException: Error parsing positive infinity value.
```

```error
The certificate presented by the service <wsid>.oms.opinsights.azure.cn was not issued by a certificate authority used for Microsoft services. Contact your network administrator to see if they are running a proxy that intercepts TLS/SSL communication.
```

```error
Access is denied. (Exception form HRESULT: 0x80070005(E_ACCESSDENIED))
```

### <a name="cause"></a>原因

代理、网关或防火墙可能阻止了网络通信。 

### <a name="resolution"></a>解决方法

检查网络并确保允许适当的端口和地址。 有关“更新管理”和混合 Runbook 辅助角色所需的端口和地址列表，请参阅[网络要求](../automation-hybrid-runbook-worker.md#network-planning)。

## <a name="scenario-unable-to-create-self-signed-certificate"></a><a name="unable-to-create-selfsigned-cert"></a>场景：无法创建自签名证书

### <a name="issue"></a>问题

收到以下错误消息之一：

```error
Unable to Register Machine for Patch Management, Registration Failed with Exception AgentService.HybridRegistration. PowerShell.Certificates.CertificateCreationException: Failed to create a self-signed certificate. ---> System.UnauthorizedAccessException: Access is denied.
```

### <a name="cause"></a>原因

混合 Runbook 辅助角色无法生成自签名证书。

### <a name="resolution"></a>解决方法

验证系统帐户是否对 **C:\ProgramData\Microsoft\Crypto\RSA** 文件夹拥有读取访问权限，然后重试。

## <a name="scenario-the-scheduled-update-failed-with-a-maintenancewindowexceeded-error"></a><a name="mw-exceeded"></a>场景：计划的更新失败并出现 MaintenanceWindowExceeded 错误

### <a name="issue"></a>问题

更新的默认维护时段为 120 分钟。 可将维护时段延长至上限 6 小时，即 360 分钟。

### <a name="resolution"></a>解决方法

编辑任何失败的计划更新部署，并增大维护时段。

有关维护时段的详细信息，请参阅[安装更新](../automation-tutorial-update-management.md#schedule-an-update-deployment)。

## <a name="scenario-machine-shows-as-not-assessed-and-shows-an-hresult-exception"></a><a name="hresult"></a>场景：计算机显示为“未评估”，并显示 HRESULT 异常

### <a name="issue"></a>问题

* 计算机在“合规性”下显示为 `Not assessed`，并且在其下面显示了一条异常消息。
* 门户中出现 HRESULT 错误代码。

### <a name="cause"></a>原因

未正确配置“更新代理”（Windows 上的 Windows 更新代理；Linux 分发版的包管理器）。 “更新管理”依赖于计算机的“更新代理”来提供所需的更新、修补程序的状态，以及部署的修补程序的结果。 如果没有该信息，则“更新管理”无法正确报告所需的或已安装的修补程序。

### <a name="resolution"></a>解决方法

尝试在计算机本地执行更新。 如果此操作失败，则通常意味着出现了更新代理配置错误。

此问题往往是由网络配置和防火墙问题导致的。 使用以下检查来纠正问题。

* 对于 Linux，请查看相应的文档，以确保可以访问包存储库的网络终结点。

* 对于 Windows，请检查[无法从 Intranet 终结点下载更新 (WSUS/SCCM)](https://docs.microsoft.com/windows/deployment/update/windows-update-troubleshooting#updates-arent-downloading-from-the-intranet-endpoint-wsussccm) 中列出的代理配置。

  * 如果为 Windows 更新配置了计算机，请确保可以访问 [HTTP/代理相关的问题](https://docs.microsoft.com/windows/deployment/update/windows-update-troubleshooting#issues-related-to-httpproxy)中所述的终结点。
  * 如果为 Windows Server Update Services (WSUS) 配置了计算机，请确保可以访问 [WUServer 注册表项](https://docs.microsoft.com/windows/deployment/update/waas-wu-settings)配置的 WSUS 服务器。

如果看到 HRESULT，请双击以红色显示的异常，来查看完整的异常消息。 查看下表中可能的解决方法或建议的措施。

|异常  |解决方法或措施  |
|---------|---------|
|`Exception from HRESULT: 0x……C`     | 搜索 [Windows 更新错误代码列表](https://support.microsoft.com/help/938205/windows-update-error-code-list)中的相关错误代码，以查找有关异常原因的更多详细信息。        |
|`0x8024402C`</br>`0x8024401C`</br>`0x8024402F`      | 这表示出现了网络连接问题。 确保计算机与“更新管理”建立了网络连接。 查看[网络规划](../automation-update-management.md#ports)部分中列出的一组所需端口和地址。        |
|`0x8024001E`| 由于服务或系统关闭，未能完成更新操作。|
|`0x8024002E`| 已禁用 Windows 更新服务。|
|`0x8024402C`     | 如果使用 WSUS 服务器，请确保 `HKEY_LOCAL_MACHINE\SOFTWARE\Policies\Microsoft\Windows\WindowsUpdate` 注册表项下的 `WUServer` 和 `WUStatusServer` 的注册表值指定了正确的 WSUS 服务器。        |
|`0x80072EE2`|出现了网络连接问题，或者与已配置的 WSUS 服务器通信时出现了问题。 检查 WSUS 设置并确保可以从客户端访问服务。|
|`The service cannot be started, either because it is disabled or because it has no enabled devices associated with it. (Exception from HRESULT: 0x80070422)`     | 确保 Windows 更新服务 (wuauserv) 正在运行且未禁用。        |
|`0x80070005`| 以下任一原因都可能导致“拒绝访问”错误：<br> 计算机受感染<br> 未正确配置 Windows 更新设置<br> %WinDir%\SoftwareDistribution 文件夹发生文件权限错误<br> 系统驱动器 (C:) 上的磁盘空间不足。
|任何其他一般异常     | 在 Internet 上搜索可能的解决方法，并与本地 IT 支持人员合作。         |

查看 **%Windir%\Windowsupdate.log** 文件也有助于确定可能的原因。 有关如何读取该日志的详细信息，请参阅[如何读取 Windowsupdate.log 文件](https://support.microsoft.com/help/902093/how-to-read-the-windowsupdate-log-file)。

还可以下载并运行 [Windows 更新故障排除程序](https://support.microsoft.com/help/4027322/windows-update-troubleshooter)，以检查计算机上的 Windows 更新是否存在任何问题。

> [!NOTE]
> [Windows 更新故障排除程序](https://support.microsoft.com/help/4027322/windows-update-troubleshooter)文档指出该程序适合在 Windows 客户端上使用，但它也适用于 Windows Server。

## <a name="scenario-update-run-returns-failed-status-linux"></a>方案：更新运行返回“失败”状态 (Linux)

### <a name="issue"></a>问题

更新运行能够启动，但在运行期间遇到错误。

### <a name="cause"></a>原因

可能的原因：

* 包管理器不正常。
* “更新代理”（Windows 的 WUA，Linux 的特定于分发版的包管理器）配置不当。
* 特定的包干扰了基于云的修补。
* 计算机不可访问。
* 更新包含未解决的依赖关系。

### <a name="resolution"></a>解决方法

如果更新运行在成功启动后的运行期间发生失败，请[检查](../manage-update-multi.md#view-results-of-an-update-deployment)运行中受影响的计算机的作业输出。 可能会找到来自你的计算机的具体错误消息，你可以对这些消息进行调查并采取措施。 更新管理要求包管理器正常运行才能成功进行更新部署。

如果在出现特定的修补程序、包或更新之后，紧接着发生作业失败，那么在下一次更新部署中，你可以尝试[排除](../automation-tutorial-update-management.md#schedule-an-update-deployment)这些项。 若要从 Windows 更新收集日志信息，请参阅 [Windows 更新日志文件](https://docs.microsoft.com/windows/deployment/update/windows-update-logs)。

如果无法解决修补问题，请在下一次更新部署开始之前，创建 **/var/opt/microsoft/omsagent/run/automationworker/omsupdatemgmt.log** 文件的副本，并将其保留以用于故障排除。

## <a name="patches-arent-installed"></a>未安装修补程序

### <a name="machines-dont-install-updates"></a>计算机不安装更新

请尝试直接在计算机上运行更新。 如果计算机无法应用更新，请查阅[故障排除指南中的潜在错误列表](/automation/troubleshoot/update-management#hresult)。

如果更新在本地运行，请尝试按照[从“更新管理”中删除 VM](/automation/automation-onboard-solutions-from-browse#clean-up-resources) 中的指导在计算机上删除并重新安装代理。

### <a name="i-know-updates-are-available-but-they-dont-show-as-available-on-my-machines"></a>我知道有可用的更新，但这些更新并未在我的计算机上显示为可用

这通常会在以下情况下发生：已将计算机配置为从 WSUS 或 Microsoft Endpoint Configuration Manager 获取更新，但 WSUS 和 Configuration Manager 尚未批准这些更新。

你可以检查是否已针对 WSUS 和 SCCM 配置计算机，方法是将 `UseWUServer` 注册表项交叉引用到[此文的“通过编辑注册表配置自动更新”部分](https://support.microsoft.com/help/328010/how-to-configure-automatic-updates-by-using-group-policy-or-registry-s)中所述的注册表项。

如果更新未在 WSUS 中获得批准，则不会安装这些更新。 可以运行以下查询来检查 Log Analytics 中是否有未批准的更新。

  ```kusto
  Update | where UpdateState == "Needed" and ApprovalSource == "WSUS" and Approved == "False" | summarize max(TimeGenerated) by Computer, KBID, Title
  ```

### <a name="updates-show-as-installed-but-i-cant-find-them-on-my-machine"></a>更新显示为已安装，但我在计算机上找不到它们

更新通常会被其他更新替代。 有关详细信息，请参阅 Windows 更新故障排除指南中的[更新被替代](https://docs.microsoft.com/windows/deployment/update/windows-update-troubleshooting#the-update-is-not-applicable-to-your-computer)。

### <a name="installing-updates-by-classification-on-linux"></a>按 Linux 上的分类安装更新

按分类（“关键更新和安全更新”）将更新部署到 Linux 有重要的注意事项，尤其是对 CentOS 来说。 [“更新管理”概述页](/automation/automation-update-management#linux-2)上已阐述了这些限制。

### <a name="kb2267602-is-consistently-missing"></a>KB2267602 一直缺失

KB2267602 是 [Windows Defender 定义更新](https://www.microsoft.com/wdsi/definitions)。 它每天更新一次。

## <a name="next-steps"></a>后续步骤

如果你的问题未在本文中列出，或者你无法解决问题，请尝试通过以下渠道之一获取更多支持。

* 通过 [Azure 论坛](https://social.msdn.microsoft.com/Forums/zh-CN/home?forum=windowsazurezhchs)获取 Azure 专家的解答。
* 提出 Azure 支持事件。 请转到 [Azure 支持站点](https://support.azure.cn/zh-cn/support/contact/)以获取支持。
