---
title: hash_many() - Azure 数据资源管理器
description: 本文介绍 Azure 数据资源管理器中的 hash_many()。
services: data-explorer
author: orspod
ms.author: v-tawe
ms.reviewer: rkarlin
ms.service: data-explorer
ms.topic: reference
origin.date: 03/06/2020
ms.date: 08/06/2020
ms.openlocfilehash: 138027f81dd271d153e531d120566045198cba34
ms.sourcegitcommit: 7ceeca89c0f0057610d998b64c000a2bb0a57285
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/06/2020
ms.locfileid: "87841445"
---
# <a name="hash_many"></a>hash_many()

返回多个值的组合哈希值。

**语法**

`hash_many(`*s1* `,` *s2* [`,` *s3* ...]`)`

**参数**

* s1、s2、...、sN：将一起进行哈希处理的输入值。

**返回**

给定标量的组合哈希值。

**示例**

<!-- csl: https://help.kusto.chinacloudapi.cn/Samples -->
```kusto
print value1 = "Hello", value2 = "World"
| extend combined = hash_many(value1, value2)
```

|value1|value2|已合并|
|---|---|---|
|你好|World|-1440138333540407281|
