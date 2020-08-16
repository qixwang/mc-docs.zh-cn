---
title: isnotnull() - Azure 数据资源管理器 | Microsoft Docs
description: 本文介绍 Azure 数据资源管理器中的 isnotnull()。
services: data-explorer
author: orspod
ms.author: v-tawe
ms.reviewer: rkarlin
ms.service: data-explorer
ms.topic: reference
origin.date: 02/13/2020
ms.date: 08/06/2020
ms.openlocfilehash: 8a6246d4c04b524cf6ca8a32fbe188db7e33865a
ms.sourcegitcommit: 7ceeca89c0f0057610d998b64c000a2bb0a57285
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/06/2020
ms.locfileid: "87841438"
---
# <a name="isnotnull"></a>isnotnull()

如果参数不为 null，则返回 `true`。

**语法**

`isnotnull(`[*value*]`)`

`notnull(`[*value*]`)` - `isnotnull` 的别名

**示例**

```kusto
T | where isnotnull(PossiblyNull) | count
```

请注意，还可通过其他方法实现这种效果：

```kusto
T | summarize count(PossiblyNull)
```