---
title: include 文件
description: include 文件
services: batch
author: laurenhughes
ms.service: batch
ms.topic: include
ms.date: 04/20/2020
ms.author: v-tawe
ms.custom: include file
ms.openlocfilehash: 7afc41fb84d7f9e6f59134929c33684092805b2e
ms.sourcegitcommit: d6db729fea7d491d876d491f19ff89ef52384329
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/29/2020
ms.locfileid: "82516011"
---
| **资源** | **默认限制** | **最大限制** |
| --- | --- | --- |
| 每订阅每区域的 Azure Batch 帐户数 | 1-3 |50 |
| 每个批处理帐户的专用核心数 | 90-900 | 联系支持人员 |
| 每个批处理帐户的低优先级核心数 | 10-100 | 联系支持人员 |
| 每个批处理帐户的[活动](https://docs.microsoft.com/rest/api/batchservice/job/get#jobstate)  作业和作业计划（已完成  作业无限制） | 100-300 | 1,000<sup>1</sup> |
| 每个批处理帐户的池数 | 20-100 | 500<sup>1</sup> |

> [!NOTE]
> 默认限制因用于创建 Batch 帐户的订阅类型而异。 显示的核心配额适用于 Batch 服务模式下的 Batch 帐户。 [查看 Batch 帐户中的配额](../articles/batch/batch-quota-limit.md#view-batch-quotas)。

<sup>1</sup>若要请求超过此限制的增加，请与 Azure 支持联系。
