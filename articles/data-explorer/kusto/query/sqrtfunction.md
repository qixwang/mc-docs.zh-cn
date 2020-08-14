---
title: sqrt() - Azure 数据资源管理器 | Microsoft Docs
description: 本文介绍 Azure 数据资源管理器中的 sqrt()。
services: data-explorer
author: orspod
ms.author: v-tawe
ms.reviewer: rkarlin
ms.service: data-explorer
ms.topic: reference
origin.date: 10/23/2018
ms.date: 08/06/2020
ms.openlocfilehash: 7ca4772a3b7d452d94e44d82e42c7b2091925160
ms.sourcegitcommit: 7ceeca89c0f0057610d998b64c000a2bb0a57285
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/06/2020
ms.locfileid: "87841562"
---
# <a name="sqrt"></a>sqrt()

返回平方根函数。  

**语法**

`sqrt(`*x*`)`

**参数**

* x：大于或等于 0 的实数。

**返回**

* 正数值，例如 `sqrt(x) * sqrt(x) == x`
* 如果参数为负或不能转换为 `real` 值，则为 `null`。 