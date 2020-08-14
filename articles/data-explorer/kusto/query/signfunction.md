---
title: sign() - Azure 数据资源管理器 | Microsoft Docs
description: 本文介绍 Azure 数据资源管理器中的 sign()。
services: data-explorer
author: orspod
ms.author: v-tawe
ms.reviewer: rkarlin
ms.service: data-explorer
ms.topic: reference
origin.date: 02/13/2020
ms.date: 08/06/2020
ms.openlocfilehash: 70835d65a3ac29fbd2dd93aa8f37467ec661d154
ms.sourcegitcommit: 7ceeca89c0f0057610d998b64c000a2bb0a57285
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/06/2020
ms.locfileid: "87841490"
---
# <a name="sign"></a>sign()

数值表达式的符号

**语法**

`sign(`*x*`)`

**参数**

* x：一个实数。

**返回**

* 指定表达式的正号 (+1)、零 (0) 或负号 (-1)。 

**示例**

```kusto
print s1 = sign(-42), s2 = sign(0), s3 = sign(11.2)

```

|s1|s2|s3|
|---|---|---|
|-1|0|1|