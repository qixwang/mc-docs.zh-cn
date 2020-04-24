---
title: 管理和监视 Azure SQL 数据仓库中的工作负荷重要性 | Microsoft Docs
description: 了解如何管理和监视请求级别重要性。
services: sql-data-warehouse
author: WenJason
manager: digimobile
ms.service: sql-data-warehouse
ms.subservice: workload-management
ms.topic: conceptual
origin.date: 05/20/2019
ms.date: 08/19/2019
ms.author: v-jay
ms.reviewer: igorstan
ms.openlocfilehash: b920ed912f2acd49cc537af5141140ec0320e898
ms.sourcegitcommit: c1ba5a62f30ac0a3acb337fb77431de6493e6096
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2020
ms.locfileid: "69544863"
---
# <a name="manage-and-monitor-workload-importance-in-azure-sql-data-warehouse"></a>管理和监视 Azure SQL 数据仓库中的工作负荷重要性

使用 DMV 和目录视图管理和监视 Azure SQL 数据仓库中的请求级别重要性。

## <a name="monitor-importance"></a>监视重要性

使用 [sys.dm_pdw_exec_requests](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/sys-dm-pdw-exec-requests-transact-sql?view=azure-sqldw-latest) 动态管理视图中新的“重要性”列监视重要性。
以下监视查询显示了查询的提交时间和开始时间。 查看提交时间和开始时间以及重要性，以了解重要性如何影响计划。

```sql
SELECT s.login_name, r.status, r.importance, r.submit_time, r.start_time
  FROM sys.dm_pdw_exec_sessions s
  JOIN sys.dm_pdw_exec_requests r ON s.session_id = r.session_id
  WHERE r.resource_class is not null
ORDER BY r.start_time
```

若要进一步了解如何计划查询，请使用目录视图。

## <a name="manage-importance-with-catalog-views"></a>使用目录视图管理重要性

sys.workload_management_workload_classifiers 目录视图包含有关 Azure SQL 数据仓库实例中分类器的信息。 若要排除映射到资源类的系统定义的分类器，请执行以下代码：

```sql
SELECT *
  FROM sys.workload_management_workload_classifiers
  WHERE classifier_id > 12
```

目录视图 [sys.workload_management_workload_classifier_details](https://docs.microsoft.com/sql/relational-databases/system-catalog-views/sys-workload-management-workload-classifier-details-transact-sql?view=azure-sqldw-latest) 包含有关创建分类器时使用的参数的信息。  以下查询显示 ExecReportsClassifier 是使用 ```membername``` 参数（值为 ExecutiveReports）创建的：

```sql
SELECT c.name,cd.classifier_type, classifier_value
  FROM sys.workload_management_workload_classifiers c
  JOIN sys.workload_management_workload_classifier_details cd
    ON cd.classifier_id = c.classifier_id
  WHERE c.name = 'ExecReportsClassifier'
```

![查询结果](./media/sql-data-warehouse-how-to-manage-and-monitor-workload-importance/wlm-query-results.png)

为了简化分类错误的排查，我们建议在创建工作负荷分类器时删除资源类角色映射。 以下代码返回现有的资源类角色成员身份。 对从相应资源类返回的每个 ```membername``` 运行 sp_droprolemember。
以下是在删除工作负荷分类器之前检查是否存在的示例：

```sql
IF EXISTS (SELECT 1 FROM sys.workload_management_workload_classifiers WHERE name = 'ExecReportsClassifier')
  DROP WORKLOAD CLASSIFIER ExecReportsClassifier;
GO
```

## <a name="next-steps"></a>后续步骤
- 有关分类的详细信息，请参阅[工作负荷分类](sql-data-warehouse-workload-classification.md)。
- 有关重要性的详细信息，请参阅[工作负荷重要性](sql-data-warehouse-workload-importance.md)

> [!div class="nextstepaction"]
> [开始配置工作负荷重要性](sql-data-warehouse-how-to-configure-workload-importance.md)
