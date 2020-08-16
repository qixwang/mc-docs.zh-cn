---
title: todatetime() - Azure 数据资源管理器
description: 本文介绍 Azure 数据资源管理器中的 todatetime()。
services: data-explorer
author: orspod
ms.author: v-tawe
ms.reviewer: rkarlin
ms.service: data-explorer
ms.topic: reference
origin.date: 02/13/2020
ms.date: 08/06/2020
ms.openlocfilehash: be749406746bd102499c077295bb5a6cda5c0ab4
ms.sourcegitcommit: 7ceeca89c0f0057610d998b64c000a2bb0a57285
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/06/2020
ms.locfileid: "87841421"
---
# <a name="todatetime"></a>todatetime()

将输入转换为[日期/时间](./scalar-data-types/datetime.md)标量。

```kusto
todatetime("2015-12-24") == datetime(2015-12-24)
```

**语法**

`todatetime(`Expr`)`

**参数**

* Expr：将转换为[日期/时间](./scalar-data-types/datetime.md)的表达式。

**返回**

如果转换成功，则结果将为[日期/时间](./scalar-data-types/datetime.md)值。
否则，结果将为 null。
 
> [!NOTE]
> 尽可能首选使用 [datetime()](./scalar-data-types/datetime.md)。
