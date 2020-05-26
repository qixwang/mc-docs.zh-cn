---
title: 使用恢复服务保管库的诊断设置
description: 本文介绍如何使用 Azure 备份服务的新旧诊断事件
author: Johnnytechn
ms.reviewer: srinathv
ms.topic: conceptual
origin.date: 10/30/2019
ms.date: 12/05/2019
ms.author: v-johya
ms.openlocfilehash: 3241fb84d081c3f70da3ed0793e8f1540aad37b2
ms.sourcegitcommit: 08b42258a48d96d754244064d065e4d5703f1cfb
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/18/2020
ms.locfileid: "83445173"
---
# <a name="using-diagnostics-settings-for-recovery-services-vaults"></a>使用恢复服务保管库的诊断设置

Azure 备份发送诊断事件，可以收集这些事件并使用它们来实现分析、警报和报告目的。 

可以通过 Azure 门户配置恢复服务保管库的诊断设置，方法是导航到该保管库并单击“诊断设置”菜单项。  单击“+ 添加诊断设置”可将一个或多个诊断事件发送到存储帐户、事件中心或 Log Analytics (LA) 工作区。 

![诊断设置边栏选项卡](./media/backup-azure-diagnostics-events/diagnostics-settings-blade.png)

## <a name="diagnostics-events-available-for-azure-backup-users"></a>Azure 备份用户可用的诊断事件

Azure 备份提供以下诊断事件，其中每个事件提供有关特定一组备份相关项目的详细数据：
* CoreAzureBackup
* AddonAzureBackupAlerts
* AddonAzureBackupProtectedInstance
* AddonAzureBackupJobs
* AddonAzureBackupPolicy
* AddonAzureBackupStorage 

[Azure 备份诊断事件的数据模型](https://aka.ms/diagnosticsdatamodel)

这些事件的数据可以发送到存储帐户、LA 工作区或事件中心。 如果将此数据发送到 LA 工作区，则需要在“诊断设置”屏幕中选择“特定于资源”开关（请参阅后续部分中的详细信息）。  


<!-- the feature is unavailable -->

## <a name="legacy-event"></a>旧事件

在传统上，保管库的所有与备份相关诊断数据已包含在名为“AzureBackupReport”的单个事件中。 上述六个事件实质上是 AzureBackupReport 中包含的所有数据的分解。 

目前，如果用户仍在对此事件运行自定义查询（例如，生成自定义日志警报、自定义可视化效果等），为了实现向后兼容，我们将继续支持 AzureBackupReport 事件。但是，我们建议对保管库中的所有新诊断设置选择新事件，因为这样可以大幅简化日志查询中数据的处理、更好地发现架构及其结构、改善引入延迟和查询时间方面的性能。 对使用 Azure 诊断模式的支持最终将会分阶段推出，因此，选择新事件可以帮助你在今后避免复杂的迁移。

在迁移所有自定义查询以使用新表中的数据之前，可以选择为 AzureBackupReport 和六个新事件创建单独的诊断设置。 下图显示了采用两项诊断设置的保管库示例。 第一项设置名为 **Setting1**，它以“Azure 诊断”模式将 AzureBackupReport 事件的数据发送到 LA 工作区。 第二项设置名为 **Setting2**，它以“特定于资源”模式将六个新 Azure 备份事件的数据发送到 LA 工作区。

![两项设置](./media/backup-azure-diagnostics-events/two-settings-example.png)

> [!IMPORTANT]
> **仅**在“Azure 诊断”模式下才支持 AzureBackupReport 事件。 **请注意，如果尝试以“特定于资源”模式发送此事件的数据，则不会将任何数据传送到 LA 工作区。**

> [!NOTE]
> 仅当用户选中了“发送到 Log Analytics”时，才会显示“Azure 诊断”/“特定于资源”开关。  若要将数据发送到存储帐户或事件中心，用户只需选择所需的目标并检查任何所需的事件，而无需提供任何其他输入。 同样，建议不要选择旧事件 AzureBackupReport 或更早的事件。

## <a name="users-sending-azure-site-recovery-events-to-log-analytics-la"></a>用户将 Azure Site Recovery 事件发送到 Log Analytics (LA)

Azure 备份和 Azure Site Recovery 事件从同一个恢复服务保管库发送。 由于 Azure Site Recovery 目前尚未加入到“特定于资源”表，对于想要将 Azure Site Recovery 事件发送到 LA 的用户，系统会指示他们**仅**使用“Azure 诊断”模式（参阅下图）。 **为 Azure Site Recovery 事件选择“特定于资源”模式会阻止将所需数据发送到 LA 工作区**。

![Site Recovery 事件](./media/backup-azure-diagnostics-events/site-recovery-settings.png)

上述细节的汇总：

* 如果已使用 Azure 诊断设置了 LA 诊断，并在其顶层编写了自定义查询，请在迁移查询以使用新事件中的数据之前，保持该设置**不变**。
* 如果你还想要加入到新表（我们建议这样做），请创建**新的**诊断设置，选择“特定于资源”，并选择前面指定的六个新事件。 
* 如果你目前正在向 LA 发送 Azure Site Recovery 事件，请**不要**为这些事件选择“特定于资源”模式，否则这些事件的数据将不会传送到 LA 工作区。 应该创建**附加的诊断设置**，选择“Azure 诊断”，然后选择相关的 Azure Site Recovery 事件。 

下图显示了用户对保管库使用三项诊断设置的示例。 第一项设置名为 **Setting1**，它以“Azure 诊断”模式将 AzureBackupReport 事件中的数据发送到 LA 工作区。 第二项设置名为 **Setting2**，它以“特定于资源”模式将六个新 Azure 备份事件中的数据发送到 LA 工作区。 第三项设置名为 **Setting3**，它以“Azure 诊断”模式将 Azure Site Recovery 事件中的数据发送到 LA 工作区。

![三项设置](./media/backup-azure-diagnostics-events/three-settings-example.png)

## <a name="next-steps"></a>后续步骤

[了解诊断事件的 Log Analytics 数据模型](https://aka.ms/diagnosticsdatamodel)
