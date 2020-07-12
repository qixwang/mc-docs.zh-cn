---
title: Azure 数据资源管理器 API 概述 - Azure 数据资源管理器 | Microsoft Docs
description: 本文概述了 Azure 数据资源管理器中的 Azure 数据资源管理器 API。
services: data-explorer
author: orspod
ms.author: v-tawe
ms.reviewer: rkarlin
ms.service: data-explorer
ms.topic: reference
origin.date: 01/27/2020
ms.date: 07/01/2020
ms.openlocfilehash: 619fefc9d01a4c2babefbc74734f394b01d15a22
ms.sourcegitcommit: 9bc3e55f01e0999f05e7b4ebaea95f3ac91d32eb
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/10/2020
ms.locfileid: "86226260"
---
# <a name="azure-data-explorer-api-overview"></a>Azure 数据资源管理器 API 概述

Azure 数据资源管理器服务支持以下通信终结点：

1. 一个 [REST API](#rest-api) 终结点，你可以通过它查询和管理 Azure 数据资源管理器中的数据。
   此终结点支持用于查询的 [Kusto 查询语言](../query/index.md)以及[控制命令](../management/index.md)。
2. 一个 [MS-TDS](#ms-tds) 终结点，用于实现部分 Microsoft 表格格式数据流 (TDS) 协议，供 Microsoft SQL Server 产品使用。
   此终结点主要供知道如何与 SQL Server 终结点进行查询通信的现有工具使用。
3. 一个 [Azure 资源管理器 (ARM)](https://docs.azure.cn/role-based-access-control/resource-provider-operations#microsoftkusto) 终结点，这是 Azure 服务用来管理资源（例如 Azure 数据资源管理器群集）的标准方法。

Azure 数据资源管理器提供多个客户端库，有了这些库，就可以轻松地使用上述终结点进行编程性访问：

1. .NET SDK
2. Python SDK
3. Java SDK
4. Node SDK
5. Go SDK
6. PowerShell
7. R

## <a name="rest-api"></a>REST API

与任何 Azure 数据资源管理器服务通信时，主要方式是使用服务的 REST API。 通过这个有完全记录的终结点，调用方可以执行以下操作：

1. 查询数据
2. 查询和修改元数据
3. 引入数据
4. 查询服务运行状况
5. 管理资源

不同的 Azure 数据资源管理器服务使用同一个公开提供的 REST API 相互通信。

除了支持使用 REST API 对 Azure 数据资源管理器进行请求外，还提供了许多客户端库，以便使用该服务而无需处理 REST API 协议。

## <a name="ms-tds"></a>MS-TDS

Azure 数据资源管理器还支持使用 Microsoft SQL Server 通信协议 (MS-TDS)，并为运行 T-SQL 查询提供有限支持，替代连接到 Azure 数据资源管理器并查询其数据这种方法。 这样，用户就可以使用众所周知的查询语法 (T-SQL) 和自己熟悉的数据库客户端工具（例如，LINQPad、sqlcmd、Tableau、Excel、Power BI）在 Azure 数据资源管理器上运行查询

可以在[此页](tds/index.md)上找到有关 MS-TDS 的更多详细信息。

## <a name="net-framework-libraries"></a>.NET Framework 库

若要以编程方式调用 Azure 数据资源管理器功能，建议使用 .NET Framework 库。
提供许多不同的库：

- [**Kusto.Data（Kusto 客户端库）** ](./netfx/about-kusto-data.md)，可以用来查询数据、查询元数据以及对其进行更改。
- [**Kusto.Ingest（Kusto 引入库）** ](netfx/about-kusto-ingest.md)，该库使用 Kusto.Data 并对其进行扩展，以方便数据引入。


**Kusto 客户端库** (Kusto.Data) 构建在 Kusto REST API 基础之上，可以将 HTTPS 请求发送到目标 Kusto 群集。 

**Kusto 引入库** (Kusto.Ingest) 使用 Kusto.Data。



上述所有库都使用 Azure API（例如 Azure 存储 API、Azure Active Directory API）。

## <a name="python-libraries"></a>Python 库

Azure 数据资源管理器提供 Python 客户端库，方便调用方发送数据查询和控制命令。

## <a name="r-library"></a>R 库

Azure 数据资源管理器提供 R 客户端库，方便调用方发送数据查询和控制命令。



## <a name="using-azure-data-explorer-from-powershell"></a>通过 PowerShell 使用 Azure 数据资源管理器

Azure 数据资源管理器 .NET Framework 库可供 PowerShell 脚本使用。
[通过 PowerShell 调用 Azure 数据资源管理器](powershell/powershell.md)提供了示例。

## <a name="ide-integration"></a>IDE 集成

`monaco-kusto` 包支持与 Monaco Editor 集成，后者是一个由 Microsoft 开发的 Web 编辑器，是 Visual Studio Code 的基础。
了解有关 [monaco-kusto](monaco/monaco-kusto.md) 包的详细信息。