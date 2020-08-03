---
title: Kusto 资源管理器代码重构 - Azure 数据资源管理器 | Microsoft Docs
description: 本文介绍 Azure 数据资源管理器中的 Kusto 资源管理器代码重构。
services: data-explorer
author: orspod
ms.author: v-tawe
ms.reviewer: rkarlin
ms.service: data-explorer
ms.topic: reference
origin.date: 06/05/2019
ms.date: 07/01/2020
ms.openlocfilehash: c5fe119a7d63b352c62f72773f253c57a760837d
ms.sourcegitcommit: c17e965d4ffd82fd7cd86b2648fcb0053a65df00
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/20/2020
ms.locfileid: "86470403"
---
# <a name="kusto-explorer-code-refactoring"></a>Kusto 资源管理器代码重构

与其他 IDE 类似，Kusto.Explorer 为 KQL 查询编辑和重构提供了几个功能。

## <a name="rename-variable-or-column-name"></a>变量名或列名重命名

单击“查询编辑器”窗口中的 `Ctrl`+`R`、`Ctrl`+`R`，可重命名当前选定的符号。

请参阅下面的快照，其中展示了相应体验：

![替换文字](./Images/KustoTools-KustoExplorer/ke-refactor-rename.gif "refactor-rename")

## <a name="extract-scalars-as-let-expressions"></a>将标量作为 `let` 表达式提取

通过单击 `Alt`+`Ctrl`+`M`，可以将当前选定的文本提升为 `let` 表达式。

![替换文字](./Images/KustoTools-KustoExplorer/ke-extract-as-let-literal.gif "extract-as-let-literal")

## <a name="extract-tabular-statements-as-let-expressions"></a>将表格语句提取为 `let` 表达式

还可以通过选择表格表达式的文本，然后单击 `Alt`+`Ctrl`+`M`，将其提升为 `let` 语句。

![替换文字](./Images/KustoTools-KustoExplorer/ke-extract-as-let-tabular.gif "extract-as-let-tabular")
