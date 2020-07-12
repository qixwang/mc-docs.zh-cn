---
title: 数据导出 - Azure 数据资源管理器
description: 本文介绍 Azure 数据资源管理器中的数据导出。
services: data-explorer
author: orspod
ms.author: v-tawe
ms.reviewer: rkarlin
ms.service: data-explorer
ms.topic: reference
origin.date: 09/09/2019
ms.date: 07/01/2020
ms.openlocfilehash: 3e45c965f1db53da26de6fb216be604f25925229
ms.sourcegitcommit: 9bc3e55f01e0999f05e7b4ebaea95f3ac91d32eb
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/10/2020
ms.locfileid: "86226201"
---
# <a name="data-export"></a>数据导出

数据导出是运行 Kusto 查询并写入其结果的过程。 稍后检查时可使用这些查询结果。

可以通过多种方法进行数据导出：

## <a name="client-side-export"></a>客户端导出
  最简单的形式就是可在客户端上执行数据导出。 客户端会针对服务运行一个查询，读取结果，然后将结果写入。
这种形式的数据导出依赖客户端工具进行导出，常见于运行该工具的本地文件系统。 支持此模型的工具有 [Kusto.Explorer](../../tools/kusto-explorer.md) 和 [Web UI](../../../web-query-data.md)。

## <a name="service-side-export-pull"></a>服务端导出（拉取）
  如果导出目标是一张表，且此表与查询位于相同或不同的群集/数据库中，则请在目标表上使用“从查询引入”。 在该流中，会运行一个查询，其结果立即引入到表中。 有关详细信息，请参阅[从查询引入](../../management/data-ingestion/ingest-from-query.md)。

## <a name="service-side-export-push"></a>服务端导出（推送）
  上面的[客户端导出](#client-side-export)和[服务端导出（拉取）](#service-side-export-pull)存在局限。 查询结果必须在执行查询的创建者和写入查询结果的使用者之间通过单个网络连接流式处理。
对于可缩放的数据导出，请使用“推送”导出模型，让运行查询的服务也以优化方式写入其结果。 此模型通过一组 `.export` 控制命令公开，它支持将查询结果导出到[外部表](export-data-to-an-external-table.md)、[SQL 表](export-data-to-sql.md)或[外部 Blob 存储](export-data-to-storage.md)。
  
  服务端导出命令受到群集的可用数据导出容量的限制。
可运行 [show capacity 命令](../../management/diagnostics.md#show-capacity)来查看群集的总的、已使用的和剩余的数据导出容量。

## <a name="recommendations-for-secret-management-when-using-data-export-commands"></a>使用数据导出命令时的机密管理建议

理想情况下，将数据导出到远程目标，例如 Azure Blob 存储和 Azure SQL 数据库。 隐式使用执行数据导出命令的安全主体的凭据。 该方法不适用于某些情况。 例如，Azure Blob 存储不支持安全主体的概念，只允许使用其自己的令牌。
此功能支持在数据导出控制命令中以内联方式引入必需的凭据。

若要以安全方式导出：

* 请在发送机密时使用[经过模糊处理的字符串文本](../../query/scalar-data-types/string.md#obfuscated-string-literals)（例如 `h@"..."`）。 这将清理机密，使它们不在内部发出的任何跟踪中显示。

* 请安全地存储密码和类似的机密，并根据需要使用应用程序进行“拉取”。
