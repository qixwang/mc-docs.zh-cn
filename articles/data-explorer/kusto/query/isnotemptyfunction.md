---
title: isnotempty() - Azure 数据资源管理器
description: 本文介绍 Azure 数据资源管理器中的 isnotempty()。
services: data-explorer
author: orspod
ms.author: v-tawe
ms.reviewer: rkarlin
ms.service: data-explorer
ms.topic: reference
origin.date: 02/13/2020
ms.date: 08/06/2020
ms.openlocfilehash: d9477161f074fc948aa20fdb3c29b48cb8f65238
ms.sourcegitcommit: 7ceeca89c0f0057610d998b64c000a2bb0a57285
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/06/2020
ms.locfileid: "87841629"
---
# <a name="isnotempty"></a>isnotempty()

如果参数不为空字符串且不为 null，则返回 `true`。

```kusto
isnotempty("") == false
```

**语法**

`isnotempty(`[*value*]`)`

`notempty(`[*value*]`)` - `isnotempty` 的别名
