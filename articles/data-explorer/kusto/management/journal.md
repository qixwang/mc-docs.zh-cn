---
title: 日志管理 - Azure 数据资源管理器
description: 本文介绍了 Azure 数据资源管理器中的日志管理。
services: data-explorer
author: orspod
ms.author: v-tawe
ms.reviewer: rkarlin
ms.service: data-explorer
ms.topic: reference
origin.date: 08/19/2019
ms.date: 07/01/2020
ms.openlocfilehash: da72e82d602f78e988e08924aa02ff3790f20b07
ms.sourcegitcommit: c17e965d4ffd82fd7cd86b2648fcb0053a65df00
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/20/2020
ms.locfileid: "86470478"
---
# <a name="journal-management"></a>日志管理

`Journal` 包含有关 Azure 数据资源管理器数据库中执行的元数据操作的信息。

元数据操作可由用户执行的控制命令或系统执行的内部控制命令（例如按保留设置放置盘区）产生。

> [!NOTE]
>
> - 包含“添加新盘区”（例如 `.ingest`、`.append`、`.move` 等）的元数据操作不会在 `Journal` 中显示匹配事件。
> - 结果集各列中的数据及其显示格式不是协定的。
>   不建议依赖于它们。

| 事件        | EventTimestamp      | 数据库   | EntityName | UpdatedEntityName | EntityVersion | EntityContainerName |
| ------------ | ------------------- | ---------- | ---------- | ----------------- | ------------- | ------------------- |
| CREATE-TABLE | 2017-01-05 14:25:07 | InternalDb | MyTable1   | MyTable1          | v7.0          | InternalDb          |
| RENAME-TABLE | 2017-01-13 10:30:01 | InternalDb | MyTable1   | MyTable2          | v8.0          | InternalDb          |

| OriginalEntityState                                              | UpdatedEntityState                                               | ChangeCommand                                                                                                                        | 主体                                  |
| ---------------------------------------------------------------- | ---------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------ | ------------------------------------------ |
| .                                                                | 姓名：MyTable1，属性：名称=“[MyTable1].[col1]”，类型=“I32” | .create table MyTable1 (col1:int)                                                                                                    | imike@fabrikam.com                         |
| .                                                                | 数据库属性（太长，无法在此处显示）          | .create database TestDB persist（@“https://imfbkm.blob.core.chinacloudapi.cn/md 、@“https://imfbkm.blob.core.chinacloudapi.cn/data ） | Azure AD 应用 ID=76263cdb-abcd-545644e9c404 |
| 姓名：MyTable1，属性：名称=“[MyTable1].[col1]”，类型=“I32” | 姓名：MyTable2，属性：名称=“[MyTable1].[col1]”，类型=“I32” | .rename table MyTable1 to MyTable2                                                                                                   | rdmik@fabrikam.com                         |

| Item                | 说明                                                           |
| ------------------- | --------------------------------------------------------------------- |
| 事件               | 元数据事件名称                                               |
| EventTimestamp      | 事件时间戳                                                   |
| 数据库            | 事件发生后，此数据库的元数据已更改             |
| EntityName          | 更改之前对其执行操作的实体名称 |
| UpdatedEntityName   | 更改后的新实体名称                                  |
| EntityVersion       | 更改后的新元数据版本（db/群集）            |
| EntityContainerName | 实体容器名称（实体=列，容器=表）            |
| OriginalEntityState | 更改前的实体状态（实体属性）         |
| UpdatedEntityState  | 更改后的新状态                                        |
| ChangeCommand       | 所执行的触发了元数据更改的控制命令       |
| 主体           | 执行该控制命令的主体（用户/应用）            |

## <a name="show-journal"></a>.show journal

`.show journal` 命令返回用户对其具有管理员访问权限的数据库或群集上的元数据更改的列表。

**权限**

所有人（群集访问）都可以执行该命令。

返回的结果将包括：

- 执行该命令的用户的所有日志条目。
- 执行命令的用户对其具有管理员访问权限的数据库的所有日志条目。
- 如果执行该命令的用户是群集管理员，则将返回所有群集日志条目。

## <a name="show-database-_databasename_-journal"></a>.show database DatabaseName 日志

`.show` `database` DatabaseName `journal` 命令返回特定数据库元数据更改的日志。

**权限**

所有人（群集访问）都可以执行该命令。
返回的结果包括：

- 如果执行命令的用户是 DatabaseName 中的数据库管理员，会返回数据库 DatabaseName 的所有日志条目 。
- 否则，将返回数据库 `DatabaseName` 和执行命令的用户的所有日志条目。
