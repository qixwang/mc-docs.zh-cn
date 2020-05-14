---
title: 将应用程序和数据复制到池节点
description: 了解如何将应用程序和数据复制到池节点。
services: batch
author: LauraBrenner
manager: evansma
ms.assetid: ''
ms.service: batch
ms.topic: article
ms.tgt_pltfrm: ''
ms.workload: big-compute
origin.date: 02/17/2020
ms.date: 04/27/2020
ms.author: v-tawe
ms.openlocfilehash: 791e2bbe3048dc7650be932b2ca9713a3f03ca3e
ms.sourcegitcommit: 1fbdefdace8a1d3412900c6c3f89678d8a9b29bc
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/07/2020
ms.locfileid: "82886998"
---
# <a name="copying-applications-and-data-to-pool-nodes"></a>将应用程序和数据复制到池节点

Azure Batch 支持使用多种方法将数据和应用程序获取到计算节点上，以便任务可以使用这些数据和应用程序。 数据和应用程序可能是运行整个作业所必需的，因此需要安装在每个节点上。 有些可能仅是特定任务所需的，或者需要为作业安装，但不需要在每个节点上都安装。 Batch 针对上述每个场景都提供了相应的工具。

- **池启动任务资源文件**：适用于需要在池中的每个节点上安装的应用程序或数据。 将此方法与应用程序包或启动任务的资源文件集合一起使用，以便执行安装命令。  

示例: 
- 使用启动任务命令行来移动或安装应用程序
- 指定 Azure 存储帐户中的特定文件或容器的列表。 有关详细信息，请参阅[在 REST 文档中添加 ResourceFile](https://docs.microsoft.com/rest/api/batchservice/pool/add#resourcefile)
- 在池中运行的每个作业都运行 MyApplication.exe，必须先使用 MyApplication.msi 安装该程序。 如果使用此机制，则需将启动任务的“等待成功”属性设置为“true”。   有关详细信息，请参阅[在 REST 文档中添加 StartTask](https://docs.microsoft.com/rest/api/batchservice/pool/add#starttask)。

- **任务资源文件**：适用于应用程序或数据仅与单个任务相关的情况。 例如：你有五个任务，每个任务处理一个不同的文件，然后将输出写入到 blob 存储。  在这种情况下，应在“任务资源文件”  集合上指定输入文件，因为每个任务都有其自己的输入文件。
## <a name="determine-the-scope-required-of-a-file"></a>确定文件的所需作用域

需要确定文件的作用域 - 该文件是池所需的、作业所需的还是任务所需的。 作用域为池的文件应当使用池应用程序包或某个启动任务。 作用域为作业的文件应当使用某个作业准备任务。 应用程序是作用域为池或作业级别的文件的典型示例。 作用域为任务的文件应当使用任务资源文件。

### <a name="other-ways-to-get-data-onto-batch-compute-nodes"></a>将数据获取到 Batch 计算节点的其他方式

还可通过其他方式将数据获取到 Batch 计算节点，这些方式未正式集成到 Batch REST API 中。 因为你可以控制 Azure Batch 节点，并且可以运行自定义的可执行文件，所以，只要 Batch 节点连接到目标，并且你拥有 Azure Batch 节点上的该源的凭据，就可以从任意数量的自定义源中拉取数据。 一些常见示例包括：

- 通过 SQL 下载数据
- 从其他 Web 服务/自定义位置下载数据
- 映射网络共享

### <a name="azure-storage"></a>Azure 存储

Blob 存储有下载可伸缩性目标。 Azure 存储文件共享的可伸缩性目标与单个 blob 的相同。 大小会影响所需的节点和池的数量。
