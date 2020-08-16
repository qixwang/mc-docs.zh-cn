---
title: log2() - Azure 数据资源管理器 | Microsoft Docs
description: 本文介绍 Azure 数据资源管理器中的 log2()。
services: data-explorer
author: orspod
ms.author: v-tawe
ms.reviewer: rkarlin
ms.service: data-explorer
ms.topic: reference
origin.date: 08/11/2019
ms.date: 08/06/2020
ms.openlocfilehash: c7f9b948236fc468af4372b6ba80524ba03289f8
ms.sourcegitcommit: 7ceeca89c0f0057610d998b64c000a2bb0a57285
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/06/2020
ms.locfileid: "87841308"
---
# <a name="log2"></a>log2()

`log2()` 返回以 2 为底的对数函数。  

**语法**

`log2(`*x*`)`

**参数**

* x：大于 0 的实数。

**返回**

* 对数是以 2 为底的对数：以 2 为底的指数函数 (exp) 的倒数。
* 如果参数为负或为 NULL 或不能转换为 `real` 值，则此项为 `null`。 

**另请参阅**

* 有关自然对数（以 e 为底），请参阅 [log()](log-function.md)。
* 有关常用对数（以 10 为底），请参阅 [log10()](log10-function.md)。