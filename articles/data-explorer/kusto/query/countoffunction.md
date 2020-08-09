---
title: countof() - Azure 数据资源管理器 | Microsoft Docs
description: 本文介绍 Azure 数据资源管理器中的 countof()。
services: data-explorer
author: orspod
ms.author: v-tawe
ms.reviewer: rkarlin
ms.service: data-explorer
ms.topic: reference
origin.date: 02/13/2020
ms.date: 07/31/2020
ms.openlocfilehash: ae57f958ae10eea395724e9e65fd6b11bdc9d744
ms.sourcegitcommit: 4e1bc2e9b2a12dbcc05c52db5dbd1ae290aeb18d
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/01/2020
ms.locfileid: "87509145"
---
# <a name="countof"></a>countof()

计算字符串中子字符串的出现次数。 纯字符串匹配项可能重叠；正则表达式匹配项不会重叠。

```kusto
countof("The cat sat on the mat", "at") == 3
countof("The cat sat on the mat", @"\b.at\b", "regex") == 3
```

## <a name="syntax"></a>语法

`countof(`*text*`,` *search* [`,` *kind*]`)`

## <a name="arguments"></a>参数

* *text*：一个字符串。
* *search*：用于在 text 内部匹配的纯字符串或[正则表达式](./re2.md)。
* *kind*：`"normal"|"regex"` 默认值为 `normal`。 

## <a name="returns"></a>返回

搜索字符串可在容器中匹配的次数。 纯字符串匹配项可能重叠；正则表达式匹配项不会重叠。

## <a name="examples"></a>示例

|||
|---|---
|`countof("aaa", "a")`| 3 
|`countof("aaaa", "aa")`| 3 (不是 2!)
|`countof("ababa", "ab", "normal")`| 2
|`countof("ababa", "aba")`| 2
|`countof("ababa", "aba", "regex")`| 1
|`countof("abcabc", "a.c", "regex")`| 2
    