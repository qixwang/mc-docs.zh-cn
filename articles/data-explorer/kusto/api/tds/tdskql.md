---
title: 基于 TDS 的 KQL - Azure 数据资源管理器 | Microsoft Docs
description: 本文介绍 Azure 数据资源管理器中基于 TDS 的 KQL。
services: data-explorer
author: orspod
ms.author: v-tawe
ms.reviewer: rkarlin
ms.service: data-explorer
ms.topic: reference
origin.date: 03/09/2019
ms.date: 07/01/2020
ms.openlocfilehash: 579c51590baef6cb7e5139f1693de69efe3bb151
ms.sourcegitcommit: 9bc3e55f01e0999f05e7b4ebaea95f3ac91d32eb
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/10/2020
ms.locfileid: "86226291"
---
# <a name="kql-over-tds"></a>基于 TDS 的 KQL

Kusto 使 TDS 终结点能够执行以本机 [KQL](../../query/index.md) 查询语言编写的查询。 利用此功能，可以更顺利地向 Kusto 迁移。 例如，可以创建 SSIS 作业以使用 KQL 查询来查询 Kusto。

## <a name="executing-kusto-stored-functions"></a>执行 Kusto 存储函数

Kusto 允许运行[存储函数](../../query/schema-entities/stored-functions.md)，就像调用 SQL 存储过程一样。

例如，存储函数 MyFunction：

|名称 |parameters|正文|文件夹|DocString
|---|---|---|---|---
|MyFunction |(myLimit: long)| {StormEvents &#124; limit myLimit}|MyFolder|带参数的演示函数||

可以这样调用：

```csharp
  using (var connection = new SqlConnection(csb.ToString()))
  {
    await connection.OpenAsync();
    using (var command = new SqlCommand("kusto.MyFunction", connection))
    {
      command.CommandType = CommandType.StoredProcedure;
      var parameter = new SqlParameter("mylimit", SqlDbType.Int);
      command.Parameters.Add(parameter);
      parameter.Value = 3;
      using (var reader = await command.ExecuteReaderAsync())
      {
        // Read the response.
      }
    }
  }
```

> [!NOTE]
> 使用名为 `kusto` 的显式架构调用存储函数，以区分 Kusto 存储函数和模拟的 SQL 系统存储过程。

还可以从 T-SQL 调用 Kusto 存储函数，就像 SQL 表格函数一样：

```sql
SELECT * FROM kusto.MyFunction(10)
```

创建优化的 KQL 查询并将其封装在存储函数中，尽量减少 T-SQL 查询代码。

## <a name="executing-kql-query"></a>执行 KQL 查询

存储过程 `sp_execute_kql` 执行 [KQL](../../query/index.md) 查询（包括参数化查询）。 此过程类似于 SQL Server `sp_executesql`。

`sp_execute_kql` 的第一个参数是 KQL 查询。 可以引入其他参数，它们的作用类似于[查询参数](../../query/queryparametersstatement.md)。

例如：

```csharp
  using (var connection = new SqlConnection(csb.ToString()))
  {
    await connection.OpenAsync();
    using (var command = new SqlCommand("sp_execute_kql", connection))
    {
      command.CommandType = CommandType.StoredProcedure;
      var query = new SqlParameter("@kql_query", SqlDbType.NVarChar);
      command.Parameters.Add(query);
      var parameter = new SqlParameter("mylimit", SqlDbType.Int);
      command.Parameters.Add(parameter);
      query.Value = "StormEvents | limit myLimit";
      parameter.Value = 3;
      using (var reader = await command.ExecuteReaderAsync())
      {
        // Read the response.
      }
    }
  }
```

> [!NOTE]
> 通过 TDS 调用时无需声明参数，因为参数类型通过协议进行设置。
