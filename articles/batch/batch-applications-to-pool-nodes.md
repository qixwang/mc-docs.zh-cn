---
title: 将应用程序和数据复制到池节点
description: 了解如何将应用程序和数据复制到池节点。
ms.topic: article
origin.date: 02/17/2020
ms.date: 04/27/2020
ms.author: v-tawe
ms.openlocfilehash: c65489d9d9c720298e5331b5df33597810b0d319
ms.sourcegitcommit: cbaa1aef101f67bd094f6ad0b4be274bbc2d2537
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/28/2020
ms.locfileid: "84126764"
---
# <a name="copying-applications-and-data-to-pool-nodes"></a>将应用程序和数据复制到池节点

Azure Batch 支持使用多种方法将数据和应用程序获取到计算节点上，以便任务可以使用这些数据和应用程序。 数据和应用程序可能是运行整个作业所必需的，因此需要安装在每个节点上。 有些可能仅是特定任务所需的，或者需要为作业安装，但不需要在每个节点上都安装。 Batch 针对上述每个场景都提供了相应的工具。

- **池启动任务资源文件**：适用于需要在池中的每个节点上安装的应用程序或数据。 将此方法与应用程序包或启动任务的资源文件集合一起使用，以便执行安装命令。  

示例: 
- 使用启动任务命令行来移动或安装应用程序

- 指定 Azure 存储帐户中的特定文件或容器的列表。 有关详细信息，请参阅[在 REST 文档中添加 ResourceFile](https://docs.microsoft.com/rest/api/batchservice/pool/add#resourcefile)

- 在池中运行的每个作业都运行 MyApplication.exe，必须先使用 MyApplication.msi 安装该程序。 如果使用此机制，则需将启动任务的“等待成功”属性设置为“true”。  有关详细信息，请参阅[在 REST 文档中添加 StartTask](https://docs.microsoft.com/rest/api/batchservice/pool/add#starttask)。

- 池上的**应用程序包引用**：适用于需要在池中的每个节点上安装的应用程序或数据。 没有与应用程序包关联的安装命令，但可以使用启动任务来运行任何安装命令。 如果你的应用程序不需要安装或包含大量文件，则可以使用此方法。 应用程序包非常适合于大量文件，因为它们会将大量文件引用合并到小型有效负载中。 如果尝试将超过 100 个单独的资源文件包含到一个任务中，则 Batch 服务可能会出现单个任务的内部系统限制。 另外，如果你有严格的版本要求，即可能有相同应用程序的许多不同版本，并且需要在它们之间进行选择，请使用应用程序包。 有关详细信息，请参阅[使用 Batch 应用程序包将应用程序部署到计算节点](https://docs.azure.cn/batch/batch-application-packages)。

- **作业准备任务资源文件**：适用于必须安装才能运行作业的应用程序或数据，但不需要在整个池上安装它们。 例如：如果你的池有许多不同类型的作业，并且只有一种作业类型需要 MyApplication 才能运行，则有必要将安装步骤放入作业准备任务。 有关作业准备任务的详细信息，请参阅[在 Batch 计算节点上运行作业准备和作业释放任务](https://docs.azure.cn/batch/batch-job-prep-release)。

- **任务资源文件**：适用于应用程序或数据仅与单个任务相关的情况。 例如：你有五个任务，每个任务处理一个不同的文件，然后将输出写入到 blob 存储。  在这种情况下，应在“任务资源文件”集合上指定输入文件，因为每个任务都有其自己的输入文件。

## <a name="determine-the-scope-required-of-a-file"></a>确定文件的所需作用域

需要确定文件的作用域 - 该文件是池所需的、作业所需的还是任务所需的。 作用域为池的文件应当使用池应用程序包或某个启动任务。 作用域为作业的文件应当使用某个作业准备任务。 应用程序是作用域为池或作业级别的文件的典型示例。 作用域为任务的文件应当使用任务资源文件。

### <a name="other-ways-to-get-data-onto-batch-compute-nodes"></a>将数据获取到 Batch 计算节点的其他方式

还可通过其他方式将数据获取到 Batch 计算节点，这些方式未正式集成到 Batch REST API 中。 因为你可以控制 Azure Batch 节点，并且可以运行自定义的可执行文件，所以，只要 Batch 节点连接到目标，并且你拥有 Azure Batch 节点上的该源的凭据，就可以从任意数量的自定义源中拉取数据。 一些常见示例包括：

- 通过 SQL 下载数据
- 从其他 Web 服务/自定义位置下载数据
- 映射网络共享

### <a name="azure-storage"></a>Azure 存储

Blob 存储有下载可伸缩性目标。 Azure 存储文件共享的可伸缩性目标与单个 blob 的相同。 大小会影响所需的节点和池的数量。

