---
title: 管理 Azure VM 上的已备份 SAP HANA 数据库
description: 本文介绍了管理和监视在 Azure 虚拟机上运行的 SAP HANA 数据库时执行的常见任务。
author: lingliw
manager: digimobile
ms.topic: conceptual
origin.date: 11/12/2019
ms.date: 03/12/2020
ms.author: v-lingwu
ms.openlocfilehash: 6c743ece971fa1b1a921f0339471ad3e2296777d
ms.sourcegitcommit: 3c98f52b6ccca469e598d327cd537caab2fde83f
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/13/2020
ms.locfileid: "79293131"
---
# <a name="manage-and-monitor-backed-up-sap-hana-databases"></a>管理和监视已备份的 SAP HANA 数据库

本文介绍对 Azure 虚拟机 (VM) 上运行的、已通过 [Azure 备份](/backup/backup-overview)服务备份到 Azure 备份恢复服务保管库的 SAP HANA 数据库进行管理和监视所要执行的常见任务。 其中将会介绍如何监视作业和警报、触发按需备份、编辑策略、停止和恢复数据库保护，以及从备份中取消注册 VM。

如果尚未为 SAP HANA 数据库配置备份，请参阅[备份 Azure VM 上的 SAP HANA 数据库](/backup/backup-azure-sap-hana-database)。

## <a name="monitor-manual-backup-jobs-in-the-portal"></a>在门户中监视手动备份作业

Azure 备份在 Azure 门户上的“备份作业”部分中显示所有手动触发的作业。 

![“备份作业”部分](./media/sap-hana-db-manage/backup-jobs.png)

此门户中显示的作业包括数据库发现和注册，以及备份和还原操作。 此部分中不显示计划的作业，包括日志备份。 此处也不显示从 SAP HANA 本机客户端 (Studio/ Cockpit/ DBA Cockpit) 手动触发的备份。

![“备份作业”列表](./media/sap-hana-db-manage/backup-jobs-list.png)

若要详细了解监视，请参阅[在 Azure 门户中进行监视](/backup/backup-azure-monitoring-built-in-monitor)和[使用 Azure Monitor 进行监视](/backup/backup-azure-monitoring-use-azuremonitor)。

## <a name="view-backup-alerts"></a>查看备份警报

警报是监视 SAP HANA 数据库备份的一种简单方法。 警报可以帮助你集中精力处理最关心的事件，而不会在备份生成的众多事件中迷失方向。 Azure 备份允许你设置警报，可以如下所述来监视警报：

