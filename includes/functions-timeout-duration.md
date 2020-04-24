---
title: include 文件
description: include 文件
services: functions
author: nzthiago
ms.service: azure-functions
ms.topic: include
ms.date: 02/17/2020
ms.author: v-junlch
ms.custom: include file
ms.openlocfilehash: 7e730d914e89a83f418a2943ccab02f00434617f
ms.sourcegitcommit: c1ba5a62f30ac0a3acb337fb77431de6493e6096
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2020
ms.locfileid: "77494187"
---
## <a name="function-app-timeout-duration"></a><a name="timeout"></a>函数应用超时持续时间 

函数应用的超时持续时间通过 `functionTimeout`host.json[ 项目文件中的 ](../articles/azure-functions/functions-host-json.md#functiontimeout) 属性进行定义。 下表显示了两种计划和不同运行时版本的默认值和最大值（以分钟为单位）：

| 计划 | 运行时版本 | 默认 | 最大值 |
|------|---------|---------|---------|
| 消耗 | 1.x | 5 | 10 |
| 消耗 | 2.x | 5 | 10 |
| 消耗 | 3.x | 5 | 10 |
| Premium | 1.x | 30 | 无限制 |
| Premium | 2.x | 30 | 无限制 |
| Premium | 3.x | 30 | 无限制 |
| 应用服务 | 1.x | 无限制 | 无限制 |
| 应用服务 | 2.x | 30 | 无限制 |
| 应用服务 | 3.x | 30 | 无限制 |

> [!NOTE] 
> 不管函数应用超时设置如何，230 秒是 HTTP 触发的函数在响应请求时需要的最长时间。 这起因于 [Azure 负载均衡器的默认空闲超时](../articles/app-service/faq-availability-performance-application-issues.md#why-does-my-request-time-out-after-230-seconds)。 对于处理时间较长的情况，考虑使用 [Durable Functions 异步模式](../articles/azure-functions/durable/durable-functions-overview.md#async-http)或[延迟实际工作并返回即时响应](../articles/azure-functions/functions-best-practices.md#avoid-long-running-functions)。

