---
title: include 文件
description: include 文件
services: batch
author: LauraBrenner
ms.service: batch
ms.topic: include
ms.date: 04/20/2020
ms.author: v-tawe
ms.custom: include file
ms.openlocfilehash: 46c0097062b76a67e0b32e129bf1460438cb48bb
ms.sourcegitcommit: cbaa1aef101f67bd094f6ad0b4be274bbc2d2537
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/28/2020
ms.locfileid: "84126627"
---
| **资源** | **默认限制** | **最大限制** |
| --- | --- | --- |
| 每订阅每区域的 Azure Batch 帐户数 | 1-3 |50 |
| 每个批处理帐户的专用核心数 | 90-900 | 联系支持人员 |
| 每个批处理帐户的低优先级核心数 | 10-100 | 联系支持人员 |
| 每个批处理帐户的[活动](https://docs.microsoft.com/rest/api/batchservice/job/get#jobstate)作业和作业计划（已完成作业无限制） | 100-300 | 1,000<sup>1</sup> |
| 每个批处理帐户的池数 | 20-100 | 500<sup>1</sup> |

<sup>1</sup>若要请求超过此限制的增加，请与 Azure 支持联系。

> [!NOTE]
> 默认限制因用于创建 Batch 帐户的订阅类型而异。 显示的核心配额适用于 Batch 服务模式下的 Batch 帐户。 [查看 Batch 帐户中的配额](../articles/batch/batch-quota-limit.md#view-batch-quotas)。

> [!IMPORTANT]
> 为了帮助我们在全球疫情期间更好地管理产能，某些区域的新批处理帐户和某些订阅类型的默认核心配额值已从上述值范围减少，在某些情况下为零核心。 创建新的批处理帐户时，请[查看核心配额](../articles/batch/batch-quota-limit.md#view-batch-quotas)，并在需要时[申请增加核心配额](../articles/batch/batch-quota-limit.md#increase-a-quota)。 
