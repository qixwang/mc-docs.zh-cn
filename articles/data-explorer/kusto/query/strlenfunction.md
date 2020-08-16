---
title: strlen() - Azure 数据资源管理器 | Microsoft Docs
description: 本文介绍 Azure 数据资源管理器中的 strlen()。
services: data-explorer
author: orspod
ms.author: v-tawe
ms.reviewer: rkarlin
ms.service: data-explorer
ms.topic: reference
origin.date: 02/13/2020
ms.date: 08/06/2020
ms.openlocfilehash: fb3a31ec6c4095dc966cdb25c4a914785f2f8111
ms.sourcegitcommit: 7ceeca89c0f0057610d998b64c000a2bb0a57285
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/06/2020
ms.locfileid: "87841551"
---
# <a name="strlen"></a>strlen()

返回输入字符串的长度（以字符为单位）。

**语法**

`strlen(`*source*`)`

**参数**

* *source*：将测量字符串长度的源字符串。

**返回**

返回输入字符串的长度（以字符为单位）。

**备注**

字符串中的每个 Unicode 字符都等于 `1`，包括代理项。
（例如，尽管在 UTF-8 编码中需要多个值，但中文字符将被计数一次）。


**示例**

```kusto
print length = strlen("hello")
```

|length|
|---|
|5|

```kusto
print length = strlen("⒦⒰⒮⒯⒪")
```

|length|
|---|
|5|