---
title: 连接字符串 - Azure 数据资源管理器 | Microsoft Docs
description: 本文介绍 Azure 数据资源管理器中的连接字符串。
services: data-explorer
author: orspod
ms.author: v-tawe
ms.reviewer: rkarlin
ms.service: data-explorer
ms.topic: reference
origin.date: 10/30/2019
ms.date: 07/01/2020
ms.openlocfilehash: 8a2b50891dbb43817f151ba441361ee53317ab24
ms.sourcegitcommit: 9bc3e55f01e0999f05e7b4ebaea95f3ac91d32eb
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/10/2020
ms.locfileid: "86226265"
---
# <a name="connection-strings"></a>连接字符串

连接字符串广泛用于 Kusto 控制命令、Kusto API，偶然也用于查询。
可以通过连接字符串这种常规的方式来说明如何查找 Kusto 外部的资源（例如 Azure Blob 存储服务和 Azure SQL Database 数据库中的 Blob）并与之交互。

有多种连接字符串格式：

* [Kusto 连接字符串](./kusto.md)说明如何与 Kusto 服务终结点交互。
  Kusto 连接字符串借鉴了 [ADO.NET 连接字符串模型](https://docs.microsoft.com/dotnet/framework/data/adonet/connection-string-syntax)。
* [存储连接字符串](./storage.md)说明如何将 Kusto 指向外部存储服务，例如 Azure Blob 存储和 Azure Data Lake Storage。
* SQL 连接字符串 - 供 Kusto [sql_request 插件](../../query/sqlrequestplugin.md)用来向 Azure DB 服务发出请求，并可供 [export to SQL 命令](../../management/data-export/export-data-to-sql.md)使用。  
  这些连接字符串遵循 [SqlClient 连接字符串](https://docs.microsoft.com/dotnet/framework/data/adonet/connection-string-syntax#sqlclient-connection-strings)规范。