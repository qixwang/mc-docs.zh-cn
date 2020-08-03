---
title: .alter table docstring - Azure 数据资源管理器
description: 本文介绍 Azure 数据资源管理器中的 `.alter table docstring` 命令。
services: data-explorer
author: orspod
ms.author: v-tawe
ms.reviewer: rkarlin
ms.service: data-explorer
ms.topic: reference
origin.date: 02/04/2020
ms.date: 07/01/2020
ms.openlocfilehash: d745e2d088f0e2eac5fba82f44381402aac34a4a
ms.sourcegitcommit: c17e965d4ffd82fd7cd86b2648fcb0053a65df00
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/20/2020
ms.locfileid: "86470500"
---
# <a name="alter-table-docstring"></a>.alter table docstring

更改现有表的 DocString 值。

`.alter` `table` TableName `docstring` Documentation 

> [!NOTE]
> * 需要[数据库管理员权限](../management/access-control/role-based-authorization.md)
> * 允许最初创建表的[数据库用户](../management/access-control/role-based-authorization.md)对其进行修改
> * 如果该表不存在，则返回错误。 要创建新表，请参阅 [.create table](create-table-command.md)

**示例** 

```kusto
.alter table LyricsAsTable docstring "This is the theme to Garry's show"
```
 
