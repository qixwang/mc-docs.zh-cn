---
title: parse_csv() - Azure 数据资源管理器
description: 本文介绍 Azure 数据资源管理器中的 parse_csv()。
services: data-explorer
author: orspod
ms.author: v-tawe
ms.reviewer: rkarlin
ms.service: data-explorer
ms.topic: reference
origin.date: 02/13/2020
ms.date: 08/06/2020
ms.openlocfilehash: ead4d3ddd099c5577a3a7efb7e9812b6f1a76040
ms.sourcegitcommit: 7ceeca89c0f0057610d998b64c000a2bb0a57285
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/06/2020
ms.locfileid: "87841350"
---
# <a name="parse_csv"></a>parse_csv()

拆分表示单个记录（包含逗号分隔值）的给定字符串，并返回包含这些值的字符串数组。

```kusto
parse_csv("aaa,bbb,ccc") == ["aaa","bbb","ccc"]
```

**语法**

`parse_csv(`*source*`)`

**参数**

* *source*：表示单个记录（包含逗号分隔值）的源字符串。

**返回**

一个包含拆分值的字符串数组。

**备注**

可以使用双引号 ('"') 来转义嵌入行的源、逗号和引号。 此函数不支持每行多条记录（仅获取第一条记录）。

**示例**

<!-- csl: https://help.kusto.chinacloudapi.cn:443/Samples -->
```kusto
print result=parse_csv('aa,"b,b,b",cc,"Escaping quotes: ""Title""","line1\nline2"')
```

|result|
|---|
|[<br>  "aa",<br>  "b,b,b",<br>  "cc",<br>  "Escaping quotes:\"Title\"",<br>  "line1\nline2"<br>]|

具有多条记录的 CSV 有效负载：

<!-- csl: https://help.kusto.chinacloudapi.cn:443/Samples -->
```kusto
print result_multi_record=parse_csv('record1,a,b,c\nrecord2,x,y,z')
```

|result_multi_record|
|---|
|[<br>  "record1",<br>  "a",<br>  "b",<br>  "c"<br>]|
