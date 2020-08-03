---
title: .drop function - Azure 数据资源管理器 | Microsoft Docs
description: 本文介绍 Azure 数据资源管理器中的 .drop function。
services: data-explorer
author: orspod
ms.author: v-tawe
ms.reviewer: rkarlin
ms.service: data-explorer
ms.topic: reference
origin.date: 02/13/2020
ms.date: 07/01/2020
ms.openlocfilehash: c406c4165b1e244c87a0312c1e002a73e078b4ef
ms.sourcegitcommit: c17e965d4ffd82fd7cd86b2648fcb0053a65df00
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/20/2020
ms.locfileid: "86470392"
---
# <a name="drop-function"></a>.drop function

删除数据库中的某个函数。
若要删除数据库中的多个函数，请参阅 [.drop functions](#drop-functions)。

**语法**

`.drop` `function` _FunctionName_ [`ifexists`]

- `ifexists`：如果已指定，则更改命令的行为，以便不会因不存在的函数而失败。

> [!NOTE]
>
> - 需要[数据库管理员权限](../management/access-control/role-based-authorization.md)。
> - 允许最初创建该函数的[数据库用户](../management/access-control/role-based-authorization.md)修改函数。

| 输出参数 | 类型   | 说明                               |
| ---------------- | ------ | ----------------------------------------- |
| 名称             | String | 删除的函数的名称 |

**示例**

```kusto
.drop function MyFunction1
```

## <a name="drop-functions"></a>.drop functions

删除数据库中的多个函数。

**语法**

`.drop` `functions` (_FunctionName1_, _FunctionName2_,..) [ifexists]

**返回**

此命令返回数据库中剩余函数的列表。

| 输出参数 | 类型   | 说明                                                                                                    |
| ---------------- | ------ | -------------------------------------------------------------------------------------------------------------- |
| 名称             | String | 函数的名称。                                                                                      |
| parameters       | String | 函数所需的参数。                                                                       |
| 正文             | String | （零个或多个）`let` 语句，后跟有效的 CSL 表达式，该表达式在函数调用时求值。 |
| 文件夹           | String | 用于 UI 函数分类的文件夹。 此参数不会更改调用函数的方式。  |
| DocString        | String | 用于 UI 目的的函数说明。                                                                 |

需要[函数管理员权限](../management/access-control/role-based-authorization.md)。

**示例**

```kusto
.drop functions (Function1, Function2, Function3) ifexists
```
