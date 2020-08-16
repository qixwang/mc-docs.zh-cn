---
title: ipv6_compare() - Azure 数据资源管理器
description: 本文介绍 Azure 数据资源管理器中的 ipv6_compare() 函数。
services: data-explorer
author: orspod
ms.author: v-tawe
ms.reviewer: alexans
ms.service: data-explorer
ms.topic: reference
origin.date: 05/27/2020
ms.date: 08/06/2020
ms.openlocfilehash: 4ee6de7597b91198c0e5f825205db4411da5d307
ms.sourcegitcommit: 7ceeca89c0f0057610d998b64c000a2bb0a57285
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/06/2020
ms.locfileid: "87841679"
---
# <a name="ipv6_compare"></a>ipv6_compare()

比较两个 IPv6 或 IPv4 网络地址字符串。 分析并比较两个 IPv6 字符串，同时考虑根据参数前缀和可选的 `PrefixMask` 参数计算出的组合 IP 前缀掩码。

```kusto
ipv6_compare('::ffff:7f00:1', '127.0.0.1') == 0
ipv6_compare('fe80::85d:e82c:9446:7994', 'fe80::85d:e82c:9446:7995')  < 0
ipv6_compare('192.168.1.1/24', '192.168.1.255/24') == 0
ipv6_compare('fe80::85d:e82c:9446:7994/127', 'fe80::85d:e82c:9446:7995/127') == 0
ipv6_compare('fe80::85d:e82c:9446:7994', 'fe80::85d:e82c:9446:7995', 127) == 0
```

**语法**

`ipv6_compare(`*Expr1*`, `*Expr2*`[ ,`*PrefixMask*`])`

**参数**

* Expr1、Expr2：表示 IPv6 或 IPv4 地址的字符串表达式。 可以使用 IP 前缀表示法（请参见注释）对 IPv6 和 IPv4 字符串进行掩码操作。
* PrefixMask：从 0 到 128 的整数，表示所考虑的最有效位的数目。

> [!Note] 
>**IP 前缀表示法**
> 
>常见的做法是通过斜杠 (`/`) 字符使用 `IP-prefix notation` 定义 IP 地址。
>斜杠 (`/`) 左侧的 IP 地址是基本 IP 地址，斜杠 (`/`) 右侧的数字（1 到 127）是网络掩码中连续位的数目。 
>
> 示例：fe80::85d:e82c:9446:7994/120 将有一个包含 120 个连续位的关联网络/子网掩码。

**返回**

* `0`：如果第一个 IPv6 字符串参数的长表示形式等于第二个 IPv6 字符串参数。
* `1`：如果第一个 IPv6 字符串参数的长表示形式大于第二个 IPv6 字符串参数。
* `-1`：如果第一个 IPv6 字符串参数的长表示形式小于第二个 IPv6 字符串参数。
* `null`：如果两个 IPv6 字符串之一转换不成功。

> [!Note]
> 此函数可以接受并比较表示 IPv6 和 IPv4 网络地址的参数。 但是，如果调用方知道参数采用 IPv4 格式，请使用 [ipv4_is_compare()](./ipv4-comparefunction.md) 函数。 此函数将产生更好的运行时性能。

## <a name="examples-ipv6ipv4-comparison-equality-cases"></a>示例:IPv6/IPv4 比较相等的情况

### <a name="compare-ips-using-the-ip-prefix-notation-specified-inside-the-ipv6ipv4-strings"></a>使用 IPv6/IPv4 字符串中指定的 IP 前缀表示法比较 IP

<!-- csl: https://help.kusto.chinacloudapi.cn/Samples -->
```kusto
datatable(ip1_string:string, ip2_string:string)
[
 // IPv4 are compared as IPv6 addresses
 '192.168.1.1',    '192.168.1.1',       // Equal IPs
 '192.168.1.1/24', '192.168.1.255',     // 24 bit IP4-prefix is used for comparison
 '192.168.1.1',    '192.168.1.255/24',  // 24 bit IP4-prefix is used for comparison
 '192.168.1.1/30', '192.168.1.255/24',  // 24 bit IP4-prefix is used for comparison
  // IPv6 cases
 'fe80::85d:e82c:9446:7994', 'fe80::85d:e82c:9446:7994',         // Equal IPs
 'fe80::85d:e82c:9446:7994/120', 'fe80::85d:e82c:9446:7998',     // 120 bit IP6-prefix is used for comparison
 'fe80::85d:e82c:9446:7994', 'fe80::85d:e82c:9446:7998/120',     // 120 bit IP6-prefix is used for comparison
 'fe80::85d:e82c:9446:7994/120', 'fe80::85d:e82c:9446:7998/120', // 120 bit IP6-prefix is used for comparison
 // Mixed case of IPv4 and IPv6
 '192.168.1.1',      '::ffff:c0a8:0101', // Equal IPs
 '192.168.1.1/24',   '::ffff:c0a8:01ff', // 24 bit IP-prefix is used for comparison
 '::ffff:c0a8:0101', '192.168.1.255/24', // 24 bit IP-prefix is used for comparison
 '::192.168.1.1/30', '192.168.1.255/24', // 24 bit IP-prefix is used for comparison
]
| extend result = ipv6_compare(ip1_string, ip2_string)
```

