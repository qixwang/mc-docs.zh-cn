---
title: pack_array() - Azure 数据资源管理器
description: 本文介绍 Azure 数据资源管理器中的 pack_array()。
services: data-explorer
author: orspod
ms.author: v-tawe
ms.reviewer: rkarlin
ms.service: data-explorer
ms.topic: reference
origin.date: 10/23/2018
ms.date: 08/06/2020
ms.openlocfilehash: 1bfc7de9aea7abc38942d24a990e453aee7742c4
ms.sourcegitcommit: 7ceeca89c0f0057610d998b64c000a2bb0a57285
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/06/2020
ms.locfileid: "87841723"
---
# <a name="pack_array"></a>pack_array()

将所有输入值打包为动态数组。

**语法**

`pack_array(`*Expr1*`[`,` *Expr2*]`)`

**参数**

* Expr1...N：要打包到动态数组中的输入表达式。

**返回**

动态数组，其中包含 Expr1、Expr2、...、ExprN 的值。

**示例**

<!-- csl: https://help.kusto.chinacloudapi.cn:443/Samples -->
```kusto
range x from 1 to 3 step 1
| extend y = x * 2
| extend z = y * 2
| project pack_array(x,y,z)
```

|Column1|
|---|
|[1,2,4]|
|[2,4,8]|
|[3,6,12]|

<!-- csl: https://help.kusto.chinacloudapi.cn:443/Samples -->
```kusto
range x from 1 to 3 step 1
| extend y = tostring(x * 2)
| extend z = (x * 2) * 1s
| project pack_array(x,y,z)
```

|Column1|
|---|
|[1,"2","00:00:02"]|
|[2,"4","00:00:04"]|
|[3,"6","00:00:06"]|
