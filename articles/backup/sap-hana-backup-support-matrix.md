---
title: SAP HANA 备份支持矩阵
description: 本文介绍使用 Azure 备份来备份 Azure VM 上的 SAP HANA 数据库时的支持方案和限制。
author: lingliw
manager: digimobile
ms.topic: conceptual
origin.date: 11/7/2019
ms.date: 12/04/2019
ms.author: v-lingwu
ms.openlocfilehash: 1bf5731ddfc65bb561949e906776590bc2340f2d
ms.sourcegitcommit: c1ba5a62f30ac0a3acb337fb77431de6493e6096
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2020
ms.locfileid: "79292079"
---
# <a name="support-matrix-for-backup-of-sap-hana-databases-on-azure-vms"></a>针对备份 Azure VM 上的 SAP HANA 数据库的支持矩阵

Azure 备份支持将 SAP HANA 数据库备份到 Azure。 本文总结了在使用 Azure 备份来备份 Azure VM 上的 SAP HANA 数据库时的支持方案和限制。

> [!NOTE]
> 日志备份的频率现在可以设置为至少 15 分钟。 日志备份只能在数据库的完整备份成功后开始。

## <a name="scenario-support"></a>方案支持

| **方案**               | **支持的配置**                                | **不支持的配置**                              |
| -------------------------- | ------------------------------------------------------------ | ------------------------------------------------------------ |
| **拓扑**               | 仅在 Azure Linux VM 中运行的 SAP HANA                    | HANA 大型实例 (HLI)                                   |
| **地理区域**                   |  **亚太** – 中国东部、中国北部、中国东部 2、中国北部 2 <BR>  **Azure 政府区域** | 法国南部、德国中部、德国东北部、US Gov 爱荷华州 |
| **OS 版本**            | 带 SP2、SP3 或 SP4 的 SLES 12；带 SP1 的 SLES 15                              | RHEL                                                |
| **HANA 版本**          | SDC on HANA 1.x、MDC on HANA 2.x <= SPS04 Rev 46       | -                                                            |
| **HANA 部署**       | 基于单个 Azure VM 的 SAP HANA - 仅纵向扩展 <br><br> 进行高可用性部署时，两个不同计算机上的两个节点均被视为具有单独数据链的单个节点。               | 横向扩展 <br><br> 在高可用性部署中，备份不会自动故障转移到辅助节点。 应为每个节点单独进行备份配置。                                           |
| **HANA 实例**         | 单个 Azure VM 上的单个 SAP HANA 实例 - 仅纵向扩展 | 单个 VM 上的多个 SAP HANA 实例                  |
| **HANA 数据库类型**    | 1\.x 上的单一数据库容器 (SDC)、2.x 上的多数据库容器 (MDC) | HANA 1.x 中的 MDC                                              |
| **HANA 数据库大小**     | 2 TB 完整备份大小（由 HANA 报告）                   |                                                              |
| **备份类型**           | 完整备份、差异备份和日志备份                          | 增量、快照                                       |
| **还原类型**          | 请参阅 SAP HANA 说明 [1642148](https://launchpad.support.sap.com/#/notes/1642148)，了解支持的还原类型 |                                                              |
| **备份限制**          | 每个 SAP HANA 实例最多可以进行 2 TB 的完整备份         |                                                              |
| **特殊配置** |                                                              | SAP HANA + 动态分层 <br>  通过 LaMa 进行克隆        |

------

> [!NOTE]
> 当前不支持从 SAP HANA 本机客户端（SAP HANA Studio/Cockpit/DBA Cockpit）进行备份和还原操作。

## <a name="next-steps"></a>后续步骤

* 了解如何[备份在 Azure VM 上运行的 SAP HANA 数据库](/backup/backup-azure-sap-hana-database)
* 了解如何[还原在 Azure VM 上运行的 SAP HANA 数据库](/backup/sap-hana-db-restore)
* 了解如何[管理使用 Azure 备份进行备份的 SAP HANA 数据库](sap-hana-db-manage.md)
* 了解[关于 SAP HANA 数据库备份常见问题的疑难解答](/backup/backup-azure-sap-hana-database-troubleshoot)
