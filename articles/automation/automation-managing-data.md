---
title: 管理 Azure 自动化数据
description: 本文包含有关管理 Azure 自动化环境的多个主题。  Azure 自动化目前包括数据保留和备份 Azure 自动化灾难恢复。
services: automation
ms.subservice: shared-capabilities
origin.date: 03/23/2020
ms.date: 05/11/2020
ms.topic: conceptual
ms.openlocfilehash: 74e1b18b8f00adc8f9e64464bed3aab9bfeb691f
ms.sourcegitcommit: 7443ff038ea8afe511f7419d9c550d27fb642246
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/09/2020
ms.locfileid: "83001610"
---
# <a name="managing-azure-automation-data"></a>管理 Azure 自动化数据

本文包含多个主题，介绍如何管理 Azure 自动化环境中的数据。

## <a name="data-retention"></a>数据保留

在删除 Azure 自动化中的某个资源时，该资源在被永久移除之前会保留多日以供审核。 在此期间，无法查看或使用该资源。 此策略也适用于属于已删除的自动化帐户的资源。

下表汇总了各种资源的保留策略。

| 数据 | 策略 |
|:--- |:--- |
| 帐户 |在用户删除某个帐户 30 天后，系统会永久移除该帐户。 |
| 资产 |在用户删除某个资产 30 天后，或者在用户删除包含该资产的帐户 30 天后，系统会永久移除该资产。 |
| DSC 节点 |在使用 Azure 门户或 Windows PowerShell 中的 [Unregister-AzureRMAutomationDscNode](https://docs.microsoft.com/powershell/module/azurerm.automation/unregister-azurermautomationdscnode) cmdlet 从自动化帐户中取消注册某个 DSC 节点 30 天后，系统会永久移除该节点。 在用户删除包含某个节点的帐户 30 天后，系统也会永久移除该节点。 |
| 作业 |在修改某个作业（例如，完成、停止或暂停该作业）30 天后，系统会删除并永久移除该作业。 |
| 模块 |在用户删除某个模块 30 天后，或者在用户删除包含该模块的帐户 30 天后，系统会永久移除该模块。 |
| 节点配置/MOF 文件 |生成新节点配置 30 天后，系统会永久移除旧节点配置。 |
| 节点报告 |在生成某个节点的新报告 90 天后，系统会永久移除该节点的报告。 |
| Runbook |在用户删除 Runbook 30 天后，或者在用户删除包含该资源的帐户 30 天后，系统会永久移除该资源。 |

保留策略应用于所有用户，且当前无法自定义。 但是，如果需要将数据保留更长一段时间，可以[将 Azure 自动化作业数据转发到 Azure Monitor 日志](automation-manage-send-joblogs-log-analytics.md)。

## <a name="data-backup"></a>数据备份

删除 Azure 中的某个自动化帐户时，会删除该帐户中的所有对象。 这些对象包括 Runbook、模块、配置、设置、作业和资产。 删除帐户后无法将其恢复。 在删除自动化帐户之前，可以参考以下信息来备份该帐户的内容。

### <a name="runbooks"></a>Runbook

可以使用 Azure 门户或 Windows PowerShell 中的 [Get-AzureAutomationRunbookDefinition](https://docs.microsoft.com/powershell/module/servicemanagement/azure/get-azureautomationrunbookdefinition) cmdlet 将 Runbook 导出到脚本文件。 可以根据[在 Azure 自动化中管理 Runbook](manage-runbooks.md) 中所述，将这些脚本文件导入另一个自动化帐户。

### <a name="integration-modules"></a>集成模块

无法从 Azure 自动化导出集成模块。 必须使这些模块可在自动化帐户外部使用。

### <a name="assets"></a>资产

无法导出 Azure 自动化资产：证书、连接、凭据、计划和变量。 可以改用 Azure 门户和 Azure cmdlet 来记下这些资产的详细信息。 然后，使用这些详细信息来创建可供导入到另一自动化帐户的 Runbook 使用的任何资产。

无法使用 cmdlet 检索已加密变量或凭据密码字段的值。 如果不知道这些值，可以在 Runbook 中检索它们。 若要检索变量值，请参阅 [Azure 自动化中的变量资产](shared-resources/variables.md)。 若要详细了解如何检索凭据值，请参阅 [Azure 自动化中的凭据资产](shared-resources/credentials.md)。

 ### <a name="dsc-configurations"></a>DSC 配置

可以使用 Azure 门户或 Windows PowerShell 中的 [Export-AzureRmAutomationDscConfiguration](https://docs.microsoft.com/powershell/module/azurerm.automation/export-azurermautomationdscconfiguration) cmdlet 将 DSC 配置导出到脚本文件。 可以在另一个自动化帐户中导入并使用这些配置。

## <a name="geo-replication-in-azure-automation"></a>Azure 自动化中的异地复制

Azure 自动化帐户中标配了异地复制。 在设置帐户时选择主要区域。 内部自动化异地复制服务会自动将一个次要区域分配到帐户。 然后，该服务会将主要区域中的帐户数据持续备份到次要区域。 在以下文章中可以找到主要区域和次要区域的完整列表：[业务连续性和灾难恢复 (BCDR)：Azure 配对区域](https://docs.microsoft.com/azure/best-practices-availability-paired-regions)。 

自动化异地复制服务创建的备份是自动化资产、配置等的完整副本。 如果主要区域发生故障并丢失数据，则可使用此备份。 如果主要区域发生数据丢失（这种情况很少见），Microsoft 会尝试恢复数据。 公司在无法恢复主要数据的情况下，会使用自动故障转移并通过 Azure 订阅将相关情况告知你。 

如果发生区域性故障，外部客户将无法直接访问自动化异地复制服务。 若要在发生区域性故障期间保留自动化配置和 Runbook，请执行以下操作：

1. 选择要与主要自动化帐户的地理区域配对的次要区域。

2. 在次要区域中创建自动化帐户。

3. 在主要帐户中，将 Runbook 作为脚本文件导出。

4. 将 Runbook 导入到次要区域中的自动化帐户。

## <a name="next-steps"></a>后续步骤

* 若要详细了解 Azure 自动化中的安全资产，请参阅[加密 Azure 自动化中的安全资产](automation-secure-asset-encryption.md)。

* 若要详细了解异地复制，请参阅[创建和使用活动异地复制](/sql-database/sql-database-active-geo-replication)。