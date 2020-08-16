---
title: repeat() - Azure 数据资源管理器 | Microsoft Docs
description: 本文介绍 Azure 数据资源管理器中的 repeat()。
services: data-explorer
author: orspod
ms.author: v-tawe
ms.reviewer: rkarlin
ms.service: data-explorer
ms.topic: reference
origin.date: 02/13/2020
ms.date: 08/06/2020
ms.openlocfilehash: 8ea999b5888ef5981a1718e37efa9d16ab8bd11e
ms.sourcegitcommit: 7ceeca89c0f0057610d998b64c000a2bb0a57285
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/06/2020
ms.locfileid: "87841501"
---
# <a name="repeat"></a>repeat()

生成包含一系列相等值的动态数组。

**语法**

`repeat(`*value*`,` *count*`)` 

**参数**

* *value*：生成数组中元素的值。 值的类型可以是 boolean、integer、long、real、datetime 或 timespan。   
* count：生成数组中的元素计数。 count 必须是整数。
如果 count 等于零，则返回空数组。
如果 count 小于零，则返回 null 值。 

**示例**

以下示例返回 `[1, 1, 1]`：

```kusto
T | extend r = repeat(1, 3)
```