|ip1_string|ip2_string|result|
|---|---|---|
|192.168.1.1|192.168.1.1|0|
|192.168.1.1/24|192.168.1.255|0|
|192.168.1.1|192.168.1.255/24|0|
|192.168.1.1/30|192.168.1.255/24|0|
|fe80::85d:e82c:9446:7994|fe80::85d:e82c:9446:7994|0|
|fe80::85d:e82c:9446:7994/120|fe80::85d:e82c:9446:7998|0|
|fe80::85d:e82c:9446:7994|fe80::85d:e82c:9446:7998/120|0|
|fe80::85d:e82c:9446:7994/120|fe80::85d:e82c:9446:7998/120|0|
|192.168.1.1|::ffff:c0a8:0101|0|
|192.168.1.1/24|::ffff:c0a8:01ff|0|
|::ffff:c0a8:0101|192.168.1.255/24|0|
|::192.168.1.1/30|192.168.1.255/24|0|

### <a name="compare-ips-using-ip-prefix-notation-specified-inside-the-ipv6ipv4-strings-and-as-additional-argument-of-the-ipv6_compare-function"></a>使用 IPv6/IPv4 字符串中指定的 IP 前缀表示法作为 `ipv6_compare()` 函数的附加参数比较 IP

<!-- csl: https://help.kusto.chinacloudapi.cn/Samples -->
```kusto
datatable(ip1_string:string, ip2_string:string, prefix:long)
[
 // IPv4 are compared as IPv6 addresses 
 '192.168.1.1',    '192.168.1.0',   31, // 31 bit IP4-prefix is used for comparison
 '192.168.1.1/24', '192.168.1.255', 31, // 24 bit IP4-prefix is used for comparison
 '192.168.1.1',    '192.168.1.255', 24, // 24 bit IP4-prefix is used for comparison
   // IPv6 cases
 'fe80::85d:e82c:9446:7994', 'fe80::85d:e82c:9446:7995',     127, // 127 bit IP6-prefix is used for comparison
 'fe80::85d:e82c:9446:7994/127', 'fe80::85d:e82c:9446:7998', 120, // 120 bit IP6-prefix is used for comparison
 'fe80::85d:e82c:9446:7994/120', 'fe80::85d:e82c:9446:7998', 127, // 120 bit IP6-prefix is used for comparison
 // Mixed case of IPv4 and IPv6
 '192.168.1.1/24',   '::ffff:c0a8:01ff', 127, // 127 bit IP6-prefix is used for comparison
 '::ffff:c0a8:0101', '192.168.1.255',    120, // 120 bit IP6-prefix is used for comparison
 '::192.168.1.1/30', '192.168.1.255/24', 127, // 120 bit IP6-prefix is used for comparison
]
| extend result = ipv6_compare(ip1_string, ip2_string, prefix)
```

|ip1_string|ip2_string|前缀|result|
|---|---|---|---|
|192.168.1.1|192.168.1.0|31|0|
|192.168.1.1/24|192.168.1.255|31|0|
|192.168.1.1|192.168.1.255|24|0|
|fe80::85d:e82c:9446:7994|fe80::85d:e82c:9446:7995|127|0|
|fe80::85d:e82c:9446:7994/127|fe80::85d:e82c:9446:7998|120|0|
|fe80::85d:e82c:9446:7994/120|fe80::85d:e82c:9446:7998|127|0|
|192.168.1.1/24|::ffff:c0a8:01ff|127|0|
|::ffff:c0a8:0101|192.168.1.255|120|0|
|::192.168.1.1/30|192.168.1.255/24|127|0|

