---
title: log() - Azure 数据资源管理器 | Microsoft Docs
description: 本文介绍 Azure 数据资源管理器中的 log()。
services: data-explorer
author: orspod
ms.author: v-tawe
ms.reviewer: rkarlin
ms.service: data-explorer
ms.topic: reference
origin.date: 08/11/2019
ms.date: 08/06/2020
ms.openlocfilehash: 9d6dd470d96e994ddc12457623433855ac8fbccd
ms.sourcegitcommit: 7ceeca89c0f0057610d998b64c000a2bb0a57285
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/06/2020
ms.locfileid: "87841309"
---
# <a name="log"></a>log()

`log()` 返回自然对数函数。  

**语法**

`log(`*x*`)`

**参数**

* x：大于 0 的实数。

**返回**

* 自然对数是以 e 为底的对数：自然指数函数 (exp) 的反函数。
* 如果参数为负或为 NULL 或不能转换为 `real` 值，则此项为 `null`。 

**另请参阅**

* 有关常用对数（以 10 为底），请参阅 [log10()](log10-function.md)。
* 有关以 2 为底的对数，请参阅 [log2()](log2-function.md)