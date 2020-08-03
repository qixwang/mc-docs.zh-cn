---
title: alter-merge table column-docstrings - Azure 数据资源管理器 | Microsoft Docs
description: 本文介绍 Azure 数据资源管理器中的 alter-merge table column-docstrings。
services: data-explorer
author: orspod
ms.author: v-tawe
ms.reviewer: rkarlin
ms.service: data-explorer
ms.topic: reference
origin.date: 02/13/2020
ms.date: 07/01/2020
ms.openlocfilehash: 5698a27f69bbef78cef1bcb271f712a1c247072a
ms.sourcegitcommit: c17e965d4ffd82fd7cd86b2648fcb0053a65df00
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/20/2020
ms.locfileid: "86470505"
---
# <a name="alter-merge-table-column-docstrings"></a>alter-merge table column-docstrings

设置指定表的一个或多个列的 `docstring` 属性。 未显式设置的列保留此属性的现有值（如果有）。

有关 alter table column-docstring 的信息，请参阅 [下方内容](#alter-table-column-docstrings)。

**语法**

`.alter-merge` `table` *TableName* `column-docstring` `(` *Col1* `:` *Docstring1* [`,` *Col2* `:` *Docstring2*]... `)`

**示例** 

```kusto
.alter-merge table Table1 column-docstrings (Column1:"DocString1", Column2:"DocString2")
```

## <a name="alter-table-column-docstrings"></a>alter table column-docstrings

设置指定表的一个或多个列的 `docstring` 属性。 未显式设置的列将删除此属性。

**语法**

`.alter` `table` *TableName* `column-docstring` `(` *Col1* `:` *Docstring1* [`,` *Col2* `:` *Docstring2*]... `)`

**示例** 

```kusto
.alter table Table1 column-docstrings (Column1:"DocString1", Column2:"DocString2")
```
