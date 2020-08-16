---
title: parse_ipv4() - Azure 数据资源管理器
description: 本文介绍 Azure 数据资源管理器中的 parse_ipv4()。
services: data-explorer
author: orspod
ms.author: v-tawe
ms.reviewer: rkarlin
ms.service: data-explorer
ms.topic: reference
origin.date: 02/24/2020
ms.date: 08/06/2020
ms.openlocfilehash: 5c7f0ce81b440ac5bef22da684e75ec47039d7ef
ms.sourcegitcommit: 7ceeca89c0f0057610d998b64c000a2bb0a57285
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/06/2020
ms.locfileid: "87841366"
---
# <a name="parse_ipv4"></a>parse_ipv4()

将 IPv4 字符串转换为 long（带符号的 64 位）数字表示形式。

```kusto
parse_ipv4("127.0.0.1") == 2130706433
parse_ipv4('192.1.168.1') < parse_ipv4('192.1.168.2') == true
```

**语法**

`parse_ipv4(`*`Expr`*`)`

**参数**

* *`Expr`* ：字符串表达式，表示将转换为 long 的 IPv4。 字符串可以包含使用 [IP 前缀表示法](#ip-prefix-notation)的网络掩码。

## <a name="ip-prefix-notation"></a>IP 前缀表示法

IP 地址可通过使用斜杠 (`/`) 字符的 `IP-prefix notation` 进行定义。
斜杠 (`/`) 左边的 IP 地址是基本 IP 地址。 斜杠 (/) 右边的数字（1 到 32）是网络掩码中连续位的数目。

**示例**

192.168.2.0/24 将具有关联的网络/子网掩码，其中包含 24 个连续位或点分十进制格式的 255.255.255.0。

**返回**

如果转换成功，则结果将是一个 long 类型的数字。
如果转换未成功，结果将为 `null`。
 
**示例**

<!-- csl: https://help.kusto.chinacloudapi.cn/Samples -->
```kusto
datatable(ip_string:string)
[
 '192.168.1.1',
 '192.168.1.1/24',
 '255.255.255.255/31'
]
| extend ip_long = parse_ipv4(ip_string)
```

|ip_string|ip_long|
|---|---|
|192.168.1.1|3232235777|
|192.168.1.1/24|3232235776|
|255.255.255.255/31|4294967294|
