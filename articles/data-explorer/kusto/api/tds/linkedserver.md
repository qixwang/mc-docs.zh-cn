---
title: Kusto 作为 SQL Server 的链接服务器 - Azure 数据资源管理器
description: 本文介绍 Azure 数据资源管理器中作为 SQL Server 的链接服务器的 Kusto。
services: data-explorer
author: orspod
ms.author: v-tawe
ms.reviewer: rkarlin
ms.service: data-explorer
ms.topic: reference
origin.date: 10/30/2019
ms.date: 07/01/2020
ms.openlocfilehash: b29db9493b36be2f77b149d80a0e9027d5a33d79
ms.sourcegitcommit: 9bc3e55f01e0999f05e7b4ebaea95f3ac91d32eb
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/10/2020
ms.locfileid: "86226297"
---
# <a name="kusto-as-a-linked-server-from-the-sql-server"></a>Kusto 作为 SQL Server 的链接服务器

本地 SQL Server 允许你附加链接服务器，并允许你创建查询，将来自 SQL Server 和来自链接服务器的数据联接在一起。

可以通过 ODBC 连接使用 Kusto 作为链接服务器。
SQL Server 本地服务需要使用 Active Directory 帐户（而不是默认服务帐户），以便通过 Azure Active Directory (Azure AD) 连接到 Azure 数据资源管理器。

1. 为 SQL Server 2017（它还附带了 SSMS 18）安装最新的 ODBC 驱动程序： https://www.microsoft.com/download/details.aspx?id=56567
1. 为特定的 Azure 数据资源管理器群集和数据库准备 ODBC 驱动程序的无 DSN 连接字符串：`Driver={ODBC Driver 17 for SQL Server};Server=<cluster>.kusto.chinacloudapi.cn;Database=<database>;Authentication=ActiveDirectoryIntegrated;Language=any@MaxStringSize:4000`。 添加了语言选项以优化 Azure 数据资源管理器，以便将字符串编码为 NVARCHAR(4000)。 有关此解决方法的详细信息，请参阅 [ODBC](./clients.md#odbc)。
1. 使用红色箭头指向的设置创建链接服务器。

:::image type="content" source="../images/linkedserverconnection.png" alt-text="链接服务器连接":::

1. 使用红色箭头指向的设置定义安全性。 

:::image type="content" source="../images/linkedserverlogin.png" alt-text="链接服务器登录":::

若要通过 Kusto 查询数据，请执行以下操作：

```sql
SELECT * FROM OpenQuery(LINKEDSERVER, 'SELECT * FROM <KustoStoredFunction>[(<Parameters>)]')
```

> [!NOTE]
> 1. 使用 Kusto [存储函数](../../query/schema-entities/stored-functions.md)从 Azure 数据资源管理器中提取数据。 存储函数可以包含从 Kusto 进行高效查询所需的所有逻辑，这些逻辑使用本机 [KQL](../../query/index.md) 语言编写，并由指定的参数值控制。 用于调用 Kusto 存储函数的 T-SQL 语法与调用 SQL 表格函数的相同。
> 1. SQL Server 不支持在其自己的查询中从链接服务器调用远程表格函数。 此限制的解决方法是使用 `OpenQuery` 执行链接服务器上的远程查询。 这样，便不会在 SQL Server 目录中调用表格函数，而是在查询（在链接服务器上执行）中调用。 外部 T-SQL 查询可用于联接 SQL Server 上的数据以及通过 `OpenQuery` 从 Kusto 存储函数返回的数据。