* 登录到 [Azure 门户](https://portal.azure.cn/)。
* 在保管库仪表板中，选择“备份警报”。 

  ![保管库仪表板上的备份警报](./media/sap-hana-db-manage/backup-alerts-dashboard.png)

* 你将能够看到警报：

  ![备份警报的列表](./media/sap-hana-db-manage/backup-alerts-list.png)

* 单击警报以查看更多详细信息：

  ![警报详细信息](./media/sap-hana-db-manage/alert-details.png)

如今，Azure 备份允许通过电子邮件发送警报。 这些警报：

* 只要出现备份失败就会触发。
* 在数据库级别按错误代码进行合并。
* 仅在数据库首次备份失败时发送。

若要详细了解监视，请参阅[在 Azure 门户中进行监视](/backup/backup-azure-monitoring-built-in-monitor)和[使用 Azure Monitor 进行监视](/backup/backup-azure-monitoring-use-azuremonitor)。

## <a name="management-operations"></a>管理操作

Azure 备份通过其支持的大量管理操作，使备份的 SAP HANA 数据库的管理变得很容易。 以下各部分详细讨论了这些更改。

### <a name="run-an-ad-hoc-backup"></a>运行即席备份

备份按照策略计划运行。 可以如下所述运行按需备份：

1. 在保管库菜单中，单击“备份项”  。
2. 在“备份项”  中，选择运行 SAP HANA 数据库的 VM，然后单击“立即备份”  。
3. 在“立即备份”中，使用日历控件选择恢复点的最后保留日期  。  。
4. 监视门户通知。 可以在保管库仪表板 >“备份作业” > “进行中”监视作业进度。   创建初始备份可能需要一些时间，具体取决于你的数据库的大小。

### <a name="run-sap-hana-native-client-backup-on-a-database-with-azure-backup-enabled"></a>在启用了 Azure 备份的数据库上运行 SAP HANA 本机客户端备份

如果要使用 Azure 备份创建正在备份的数据库的本地备份（使用 HANA Studio / Cockpit），请执行以下操作：

1. 等待数据库的所有完整备份或日志备份完成。 在 SAP HANA Studio / Cockpit 中检查状态。
2. 禁用日志备份，并将备份目录设置为相关数据库的文件系统。
3. 为此，请双击“systemdb” > “配置” > “选择数据库” > “筛选器(日志)”。    
4. 将 **enable_auto_log_backup** 设置为 **No**。
5. 将 **log_backup_using_backint** 设置为 **False**。
6. 创建数据库的完整备份。
7. 等待完整备份和目录备份完成。
8. 将前面的设置恢复为 Azure 的设置：
   * 将 **enable_auto_log_backup** 设置为 **Yes**。
   * 将 **log_backup_using_backint** 设置为 **True**。

### <a name="change-policy"></a>更改策略

你可以更改 SAP HANA 备份项的基础策略。

* 在保管库仪表板中，转到“备份项”  ：

  ![选择备份项](./media/sap-hana-db-manage/backup-items.png)

* 选择“Azure VM 中的 SAP HANA” 

  ![选择“Azure VM 中的 SAP HANA”](./media/sap-hana-db-manage/sap-hana-in-azure-vm.png)

* 选择要更改其基础策略的备份项
* 单击现有备份策略

  ![选择现有备份策略](./media/sap-hana-db-manage/existing-backup-policy.png)

* 更改策略，从列表中进行选择。 如果需要，[创建新的备份策略](/backup/backup-azure-sap-hana-database#create-a-backup-policy)。

  ![从下拉列表中选择策略。](./media/sap-hana-db-manage/choose-backup-policy.png)

* 保存更改

  ![保存更改](./media/sap-hana-db-manage/save-changes.png)

* 策略修改将影响所有关联的备份项，并触发相应的**配置保护**作业。

>[!NOTE]
> 在保留期内进行的任何更改将应用到新恢复点，并以追溯方式应用到所有旧恢复点。
>
> 增量备份策略不能用于 SAP HANA 数据库。 对于这些数据库，目前不支持增量备份。

### <a name="stop-protection-for-an-sap-hana-database"></a>停止对 SAP HANA 数据库的保护

可以通过以下几种方式停止保护 SAP HANA 数据库：

* 停止所有将来的备份作业并删除所有恢复点。
* 停止所有将来的备份作业，且恢复点保留不变。

如果选择保留恢复点，请记住以下详细内容：

* 所有恢复点永远保留不变，在停止保护时所有修剪操作都会停止，但会保留数据。
* 需要为受保护的实例和消耗的存储空间付费。 有关详细信息，请参阅 [Azure 备份定价](https://www.azure.cn/pricing/details/backup/)。
* 如果在不停止备份的情况下删除数据源，则新备份将会失败。

停止数据库的保护：

* 在保管库仪表板中，选择“备份项”。 
* 在“备份管理类型”下，选择“Azure VM 中的 SAP HANA”  

  ![选择“Azure VM 中的 SAP HANA”](./media/sap-hana-db-manage/sap-hana-azure-vm.png)

* 选择要停止保护的数据库：

  ![选择要停止保护的数据库](./media/sap-hana-db-manage/select-database.png)

* 在数据库菜单中，选择“停止备份”  。

  ![选择“停止备份”](./media/sap-hana-db-manage/stop-backup.png)

* 在“停止备份”菜单中，选择是要保留还是删除数据。  根据需要提供原因和注释。

  ![选择保留或删除数据](./media/sap-hana-db-manage/retain-backup-data.png)

* 选择“停止备份”。 

### <a name="resume-protection-for-an-sap-hana-database"></a>恢复对 SAP HANA 数据库的保护

停止对 SAP HANA 数据库的保护时，如果选择“保留备份数据”选项，以后可以恢复保护。  如果不保留备份的数据，则将无法恢复保护。

若要恢复对 SAP HANA 数据库的保护，请执行以下操作：

* 打开备份项，并选择“恢复备份”。 

   ![选择恢复备份](./media/sap-hana-db-manage/resume-backup.png)

* 在“备份策略”菜单中选择策略，然后选择“保存”。  

### <a name="upgrading-from-sap-hana-10-to-20"></a>从 SAP HANA 1.0 升级到 2.0

了解[从 SAP HANA 1.0 升级到 2.0 后](backup-azure-sap-hana-database-troubleshoot.md#upgrading-from-sap-hana-10-to-20)，如何继续执行 SAP HANA 数据库的备份。

### <a name="upgrading-without-a-sid-change"></a>在不更改 SID 的情况下升级

了解如何继续执行 [SID 在升级后未更改](backup-azure-sap-hana-database-troubleshoot.md#upgrading-without-an-sid-change)的 SAP HANA 数据库的备份。

### <a name="unregister-an-sap-hana-database"></a>取消注册 SAP HANA 数据库

在禁用保护之后但删除保管库之前取消注册 SAP HANA 实例：

* 在保管库仪表板上，在“管理”下，选择“备份基础结构”。  

   ![选择“备份基础结构”。](./media/sap-hana-db-manage/backup-infrastructure.png)

* 选择“Azure VM 中的工作负荷”作为“备份管理类型”  

   ![选择“Azure VM 中的工作负荷”作为“备份管理类型”](./media/sap-hana-db-manage/backup-management-type.png)

* 在“受保护服务器”中，选择要取消注册的实例。  若要删除保管库，必须取消注册所有服务器/实例。

* 右键单击受保护的实例并选择“取消注册”。 

   ![选择取消注册](./media/sap-hana-db-manage/unregister.png)

## <a name="next-steps"></a>后续步骤

* 了解如何[排查备份 SAP HANA 数据库时的常见问题](/backup/backup-azure-sap-hana-database-troubleshoot)。
