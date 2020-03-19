---
title: 关于 Azure VM 中的 SAP HANA 数据库备份
description: 本文介绍如何备份在 Azure 虚拟机上运行的 SAP HANA 数据库。
author: lingliw
manager: digimobile
ms.topic: conceptual
origin.date: 11/7/2019
ms.date: 03/12/2020
ms.author: v-lingwu
ms.openlocfilehash: 071ba118aa91904f45f2640cbe2304c34c5ee1f5
ms.sourcegitcommit: 3c98f52b6ccca469e598d327cd537caab2fde83f
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/13/2020
ms.locfileid: "79291191"
---
# <a name="about-sap-hana-database-backup-in-azure-vms"></a>关于 Azure VM 中的 SAP HANA 数据库备份

SAP HANA 数据库是关键工作负荷，要求较低的恢复点目标 (RPO) 和较快的恢复时间目标 (RTO)。 现在可以使用 [Azure 备份](/backup/backup-overview)来[备份在 Azure VM 上运行的 SAP HANA 数据库](/backup/tutorial-backup-sap-hana-db)。

Azure 备份由 SAP 进行了 [Backint 认证](https://www.sap.com/dmc/exp/2013_09_adpd/enEN/#/d/solutions?id=8f3fd455-a2d7-4086-aa28-51d8870acaa5)，通过利用 SAP HANA 的本机 API 提供本机备份支持。 Azure 备份提供的此产品/服务符合 Azure 备份的**零基础结构**备份的原则，消除了部署和管理备份基础结构的需求。 现在，你可以无缝地备份和还原在 Azure VM 上运行的 SAP HANA 数据库（现在还支持 [M 系列 VM](../virtual-machines/m-series.md)！）并利用 Azure 备份提供的企业管理功能。

## <a name="added-value"></a>附加价值

使用 Azure 备份来备份和还原 SAP HANA 数据库可以提供以下优势：

* **15 分钟恢复点目标 (RPO)** ：现在可以恢复最多 15 分钟的关键数据。
* **一次单击，时间点还原**：将生产数据还原到备用 HANA 服务器变得非常简单。 用于执行还原的备份和目录的链接全部由 Azure 在幕后进行管理。
* **长期保留**：用于满足严格的合规性和审核需求。 基于保留期将你的备份保留多年，超过此时间后，将由内置的生命周期管理功能自动修剪恢复点。
* **Azure 中的备份管理**：使用 Azure 备份的管理和监视功能来改进管理体验。 也支持 Azure CLI。

若要查看目前支持的备份和还原方案，请参阅 [SAP HANA 方案支持矩阵](/backup/sap-hana-backup-support-matrix#scenario-support)。

## <a name="backup-architecture"></a>备份体系结构

![备份体系结构关系图](./media/sap-hana-db-about/backup-architecture.png)

* 备份过程开始时将首先在 Azure 中[创建恢复服务保管库](/backup/tutorial-backup-sap-hana-db#create-a-recovery-service-vault)。 此保管库将用来存储随时间推移创建的备份和恢复点。
* 运行 SAP HANA 服务器的 Azure VM 向保管库进行注册，并且将[发现](/backup/tutorial-backup-sap-hana-db#discover-the-databases)要备份的数据库。 若要使 Azure 备份服务能够发现数据库，必须在 HANA 服务器上以 root 用户身份运行[预注册脚本](https://aka.ms/scriptforpermsonhana)。
* 此脚本在 **hdbuserstore** 中创建 **AZUREWLBACKUPHANAUSER** DB 用户和一个同名的对应键。 若要详细了解该脚本的功能，请参阅[预注册脚本的功能](tutorial-backup-sap-hana-db.md#what-the-pre-registration-script-does)部分。
* Azure 备份服务现在在已注册的 SAP HANA 服务器上安装**适用于 HANA 的 Azure 备份插件**。
* **适用于 HANA 的 Azure 备份插件**使用由预注册脚本创建的 **AZUREWLBACKUPHANAUSER** DB 用户执行所有备份和还原操作。 如果你尝试在不运行此脚本的情况下配置 SAP HANA DB 的备份，可能会收到以下错误：**UserErrorHanaScriptNotRun**。
* 若要在发现的数据库上[配置备份](/backup/tutorial-backup-sap-hana-db#configure-backup)，请选择所需的备份策略并启用备份。

* 配置备份后，Azure 备份服务将在受保护的 SAP HANA 服务器上在数据库级别设置以下 Backint 参数：
  * [catalog_backup_using_backint:true]
  * [enable_accumulated_catalog_backup:false]
  * [parallel_data_backup_backint_channels:1]
  * [log_backup_timeout_s:900)]
  * [backint_response_timeout:7200]

>[!NOTE]
>确保在主机级别不  存在这些参数。 主机级别的参数将替代这些参数，并可能会导致意外行为。
>

* **适用于 HANA 的 Azure 备份插件**维护所有备份计划和策略详细信息。 它触发计划的备份并通过 Backint API 与 **HANA 备份引擎**进行通信。
* **HANA 备份引擎**返回 Backint 流以及要备份的数据。
* 所有计划的备份和按需备份（从 Azure 门户触发的），无论是完整备份还是差异备份，都是由**适用于 HANA 的 Azure 备份插件**启动的。 但是，日志备份由 **HANA 备份引擎**自身进行管理和触发。

## <a name="next-steps"></a>后续步骤

* 了解如何[还原在 Azure VM 上运行的 SAP HANA 数据库](/backup/sap-hana-db-restore)
* 了解如何[管理使用 Azure 备份进行备份的 SAP HANA 数据库](/backup/sap-hana-db-manage)
