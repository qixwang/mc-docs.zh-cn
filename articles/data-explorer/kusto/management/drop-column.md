---
title: 删除列 - Azure 数据资源管理器 | Microsoft Docs
description: 本文介绍数据资源管理器中的删除列。
services: data-explorer
author: orspod
ms.author: v-tawe
ms.reviewer: rkarlin
ms.service: data-explorer
ms.topic: reference
origin.date: 02/11/2020
ms.date: 07/01/2020
ms.openlocfilehash: 209495620292965421f805bec56e11c07542f26c
ms.sourcegitcommit: c17e965d4ffd82fd7cd86b2648fcb0053a65df00
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/20/2020
ms.locfileid: "86470393"
---
# <a name="drop-column"></a>删除列

从表中删除列。
若要从一个表中删除多个列，请参阅[以下内容](#drop-table-columns)。

> [!WARNING]
> 此命令不可逆。 删除的列中的所有数据都将被删除。
> 将来用于添加该列的命令将无法恢复数据。

**语法**

`.drop` `column` _TableName_ `.` _ColumnName_

## <a name="drop-table-columns"></a>删除表列

从一个表中删除多个列。

> [!WARNING]
> 此命令不可逆。 删除的列中的所有数据都将被删除。
> 将来用于添加这些列的命令将无法还原数据。

**语法**

`.drop` `table` _TableName_ `columns` `(` _Col1_ [`,` *Col2*]... `)`
