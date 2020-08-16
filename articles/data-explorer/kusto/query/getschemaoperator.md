---
title: getschema 运算符 - Azure 数据资源管理器
description: 本文介绍 Azure 数据资源管理器中的 getschema 运算符。
services: data-explorer
author: orspod
ms.author: v-tawe
ms.reviewer: rkarlin
ms.service: data-explorer
ms.topic: reference
origin.date: 02/13/2020
ms.date: 08/06/2020
ms.openlocfilehash: 56bd8d1491b5a757b4d648b73a59cca9f6621986
ms.sourcegitcommit: 7ceeca89c0f0057610d998b64c000a2bb0a57285
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/06/2020
ms.locfileid: "87841750"
---
# <a name="getschema-operator"></a>getschema 运算符 

生成表示输入的表格架构的表。

```kusto
T | summarize MyCount=count() by Country | getschema 
```

**语法**

T `| ` `getschema`

**示例**

<!-- csl: https://help.kusto.chinacloudapi.cn:443/Samples -->
```kusto
StormEvents
| top 10 by Timestamp
| getschema
```

|ColumnName|ColumnOrdinal|数据类型|ColumnType|
|---|---|---|---|
|Timestamp|0|System.DateTime|datetime|
|语言|1|System.String|string|
|页面|2|System.String|string|
|视图|3|System.Int64|long
|BytesDelivered|4|System.Int64|long
