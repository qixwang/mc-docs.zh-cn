---
title: .show table schema - Azure 数据资源管理器 | Microsoft Docs
description: 本文介绍 Azure 数据资源管理器中的 .show table schema。
services: data-explorer
author: orspod
ms.author: v-tawe
ms.reviewer: rkarlin
ms.service: data-explorer
ms.topic: reference
origin.date: 02/04/2020
ms.date: 07/01/2020
ms.openlocfilehash: 15ff4408201dcbd002f35c9d52ed2ef524d88d3b
ms.sourcegitcommit: c17e965d4ffd82fd7cd86b2648fcb0053a65df00
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/20/2020
ms.locfileid: "86470444"
---
# <a name="show-table-schema"></a>.show table schema

获取要在 create/alter 命令和其他表元数据中使用的架构。

需要[数据库用户权限](../management/access-control/role-based-authorization.md)。

```kusto
.show table TableName cslschema
```

| 输出参数 | 类型   | 说明                                               |
| ---------------- | ------ | --------------------------------------------------------- |
| TableName        | String | 表的名称。                                    |
| 架构           | 字符串 | table schema as 应该用于表创建/更改 |
| DatabaseName     | String | 表所属的数据库                   |
| 文件夹           | String | 表的文件夹                                            |
| DocString        | String | 表的 docstring                                         |

## <a name="show-table-schema-as-json"></a>.show table schema as JSON

获取 JSON 格式的架构和附加的表元数据。

需要[数据库用户权限](../management/access-control/role-based-authorization.md)。

```kusto
.show table TableName schema as json
```

| 输出参数 | 类型   | 说明                             |
| ---------------- | ------ | --------------------------------------- |
| TableName        | String | 表的名称                   |
| 架构           | 字符串 | JSON 格式的表架构         |
| DatabaseName     | String | 表所属的数据库 |
| 文件夹           | String | 表的文件夹                          |
| DocString        | String | 表的 docstring                       |
