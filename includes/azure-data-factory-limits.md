---
title: include 文件
description: include 文件
services: data-factory
author: linda33wj
ms.service: data-factory
ms.topic: include
ms.date: 5/30/2019
ms.author: jingwang
ms.custom: include file
ms.openlocfilehash: 60a6d3b1edd4efc9701d38f4c43b1ca36cb699f9
ms.sourcegitcommit: c1ba5a62f30ac0a3acb337fb77431de6493e6096
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2020
ms.locfileid: "71151853"
---
Azure 数据工厂是一项多租户服务，具有以下默认限制，目的是确保客户订阅不受彼此工作负荷的影响。 若要将限制提高到订阅的最大值，请联系支持人员。

### <a name="version-2"></a>版本 2

| 资源 | 默认限制 | 最大限制 |
| -------- | ------------- | ------------- |
| Azure 订阅中的数据工厂 | 50 | 请[联系支持人员](https://support.azure.cn/zh-cn/support/support-azure/)。 |
| 数据工厂中的实体（例如管道、数据集、触发器、链接服务以及集成运行时）总数 | 5,000 | 请[联系支持人员](https://support.azure.cn/zh-cn/support/support-azure/)。 |
| 一个订阅中 Azure-SSIS Integration Runtime 的总 CPU 核心数 | 256 | 请[联系支持人员](https://support.azure.cn/zh-cn/support/support-azure/)。 |
| 每个数据工厂的并行管道运行数，这些运行在该工厂中的所有管道之间共享 | 10,000  | 请[联系支持人员](https://support.azure.cn/zh-cn/support/support-azure/)。 |
| 每个 [Azure Integration Runtime 区域](../articles/data-factory/concepts-integration-runtime.md#integration-runtime-location)每个订阅的并发外部活动运行数<br><small>外部活动在集成运行时上托管，但在链接服务（包括存储过程、HDInsights 等）上执行。</small> | 3000 | 请[联系支持人员](https://support.azure.cn/zh-cn/support/support-azure/)。 |
| 每个 [Azure Integration Runtime 区域](../articles/data-factory/concepts-integration-runtime.md#integration-runtime-location)每个订阅的并发管道活动运行数 <br><small>管道活动在集成运行时上执行，包括 Lookup、GetMetadata 和 Delete。</small>| 1000 | 请[联系支持人员](https://support.azure.cn/zh-cn/support/support-azure/)。 |
| 每个 [Azure Integration Runtime 区域](../articles/data-factory/concepts-integration-runtime.md#integration-runtime-location)每个订阅的并发创作操作数<br><small>包括测试连接、浏览文件夹列表和表列表，以及预览数据。 | 200 | 请[联系支持人员](https://support.azure.cn/zh-cn/support/support-azure/)。 |
| 每个管道的最大活动数（包括容器的内部活动） | 40 | 40 |
| 可以针对单个自承载集成运行时创建的链接集成运行时的最大数量。 | 100 | 请[联系支持人员](https://support.azure.cn/zh-cn/support/support-azure/)。 |
| 每个管道的最大参数个数 | 50 | 50 |
| ForEach 项 | 100,000 | 100,000 |
| ForEach 并行度 | 20 | 50 |
| 每个表达式的字符数 | 8,192 | 8,192 |
| 最小翻转窗口触发器间隔 | 15 分钟 | 15 分钟 |
| 管道活动运行的最长超时时间 | 7 天 | 7 天 |
| 管道对象的每对象字节数<sup>3</sup> | 200 KB | 200 KB |
| 数据集和链接服务对象的每对象字节数<sup>3</sup> | 100 KB | 2000 KB |
| 每个复制活动运行的数据集成单位<sup>1</sup> | 256 | 请[联系支持人员](https://support.azure.cn/zh-cn/support/support-azure/)。 |
| 编写 API 调用 | 1200/h<br/><br/> 此限制是由 Azure 资源管理器而不是 Azure 数据工厂所强加的。 | 请[联系支持人员](https://support.azure.cn/zh-cn/support/support-azure/)。 |
| 读取 API 调用 | 12500/h<br/><br/> 此限制是由 Azure 资源管理器而不是 Azure 数据工厂所强加的。 | 请[联系支持人员](https://support.azure.cn/zh-cn/support/support-azure/)。 |
| 每分钟监视的查询数 | 1,000 | 请[联系支持人员](https://support.azure.cn/zh-cn/support/support-azure/)。 |
| 每分钟的实体 CRUD 操作数 | 50 | 请[联系支持人员](https://support.azure.cn/zh-cn/support/support-azure/)。 |

<sup>1</sup> 数据集成单元 (DIU) 在云到云复制操作中使用，请从[数据集成单元（版本 2）](../articles/data-factory/copy-activity-performance.md#data-integration-units)了解更多信息。 有关计费的信息，请参阅 [Azure 数据工厂定价](https://www.azure.cn/pricing/details/data-factory/)。

<sup>3</sup> 管道、数据集和链接服务对象代表工作负荷的逻辑组。 对这些对象的限制与可以使用 Azure 数据工厂移动或处理的数据量无关。 数据工厂已设计为可以扩展，以便处理数千万亿字节的数据。

