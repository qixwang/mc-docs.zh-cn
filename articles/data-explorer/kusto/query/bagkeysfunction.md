---
title: bag_keys() - Azure 数据资源管理器 | Microsoft Docs
description: 本文介绍 Azure 数据资源管理器中的 bag_keys()。
services: data-explorer
author: orspod
ms.author: v-tawe
ms.reviewer: rkarlin
ms.service: data-explorer
ms.topic: reference
origin.date: 10/23/2018
ms.date: 07/31/2020
ms.openlocfilehash: 00dfab25cfe9cabf88da8896d2b0e12d211463f1
ms.sourcegitcommit: 4e1bc2e9b2a12dbcc05c52db5dbd1ae290aeb18d
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/01/2020
ms.locfileid: "87509150"
---
# <a name="bag_keys"></a>bag_keys()

枚举动态属性包对象中的所有根密钥。

`bag_keys(`*动态对象*`)`

## <a name="returns"></a>返回

密钥的数组，顺序不确定。

## <a name="examples"></a>示例

<!-- csl: https://help.kusto.chinacloudapi.cn/Samples -->
```
datatable(index:long, d:dynamic) [
1, dynamic({'a':'b', 'c':123}), 
2, dynamic({'a':'b', 'c':{'d':123}}),
3, dynamic({'a':'b', 'c':[{'d':123}]}),
4, dynamic(null),
5, dynamic({}),
6, dynamic('a'),
7, dynamic([])]
| extend keys = bag_keys(d)
```

|index|d|密钥|
|---|---|---|
|1|{<br>  "a": "b",<br>  "c":123<br>}|[<br>  "a",<br>  "c"<br>]|
|2|{<br>  "a": "b",<br>  "c": {<br>    "d":123<br>  }<br>}|[<br>  "a",<br>  "c"<br>]|
|3|{<br>  "a": "b",<br>  "c": [<br>    {<br>      "d":123<br>    }<br>  ]<br>}|[<br>  "a",<br>  "c"<br>]|
|4|||
|5|{}|[]|
|6|a||
|7|[]||
