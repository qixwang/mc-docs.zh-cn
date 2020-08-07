---
title: SAP HANA 备份支持矩阵
description: 本文介绍使用 Azure 备份来备份 Azure VM 上的 SAP HANA 数据库时的支持方案和限制。
author: Johnnytechn
ms.topic: conceptual
origin.date: 11/7/2019
ms.date: 07/31/2020
ms.author: v-johya
ms.openlocfilehash: aca31128a7fcc56cfe656ab435b9ac018e8c637f
ms.sourcegitcommit: b5794af488a336d84ee586965dabd6f45fd5ec6d
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/01/2020
ms.locfileid: "87508464"
---
# <a name="support-matrix-for-backup-of-sap-hana-databases-on-azure-vms"></a>针对备份 Azure VM 上的 SAP HANA 数据库的支持矩阵

Azure 备份支持将 SAP HANA 数据库备份到 Azure。 本文总结了在使用 Azure 备份来备份 Azure VM 上的 SAP HANA 数据库时的支持方案和限制。

> [!NOTE]
> 日志备份的频率现在可以设置为至少 15 分钟。 日志备份只能在数据库的完整备份成功后开始。

## <a name="scenario-support"></a>方案支持

| **方案**               | **支持的配置**                                | **不支持的配置**                              |
| -------------------------- | ------------------------------------------------------------ | ------------------------------------------------------------ |
| **拓扑**               | 仅在 Azure Linux VM 中运行的 SAP HANA                    | HANA 大型实例 (HLI)                                   |
| **区域**                   |  中国东部、中国北部、中国东部 2、中国北部 2  |
| **OS 版本**            | 带 SP2、SP3 和 SP4 的 SLES 12；带 SP0 和 SP1 的 SLES 15 <br><br>   **预览版** - RHEL 7.4、7.6、7.7 和 8.1  <br>     RHEL（7.4、7.6、7.7 和 8.1）的 SAP HANA 备份预览版[入门](/backup/tutorial-backup-sap-hana-db)。 如需深入咨询，请通过 [AskAzureBackupTeam@microsoft.com](mailto:AskAzureBackupTeam@microsoft.com) 联系我们。                |                                             |
| **HANA 版本**          | HANA 1.x 上的 SDC、HANA 2.x 上的 MDC <= SPS04 Rev 48       | -                                                            |
| **HANA 部署**       | 基于单个 Azure VM 的 SAP HANA - 仅纵向扩展 <br><br> 进行高可用性部署时，两个不同计算机上的两个节点均被视为具有单独数据链的单个节点。               | 横向扩展 <br><br> 在高可用性部署中，备份不会自动故障转移到辅助节点。 应为每个节点单独进行备份配置。                                           |
| **HANA 实例**         | 单个 Azure VM 上的单个 SAP HANA 实例 - 仅纵向扩展 | 单个 VM 上的多个 SAP HANA 实例                  |
| **HANA 数据库类型**    | 1\.x 上的单一数据库容器 (SDC)、2.x 上的多数据库容器 (MDC) | HANA 1.x 中的 MDC                                              |
| **HANA 数据库大小**     | 大小 <= 2 TB 的 HANA 数据库（这不是 HANA 系统的内存大小）               |                                                              |
| **备份类型**           | 完整备份、差异备份和日志备份                          | 增量、快照                                       |
| **还原类型**          | 请参阅 SAP HANA 说明 [1642148](https://launchpad.support.sap.com/#/notes/1642148)，了解支持的还原类型 |                                                              |
| **备份限制**          | 每个 SAP HANA 实例最多可以进行 2 TB 的完整备份         |                                                              |
| **特殊配置** |                                                              | SAP HANA + 动态分层 <br>  通过 LaMa 进行克隆        |

------

>[!NOTE]
>备份在 Azure VM 中运行的 SAP HANA 数据库时，Azure 备份不会针对夏令时更改自动进行调整。
>
>请根据需要手动修改策略。


> [!NOTE]
> 现在可以在 Azure 门户中[监视到同一计算机的备份和还原作业](/backup/sap-hana-db-manage#monitor-manual-backup-jobs-in-the-portal)，这些作业是从 HANA 原生客户端 (SAP HANA Studio/Cockpit/DBA Cockpit) 触发的。

## <a name="next-steps"></a>后续步骤

* 了解如何[备份在 Azure VM 上运行的 SAP HANA 数据库](/backup/backup-azure-sap-hana-database)
* 了解如何[还原在 Azure VM 上运行的 SAP HANA 数据库](/backup/sap-hana-db-restore)
* 了解如何[管理使用 Azure 备份进行备份的 SAP HANA 数据库](sap-hana-db-manage.md)
* 了解[关于 SAP HANA 数据库备份常见问题的疑难解答](/backup/backup-azure-sap-hana-database-troubleshoot)

