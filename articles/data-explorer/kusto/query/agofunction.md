---
title: ago() - Azure 数据资源管理器 | Microsoft Docs
description: 本文介绍 Azure 数据资源管理器中的 ago()。
services: data-explorer
author: orspod
ms.author: v-tawe
ms.reviewer: rkarlin
ms.service: data-explorer
ms.topic: reference
origin.date: 02/13/2020
ms.date: 07/31/2020
ms.openlocfilehash: 86cb38bd91d229a0dea8b96be23789cac96fffd6
ms.sourcegitcommit: 4e1bc2e9b2a12dbcc05c52db5dbd1ae290aeb18d
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/01/2020
ms.locfileid: "87509555"
---
# <a name="ago"></a>ago()

从当前 UTC 时钟时间减去给定时间跨度。

```kusto
ago(1h)
ago(1d)
```

与 `now()` 类似，此函数可在语句中多次使用，并且所有实例化引用的 UTC 时钟时间均相同。

## <a name="syntax"></a>语法

`ago(`a_timespan`)`

## <a name="arguments"></a>参数

* a_timespan：要从当前 UTC 时钟时间 (`now()`) 减去的间隔。

## <a name="returns"></a>返回

`now() - a_timespan`

## <a name="example"></a>示例

过去一小时内具有时间戳的所有行：

```kusto
T | where Timestamp > ago(1h)
```