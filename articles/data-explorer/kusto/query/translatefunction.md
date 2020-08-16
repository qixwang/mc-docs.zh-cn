---
title: translate() - Azure 数据资源管理器 | Microsoft Docs
description: 本文介绍 Azure 数据资源管理器中的 translate()。
services: data-explorer
author: orspod
ms.author: v-tawe
ms.reviewer: rkarlin
ms.service: data-explorer
ms.topic: reference
origin.date: 03/11/2019
ms.date: 08/06/2020
ms.openlocfilehash: f8bbf72bb1affbf7ced82d258b67bbf1a60f937b
ms.sourcegitcommit: 7ceeca89c0f0057610d998b64c000a2bb0a57285
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/06/2020
ms.locfileid: "87841483"
---
# <a name="translate"></a>translate()

将一组字符（“searchList”）替换为给定字符串中的另一组字符（“replacementList”）。
此函数在“searchList”中搜索字符，并将其替换为“replacementList”中的相应字符

**语法**

`translate(`*searchList*`,` *replacementList*`,` *text*`)`

**参数**

* searchList：应被替换的字符的列表
* replacementList：应替换“searchList”中字符的字符的列表
* *text*：要搜索的字符串

**返回**

将“replacementList”中出现的所有字符替换为“searchList”中的相应字符后得到的 text

**示例**

|输入                                 |输出   |
|--------------------------------------|---------|
|`translate("abc", "x", "abc")`        |`"xxx"`  |
|`translate("abc", "", "ab")`          |`""`     |
|`translate("krasp", "otsku", "spark")`|`"kusto"`|