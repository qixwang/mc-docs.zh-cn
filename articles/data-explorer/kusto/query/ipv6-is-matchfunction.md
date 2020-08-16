---
title: ipv6_is_match() - Azure 数据资源管理器
description: 本文介绍 Azure 数据资源管理器中的 ipv6_is_match() 函数。
services: data-explorer
author: orspod
ms.author: v-tawe
ms.reviewer: alexans
ms.service: data-explorer
ms.topic: reference
origin.date: 05/27/2020
ms.date: 08/06/2020
ms.openlocfilehash: 543d0ab0d1ef8e6b9c040dab2f0a75de58bf55cf
ms.sourcegitcommit: 7ceeca89c0f0057610d998b64c000a2bb0a57285
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/06/2020
ms.locfileid: "87841632"
---
# <a name="ipv6_is_match"></a>ipv6_is_match()

匹配两个 IPv6 或 IPv4 网络地址字符串。 分析并比较两个 IPv6/IPv4 字符串，同时考虑根据参数前缀和可选的 `PrefixMask` 参数计算出的组合 IP 前缀掩码。

```kusto
ipv6_is_match('::ffff:7f00:1', '127.0.0.1') == true
ipv6_is_match('fe80::85d:e82c:9446:7994', 'fe80::85d:e82c:9446:7995') == false
ipv6_is_match('192.168.1.1/24', '192.168.1.255/24') == true
ipv6_is_match('fe80::85d:e82c:9446:7994/127', 'fe80::85d:e82c:9446:7995/127') == true
ipv6_is_match('fe80::85d:e82c:9446:7994', 'fe80::85d:e82c:9446:7995', 127) == true
```

**语法**

`ipv6_is_match(`*Expr1*`, `*Expr2*`[ ,`*PrefixMask*`])`

**参数**

* Expr1、Expr2：表示 IPv6 或 IPv4 地址的字符串表达式。 可以使用 [IP 前缀表示法](#ip-prefix-notation)对 IPv6 和 IPv4 字符串进行掩码操作。
* PrefixMask：从 0 到 128 的整数，表示所考虑的最有效位的数目。

## <a name="ip-prefix-notation"></a>IP 前缀表示法
 
IP 地址可以通过斜杠 (`/`) 字符使用 `IP-prefix notation` 进行定义。
斜杠 (`/`) 左边的 IP 地址是基本 IP 地址。 斜杠 (`/`) 右边的数字（1 到 127）是网络掩码中连续位的数目。 

示例：fe80::85d:e82c:9446:7994/120 将有一个包含 120 个连续位的关联网络/子网掩码。

**返回**

* `true`：如果第一个 IPv6/IPv4 字符串参数的长表示形式等于第二个 IPv6/IPv4 字符串参数。
* `false`：其他情况。
* `null`：如果两个 IPv6/IPv4 字符串之一转换不成功。

> [!Note]
> 此函数可以接受并比较表示 IPv6 和 IPv4 网络地址的参数。 如果调用方知道参数采用 IPv4 格式，请使用 [ipv4_is_match()](./ipv4-is-matchfunction.md) 函数。 此函数将产生更好的运行时性能。

## <a name="examples"></a>示例

### <a name="ipv6ipv4-comparison-equality-case---ip-prefix-notation-specified-inside-the-ipv6ipv4-strings"></a>IPv6/IPv4 比较相等的情况 - IPv6/IPv4 字符串中指定的 IP 前缀表示法

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
| extend result = ipv6_is_match(ip1_string, ip2_string)
```

|ip1_string|ip2_string|result|
|---|---|---|
|192.168.1.1|192.168.1.1|1|
|192.168.1.1/24|192.168.1.255|1|
|192.168.1.1|192.168.1.255/24|1|
|192.168.1.1/30|192.168.1.255/24|1|
|fe80::85d:e82c:9446:7994|fe80::85d:e82c:9446:7994|1|
|fe80::85d:e82c:9446:7994/120|fe80::85d:e82c:9446:7998|1|
|fe80::85d:e82c:9446:7994|fe80::85d:e82c:9446:7998/120|1|
|fe80::85d:e82c:9446:7994/120|fe80::85d:e82c:9446:7998/120|1|
|192.168.1.1|::ffff:c0a8:0101|1|
|192.168.1.1/24|::ffff:c0a8:01ff|1|
|::ffff:c0a8:0101|192.168.1.255/24|1|
|::192.168.1.1/30|192.168.1.255/24|1|


### <a name="ipv6ipv4-comparison-equality-case--ip-prefix-notation-specified-inside-the-ipv6ipv4-strings-and-as-additional-argument-of-the-ipv6_is_match-function"></a>IPv6/IPv4 比较相等的情况 - IP 前缀表示法在 IPv6/IPv4 字符串中指定，作为 `ipv6_is_match()` 函数的附加参数

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
| extend result = ipv6_is_match(ip1_string, ip2_string, prefix)
```

|ip1_string|ip2_string|前缀|result|
|---|---|---|---|
|192.168.1.1|192.168.1.0|31|1|
|192.168.1.1/24|192.168.1.255|31|1|
|192.168.1.1|192.168.1.255|24|1|
|fe80::85d:e82c:9446:7994|fe80::85d:e82c:9446:7995|127|1|
|fe80::85d:e82c:9446:7994/127|fe80::85d:e82c:9446:7998|120|1|
|fe80::85d:e82c:9446:7994/120|fe80::85d:e82c:9446:7998|127|1|
|192.168.1.1/24|::ffff:c0a8:01ff|127|1|
|::ffff:c0a8:0101|192.168.1.255|120|1|
|::192.168.1.1/30|192.168.1.255/24|127|1|
