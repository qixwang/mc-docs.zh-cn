---
title: parse_command_line() - Azure 数据资源管理器
description: 本文介绍 Azure 数据资源管理器中的 parse_command_line()。
services: data-explorer
author: orspod
ms.author: v-tawe
ms.reviewer: slneimer
ms.service: data-explorer
ms.topic: reference
origin.date: 06/28/2020
ms.date: 08/06/2020
ms.openlocfilehash: 4ee69391d4fcede20644531bdad8d0b662537845
ms.sourcegitcommit: 7ceeca89c0f0057610d998b64c000a2bb0a57285
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/06/2020
ms.locfileid: "87841720"
---
# <a name="parse_command_line"></a>parse_command_line()

分析 Unicode 命令行字符串并返回命令行参数的动态数组。

**语法**

`parse_command_line(`*command_line*,*parser_type*`)`

**参数**

* command_line：要分析的命令行。
* parser_type：当前支持的唯一值为 `"Windows"`，它以与 [CommandLineToArgvW](https://docs.microsoft.com/windows/win32/api/shellapi/nf-shellapi-commandlinetoargvw) 相同的方式分析命令行。

**返回**

命令行参数的动态数组。

**示例**

<!-- csl: https://help.kusto.chinacloudapi.cn:443/Samples -->
```kusto
print parse_command_line("echo \"hello world!\"", "windows")
```

|结果|
|---|
|["echo","hello world!"]|
