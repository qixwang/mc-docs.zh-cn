---
title: hash_md5() - Azure 数据资源管理器
description: 本文介绍 Azure 数据资源管理器中的 hash_md5()。
services: data-explorer
author: orspod
ms.author: v-tawe
ms.reviewer: alexans
ms.service: data-explorer
ms.topic: reference
origin.date: 06/29/2020
ms.date: 08/06/2020
ms.openlocfilehash: e973dd4155734581e6648fe68828d67eff9a3cb9
ms.sourcegitcommit: 7ceeca89c0f0057610d998b64c000a2bb0a57285
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/06/2020
ms.locfileid: "87841646"
---
# <a name="hash_md5"></a>hash_md5()

返回输入值的 MD5 哈希值。

**语法**

`hash_md5(`*source*`)`

**参数**

* *source*：要进行哈希处理的值。

**返回**

给定标量的 MD5 哈希值编码为十六进制字符串（其中的每两个字符都表示 0 到 255 之间的一个十六进制数字）。

> [!WARNING]
> 此函数 (MD5) 所用的算法可确保不会在以后修改，但计算起来非常复杂。 建议需要在单个查询期间使用“轻量级”哈希函数的用户改为使用函数 [hash()](./hashfunction.md)。

**示例**

<!-- csl: https://help.kusto.chinacloudapi.cn/Samples -->
```kusto
print 
h1=hash_md5("World"),
h2=hash_md5(datetime(2020-01-01))
```

|h1|h2|
|---|---|
|f5a7924e621e84c9280a9a27e1bcb7f6|786c530672d1f8db31fee25ea8a9390b|


以下示例使用 `hash_md5()` 函数根据 State 的 MD5 哈希值聚合 StormEvents。 

<!-- csl: https://help.kusto.chinacloudapi.cn/Samples -->
```kusto
StormEvents
| summarize StormCount = count() by State, StateHash=hash_md5(State)
| top 5 by StormCount
```

|状态|StateHash|StormCount|
|---|---|---|
|德克萨斯|3b00dbe6e07e7485a1c12d36c8e9910a|4701|
|KANSAS|e1338d0ac8be43846cf9ae967bd02e7f|3166|
|衣阿华州|6d4a7c02942f093576149db764d4e2d2|2337|
|ILLINOIS|8c00d9e0b3fcd55aed5657e42cc40cf1|2022|
|MISSOURI|2d82f0c963c0763012b2539d469e5008|2016|
