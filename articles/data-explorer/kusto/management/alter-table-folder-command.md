---
title: .alter table folder - Azure 数据资源管理器 | Microsoft Docs
description: 本文介绍 Azure 数据资源管理器中的 .alter table folder。
services: data-explorer
author: orspod
ms.author: v-tawe
ms.reviewer: rkarlin
ms.service: data-explorer
ms.topic: reference
origin.date: 02/06/2020
ms.date: 07/01/2020
ms.openlocfilehash: 290612245c3704c7854a70ed64f84c0c166181b9
ms.sourcegitcommit: c17e965d4ffd82fd7cd86b2648fcb0053a65df00
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/20/2020
ms.locfileid: "86470499"
---
# <a name="alter-table-folder"></a>.alter table folder

更改现有表的 Folder 值。 

`.alter` `table` TableName `folder` Folder 

> [!NOTE]
> * 需要[数据库管理员权限](../management/access-control/role-based-authorization.md)
> * 还允许最初创建表的[数据库用户](../management/access-control/role-based-authorization.md)对其进行编辑
> * 如果该表不存在，则返回错误。 要了解如何创建新表，请参阅 [.create table](create-table-command.md)

**示例** 

```kusto
.alter table MyTable folder "Updated folder"
```

```kusto
.alter table MyTable folder @"First Level\Second Level"
```