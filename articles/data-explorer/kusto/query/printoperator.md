---
title: print 运算符 - Azure 数据资源管理器
description: 本文介绍 Azure 数据资源管理器中的 print 运算符。
services: data-explorer
author: orspod
ms.author: v-tawe
ms.reviewer: rkarlin
ms.service: data-explorer
ms.topic: reference
origin.date: 03/16/2019
ms.date: 08/06/2020
ms.openlocfilehash: dd17c94d6993c517dd1f136f37025415117396f7
ms.sourcegitcommit: 7ceeca89c0f0057610d998b64c000a2bb0a57285
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/06/2020
ms.locfileid: "87841666"
---
# <a name="print-operator"></a>print 运算符

输出单个行，其中包含一个或多个标量表达式。

<!-- csl: https://help.kusto.chinacloudapi.cn:443/Samples -->
```kusto
print x=1, s=strcat("Hello", ", ", "World!")
```

**语法**

`print` [*ColumnName* `=`] *ScalarExpression* [',' ...]

**参数**

* ColumnName：要分配给输出的单数列的选项名称。
* ScalarExpression：要计算的标量表达式。

**返回**

一个单列单行表，其单个单元格的值为已计算的 ScalarExpression。

**示例**

`print` 运算符非常有用，可以快速计算一个或多个标量表达式，并用生成的值创建一个单行表。
例如：

<!-- csl: https://help.kusto.chinacloudapi.cn:443/Samples -->
```kusto
print 0 + 1 + 2 + 3 + 4 + 5, x = "Wow!"
```
<!-- csl: https://help.kusto.chinacloudapi.cn:443/Samples -->
```kusto
print banner=strcat("Hello", ", ", "World!")
```
