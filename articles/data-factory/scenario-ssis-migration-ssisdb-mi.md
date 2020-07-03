---
title: 将 Azure SQL 托管实例作为数据库工作负荷目标的 SSIS 迁移
description: 将 Azure SQL 托管实例作为数据库工作负荷目标的 SSIS 迁移。
services: data-factory
documentationcenter: ''
author: WenJason
ms.author: v-jay
ms.reviewer: ''
manager: ''
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
origin.date: 9/12/2019
ms.date: 06/29/2020
ms.openlocfilehash: d13c143445d041a1ed598dc36d334aaac6a4e4df
ms.sourcegitcommit: f5484e21fa7c95305af535d5a9722b5ab416683f
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/24/2020
ms.locfileid: "85323296"
---
# <a name="ssis-migration-with-azure-sql-managed-instance-as-the-database-workload-destination"></a>将 Azure SQL 托管实例作为数据库工作负荷目标的 SSIS 迁移

[!INCLUDE[appliesto-adf-xxx-md](includes/appliesto-adf-xxx-md.md)]

将数据库工作负荷从 SQL Server 实例迁移到 Azure SQL 托管实例时，应熟悉 [Azure 数据迁移服务](/dms/dms-overview) (DMS) 和[使用 DMS 进行 SQL 托管实例迁移的网络拓扑](/dms/resource-network-topologies)。

本文重点讨论如何迁移存储在 SSIS 目录 (SSISDB) 中的 SQL Server Integration Service (SSIS) 包以及用于计划 SSIS 包执行的 SQL Server 代理作业。

## <a name="migrate-ssis-catalog-ssisdb"></a>迁移 SSIS 目录 (SSISDB)

SSISDB 迁移可以使用 DMS 来完成，如下文所述：[将 SSIS 包迁移到 SQL 托管实例](/dms/how-to-migrate-ssis-packages-managed-instance)。

## <a name="ssis-jobs-to-sql-managed-instance-agent"></a>将 SSIS 作业迁移到 SQL 托管实例代理

SQL 托管实例有一个一流的本机计划程序，就像本地 SQL Server 代理一样。  由于适合 SSIS 作业的迁移工具尚未发布，因此必须通过脚本/手动复制方式将 SSIS 作业从本地 SQL Server 代理迁移到 SQL 托管实例代理。

## <a name="additional-resources"></a>其他资源

- [Azure 数据工厂](/data-factory/introduction)
- [Azure-SSIS Integration Runtime](/data-factory/create-azure-ssis-integration-runtime)
- [Azure 数据库迁移服务](/dms/dms-overview)
- [使用 DMS 进行 SQL 托管实例迁移的网络拓扑](/dms/resource-network-topologies)
- [将 SSIS 包迁移到 SQL 托管实例](/dms/how-to-migrate-ssis-packages-managed-instance)

## <a name="next-steps"></a>后续步骤

- [连接到 Azure 中的 SSISDB](https://docs.microsoft.com/sql/integration-services/lift-shift/ssis-azure-connect-to-catalog-database)
- [运行部署在 Azure 中的 SSIS 包](https://docs.microsoft.com/sql/integration-services/lift-shift/ssis-azure-run-packages)
