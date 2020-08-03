---
title: .alter database prettyname - Azure 数据资源管理器
description: 本文介绍 `.alter` 数据库的友好名称命令。
services: data-explorer
author: orspod
ms.author: v-tawe
ms.reviewer: rkarlin
ms.service: data-explorer
ms.topic: reference
origin.date: 02/13/2020
ms.date: 07/01/2020
ms.openlocfilehash: 68dab4641a958b1e57504597eab3b3b07c5119ae
ms.sourcegitcommit: c17e965d4ffd82fd7cd86b2648fcb0053a65df00
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/20/2020
ms.locfileid: "86470517"
---
# <a name="alter-database-prettyname"></a>.alter database prettyname

更改数据库的友好名称。

需要 [DatabaseAdmin 权限](../management/access-control/role-based-authorization.md)。

**语法**

`.alter` `database` *DatabaseName* `prettyname` `'`*DatabasePrettyName*`'`

**返回输出**
 
|输出参数 |类型 |说明 
|---|---|---
|DatabaseName |String |数据库的名称
|PrettyName |String |数据库的友好名称
