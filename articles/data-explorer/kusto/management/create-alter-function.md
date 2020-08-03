---
title: .create-or-alter function - Azure 数据资源管理器
description: 本文介绍 Azure 数据资源管理器中的 .create-or-alter 功能。
services: data-explorer
author: orspod
ms.author: v-tawe
ms.reviewer: rkarlin
ms.service: data-explorer
ms.topic: reference
origin.date: 02/11/2020
ms.date: 07/01/2020
ms.openlocfilehash: 5ee404e0dfcdab1f82b586f71a9d83a1f41f6278
ms.sourcegitcommit: c17e965d4ffd82fd7cd86b2648fcb0053a65df00
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/20/2020
ms.locfileid: "86470364"
---
# <a name="create-or-alter-function"></a>.create-or-alter function

创建存储函数或更改现有函数，并将其存储在数据库元数据中。

```kusto
.create-or-alter function [with (docstring = '<description>', folder='<name>')] [FunctionName] ([paramName:paramType], ...) { CSL-statement }
```

如果在数据库元数据中不存在具有所提供的 FunctionName 的函数，则该命令将创建一个新函数。 否则，将更改该函数。

**示例**

```kusto
.create-or-alter function  with (docstring = 'Demo function with parameter', folder='MyFolder') TestFunction(myLimit:int)
{
    StormEvents | take myLimit
}
```

| 名称         | parameters    | 正文                                | 文件夹   | DocString                    |
| ------------ | ------------- | ----------------------------------- | -------- | ---------------------------- |
| TestFunction | (myLimit:int) | { StormEvents &#124; take myLimit } | MyFolder | 带参数的演示函数 |
