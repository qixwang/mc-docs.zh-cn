---
title: getmonth() - Azure 数据资源管理器
description: 本文介绍 Azure 数据资源管理器中的 getmonth()。
services: data-explorer
author: orspod
ms.author: v-tawe
ms.reviewer: rkarlin
ms.service: data-explorer
ms.topic: reference
origin.date: 02/22/2020
ms.date: 08/06/2020
ms.openlocfilehash: 2b7900a4cab6a642930f13e91efb325f20164a2c
ms.sourcegitcommit: 7ceeca89c0f0057610d998b64c000a2bb0a57285
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/06/2020
ms.locfileid: "87841751"
---
# <a name="getmonth"></a>getmonth()

从日期时间获取月份号 (1-12)。

另一个别名：monthoyear()

**示例**

<!-- csl: https://help.kusto.chinacloudapi.cn/Samples -->
```kusto
print month = getmonth(datetime(2015-10-12))
```
