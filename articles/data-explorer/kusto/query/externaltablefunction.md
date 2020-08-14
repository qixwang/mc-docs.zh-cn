---
title: External_table() - Azure 数据资源管理器 | Microsoft Docs
description: 本文介绍 Azure 数据资源管理器中的 external_table()。
services: data-explorer
author: orspod
ms.author: v-tawe
ms.reviewer: rkarlin
ms.service: data-explorer
ms.topic: reference
origin.date: 08/21/2019
ms.date: 08/06/2020
ms.openlocfilehash: 9092eee3e37bfb3ae4292ca693d8ee9b46355cac
ms.sourcegitcommit: 7ceeca89c0f0057610d998b64c000a2bb0a57285
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/06/2020
ms.locfileid: "87841408"
---
# <a name="external_table"></a>external_table()

按名称引用外部表。

```kusto
external_table('StormEvent')
```

**语法**

`external_table` `(` *TableName* [`,` *MappingName* ] `)`

**参数**

* TableName：正在查询的外部表的名称。
  必须是引用 `blob` 或 `adl` 类型的外部表的字符串文本。 <!-- TODO: Document data formats supported -->

* *MappingName*：映射对象的可选名称，该对象将实际（外部）数据分片中的字段映射到此函数输出的列。

**备注**

有关外部表的详细信息，请参阅[外部表](schema-entities/externaltables.md)。

另请参阅[用于管理外部表的命令](../management/externaltables.md)。

`external_table` 函数与 [table](tablefunction.md) 函数具有类似的限制。