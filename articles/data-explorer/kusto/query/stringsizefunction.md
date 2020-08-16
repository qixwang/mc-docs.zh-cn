---
title: string_size() - Azure 数据资源管理器 | Microsoft Docs
description: 本文介绍 Azure 数据资源管理器中的 string_size()。
services: data-explorer
author: orspod
ms.author: v-tawe
ms.reviewer: rkarlin
ms.service: data-explorer
ms.topic: reference
origin.date: 02/13/2020
ms.date: 08/06/2020
ms.openlocfilehash: 9e24e2b4e986a47e393092e9d438ce5a7b4190b2
ms.sourcegitcommit: 7ceeca89c0f0057610d998b64c000a2bb0a57285
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/06/2020
ms.locfileid: "87841426"
---
# <a name="string_size"></a>string_size()

返回输入字符串的大小（以字节为单位）。

**语法**

`string_size(`*source*`)`

**参数**

* *source*：将测量字符串大小的源字符串。

**返回**

返回输入字符串的长度（以字节为单位）。

**示例**

```kusto
print size = string_size("hello")
```

|大小|
|---|
|5|

```kusto
print size = string_size("⒦⒰⒮⒯⒪")
```

|大小|
|---|
|15|