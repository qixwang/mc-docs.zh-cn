---
title: parse_ipv6() - Azure 数据资源管理器
description: 本文介绍 Azure 数据资源管理器中的 parse_ipv6() 函数。
services: data-explorer
author: orspod
ms.author: v-tawe
ms.reviewer: alexans
ms.service: data-explorer
ms.topic: reference
origin.date: 05/27/2020
ms.date: 08/06/2020
ms.openlocfilehash: 60a873e81ad60d6f43303dbaa1eaab69d0a5784c
ms.sourcegitcommit: 7ceeca89c0f0057610d998b64c000a2bb0a57285
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/06/2020
ms.locfileid: "87841363"
---
# <a name="parse_ipv6"></a>parse_ipv6()

将 IPv6 或 IPv4 字符串转换为规范 IPv6 字符串表示形式。

```kusto
parse_ipv6("127.0.0.1") == '0000:0000:0000:0000:0000:ffff:7f00:0001'
parse_ipv6(":fe80::85d:e82c:9446:7994") == 'fe80:0000:0000:0000:085d:e82c:9446:7994'
```

**语法**

`parse_ipv6(`*`Expr`*`)`

**参数**

* *`Expr`* ：表示将转换为规范 IPv6 表示形式的 IPv6/IPv4 网络地址的字符串表达式。 字符串可以包含使用 [IP 前缀表示法](#ip-prefix-notation)的网络掩码。

## <a name="ip-prefix-notation"></a>IP 前缀表示法

IP 地址可通过使用左斜线 (`/`) 字符的 `IP-prefix notation` 进行定义。
左斜线 (`/`) 左边的 IP 地址是基本 IP 地址。 左斜线 (`/`) 右边的数字（1 到 127）是网络掩码中连续 1 位的数目。

**返回**

如果转换成功，则结果将是表示规范 IPv6 网络地址的字符串。
如果转换未成功，结果将为 `null`。

**示例**

<!-- csl: https://help.kusto.chinacloudapi.cn/Samples -->
```kusto
datatable(ip_string:string, netmask:long)
[
 '192.168.255.255',     32,  // 32-bit netmask is used
 '192.168.255.255/24',  30,  // 24-bit netmask is used, as IPv4 address doesn't use upper 8 bits
 '255.255.255.255',     24,  // 24-bit netmask is used
]
| extend ip_long = parse_ipv4_mask(ip_string, netmask)
```

|ip_string|网络掩码|ip_long|
|---|---|---|
|192.168.255.255|32|3232301055|
|192.168.255.255/24|30|3232300800|
|255.255.255.255|24|4294967040|


