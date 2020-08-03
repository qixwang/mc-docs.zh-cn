---
title: 重命名列 - Azure 数据资源管理器 | Microsoft Docs
description: 本文介绍 Azure 数据资源管理器中的 rename column。
services: data-explorer
author: orspod
ms.author: v-tawe
ms.reviewer: rkarlin
ms.service: data-explorer
ms.topic: reference
origin.date: 02/11/2020
ms.date: 07/01/2020
ms.openlocfilehash: 834dab2d0d42361decb8730a7e06126a27acf217
ms.sourcegitcommit: c17e965d4ffd82fd7cd86b2648fcb0053a65df00
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/20/2020
ms.locfileid: "86470335"
---
# <a name="rename-column"></a>rename column

更改现有表列的名称。
若要更改多个列的名称，请参阅[以下内容](#rename-columns)。

**语法**

`.rename` `column` [*DatabaseName* `.`] _TableName_ `.` _ColumnExistingName_ `to` _ColumnNewName_

其中 DatabaseName、TableName、ColumnExistingName 和 ColumnNewName 是相应实体的名称，并遵循[标识符命名规则](../query/schema-entities/entity-names.md)   。

## <a name="rename-columns"></a>重命名列

更改同一个表中的多个现有列的名称。

**语法**

`.rename` `columns` _Col1_ `=` [_DatabaseName_ `.` [*TableName* `.` *Col2*]] `,` ...

该命令可用于交换两列的名称（每列均重命名为另一列的名称。）
