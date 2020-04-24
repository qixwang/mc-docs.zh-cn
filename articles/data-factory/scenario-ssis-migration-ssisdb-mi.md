---
title: 以 Azure SQL 数据库托管实例为数据库工作负荷目标的 SSIS 迁移 | Microsoft Docs
description: 以 Azure SQL 数据库托管实例为数据库工作负荷目标的 SSIS 迁移。
services: data-factory
documentationcenter: ''
author: WenJason
ms.author: v-jay
ms.reviewer: ''
manager: ''
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
origin.date: 9/12/2019
ms.date: 10/14/2019
ms.openlocfilehash: d0b5d3006e45e536d32039bb118b6796bc3fe2e3
ms.sourcegitcommit: c1ba5a62f30ac0a3acb337fb77431de6493e6096
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2020
ms.locfileid: "72275882"
---
# <a name="ssis-migration-with-azure-sql-database-managed-instance-as-the-database-workload-destination"></a>以 Azure SQL 数据库托管实例为数据库工作负荷目标的 SSIS 迁移

将数据库工作负荷从本地 SQL Server 迁移到 Azure SQL 数据库托管实例时，应熟悉 [Azure 数据迁移服务](/dms/dms-overview) (DMS) 和[使用 DMS 进行的 Azure SQL 数据库托管实例迁移的网络拓扑](/dms/resource-network-topologies)。

本文重点讨论如何迁移存储在 SSIS 目录 (SSISDB) 中的 SQL Server Integration Service (SSIS) 包以及用于计划 SSIS 包执行的 SQL Server 代理作业。

## <a name="migrate-ssis-catalog-ssisdb"></a>迁移 SSIS 目录 (SSISDB)

SSISDB 迁移可以使用 DMS 来完成，如下文所述：[将 SSIS 包迁移到 Azure SQL 数据库托管实例](/dms/how-to-migrate-ssis-packages-managed-instance)。

## <a name="ssis-jobs-to-azure-sql-database-managed-instance-agent"></a>从 SSIS 作业到 Azure SQL 数据库托管实例代理

Azure SQL 数据库托管实例有一个一流的本机计划程序，就像本地 SQL Server 代理一样。  由于适合 SSIS 作业的迁移工具尚未发布，因此必须通过脚本/手动复制方式将它们从本地 SQL Server 代理迁移到 Azure SQL 数据库托管实例代理。

## <a name="additional-resources"></a>其他资源

- [Azure 数据工厂](/data-factory/introduction)
- [Azure-SSIS Integration Runtime](/data-factory/create-azure-ssis-integration-runtime)
- [Azure 数据库迁移服务](/dms/dms-overview)
- [使用 DMS 进行的 Azure SQL 数据库托管实例迁移的网络拓扑](/dms/resource-network-topologies)
- [将 SSIS 包迁移到 Azure SQL 数据库托管实例](/dms/how-to-migrate-ssis-packages-managed-instance)

## <a name="next-steps"></a>后续步骤

- [连接到 Azure 中的 SSISDB](https://docs.microsoft.com/sql/integration-services/lift-shift/ssis-azure-connect-to-catalog-database)
- [运行部署在 Azure 中的 SSIS 包](https://docs.microsoft.com/sql/integration-services/lift-shift/ssis-azure-run-packages)
