---
title: 出错后重试任务
description: 检查是否存在错误，然后重试。
services: batch
documentationcenter: .net
author: LauraBrenner
manager: evansma
editor: ''
tags: azure-batch
ms.assetid: 16279b23-60ff-4b16-b308-5de000e4c028
ms.service: batch
ms.topic: article
ms.tgt_pltfrm: ''
ms.workload: big-compute
ms.date: 04/29/2020
ms.author: v-tawe
ms.custom: seodec18
origin.date: 02/15/2020
ms.openlocfilehash: fccab332735126c28e0f4d645c70eb4b560e2e2e
ms.sourcegitcommit: 1fbdefdace8a1d3412900c6c3f89678d8a9b29bc
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/07/2020
ms.locfileid: "82886982"
---
# <a name="detecting-and-handling-batch-service-errors"></a>检测和处理 Batch 服务错误

使用 REST 服务 API 时，务必检查是否有错误。 运行批处理作业时发生错误的情况并不少见。

## <a name="common-errors"></a>常见错误 

- 网络故障 - 这些请求从未到达 Batch，或者 Batch 响应未及时到达客户端。
- 内部服务器错误 - 这些是标准 5xx 状态代码 HTTP 响应。
- 限制可能会导致错误，例如带有 Retry-after 标头的 429 或 503 状态代码 HTTP 响应。
- 4xx 错误，包括 AlreadyExists 和 InvalidOperation 等错误。 这意味着资源未处于状态转换的正确状态。

若要详细了解各种类型的错误代码和特定的错误代码，请参阅 [Batch 状态和错误代码](https://docs.microsoft.com/rest/api/batchservice/batch-status-and-error-codes)。

## <a name="when-to-retry"></a>何时重试

如果出现错误，Batch API 会通知你。 这些 Batch API 都可以重试，并且都包含一个全局重试处理程序。 最好使用此内置机制。

失败后，应等待一段时间（重试间隔为几秒钟），然后重试。 如果重试的次数过多或速度过快，重试处理程序会中止。

### <a name="for-more-information"></a>更多信息  

[Batch API 和工具](batch-apis-tools.md)会链接到 API 参考信息。 例如，.NET API 包含 [RetryPolicyProvider 类]( https://docs.azure.cn/dotnet/api/microsoft.azure.batch.retrypolicyprovider?view=azure-dotnet)，应在其中指定所需的重试策略。 

有关每个 API 及其默认重试策略的详细信息，请参阅 [Batch 状态和错误代码](https://docs.microsoft.com/rest/api/batchservice/batch-status-and-error-codes)。
