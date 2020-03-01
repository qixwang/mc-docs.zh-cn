---
title: 将作业和任务输出持久保存到数据存储 - Azure Batch
description: 了解如何通过不同的选项来持久保存 Batch 任务和作业的输出数据。 可以将数据持久保存到 Azure 存储或其他数据存储。
services: batch
author: lingliw
manager: digimobile
editor: ''
ms.assetid: 16e12d0e-958c-46c2-a6b8-7843835d830e
ms.service: batch
ms.topic: article
ms.tgt_pltfrm: ''
ms.workload: big-compute
origin.date: 11/14/2018
ms.date: 11/26/2018
ms.author: v-lingwu
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: b92a8ae161fb5640a712f54935db1fd9845de83a
ms.sourcegitcommit: 27eaabd82b12ad6a6840f30763034a6360977186
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/20/2020
ms.locfileid: "77497367"
---
# <a name="persist-job-and-task-output"></a>持久保存作业和任务输出

[!INCLUDE [batch-task-output-include](../../includes/batch-task-output-include.md)]

任务输出的一些常见示例包括：

- 任务处理输入数据时创建的文件。
- 与任务执行情况关联的日志文件。

本文介绍用于持久保存任务输出的各个选项。

## <a name="options-for-persisting-output"></a>持久保存输出的选项

可以根据方案通过多种不同的方法来持久保存任务输出：

- [使用 Batch 服务 API](batch-task-output-files.md)。  
- [使用适用于 .NET 的 Batch 文件约定库](batch-task-output-file-conventions.md)。  
- 在应用程序中实现 Batch 文件约定标准。
- 实现自定义文件移动解决方案。

以下各部分简要介绍每个方法以及持久保存输出的一般设计注意事项。

### <a name="use-the-batch-service-api"></a>使用 Batch 服务 API

Batch 服务支持在[向作业添加任务](https://docs.microsoft.com/rest/api/batchservice/add-a-task-to-a-job)或[向作业添加任务集合](https://docs.microsoft.com/rest/api/batchservice/add-a-collection-of-tasks-to-a-job)时指定任务数据在 Azure 存储中的输出文件。

若要详细了解如何使用 Batch 服务 API 来持久保存任务输出，请参阅[使用 Batch 服务 API 将任务数据持久保存到 Azure 存储](batch-task-output-files.md)。

### <a name="use-the-batch-file-conventions-library-for-net"></a>使用适用于 .NET 的 Batch 文件约定库

Batch 定义了一组可选的约定，用于命名 Azure 存储中的任务输出文件。 [Batch 文件约定标准](https://github.com/Azure/azure-sdk-for-net/tree/master/sdk/batch/Microsoft.Azure.Batch.Conventions.Files#conventions)介绍了这些约定。 如果给定的输出文件取决于作业和任务的名称，则文件约定标准决定了 Azure 存储中目标容器和 Blob 路径的名称。

你可以自行决定是否使用文件约定标准来命名输出数据文件。 你还可以任意命名目标容器和 Blob。 如果使用文件约定标准来命名输出文件，则可在 [Azure 门户][portal]中查看输出文件。

通过 C# 和 .NET 生成 Batch 解决方案的开发人员可以使用[适用于 .NET 的文件约定库][nuget_package]，按 [Batch 文件约定标准](https://github.com/Azure/azure-sdk-for-net/tree/master/sdk/batch/Microsoft.Azure.Batch.Conventions.Files#conventions)将任务数据持久保存到 Azure 存储帐户。 文件约定库负责将输出文件移至 Azure 存储，并以已知方式对目标容器和 Blob 命名。

有关使用适用于 .NET 的文件约定库保存任务输出的详细信息，请参阅[使用适用于 .NET 的 Batch 文件约定库将作业和任务数据保存到 Azure 存储](batch-task-output-file-conventions.md)。

### <a name="implement-the-batch-file-conventions-standard"></a>实现 Batch 文件约定标准

如果使用 .NET 之外的语言，则可在自己的应用程序中实现 [Batch 文件约定标准](https://github.com/Azure/azure-sdk-for-net/tree/master/sdk/batch/Microsoft.Azure.Batch.Conventions.Files#conventions)。

如果需要经验证的命名方案，或者需要在 Azure 门户中查看任务输出，则可能需要自行实现文件约定命名标准。

### <a name="implement-a-custom-file-movement-solution"></a>实现自定义文件移动解决方案

也可实现你自己的完整的文件移动解决方案。 以下情况可以使用此方法：

- 需将任务数据持久保存到 Azure 存储之外的其他数据存储。 若要将文件上传到 Azure SQL 或 Azure DataLake 之类的数据存储，可以创建一个自定义脚本或可执行文件，以便将文件上传到该位置。 运行主要的可执行文件以后，即可在命令行中进行调用。 例如，可以在 Windows 节点上调用以下两个命令：`doMyWork.exe && uploadMyFilesToSql.exe`
- 需对初始结果执行检查点或提前上传操作。
- 需对错误处理保持精细控制。 例如，如果需要使用任务依赖关系操作根据特定的任务退出代码来执行某些上传操作，则可能需要实现你自己的解决方案。 有关任务依赖关系操作的详细信息，请参阅[创建任务依赖关系，以运行依赖于其他任务的任务](batch-task-dependencies.md)。

## <a name="design-considerations-for-persisting-output"></a>持久保存输出的设计注意事项

设计 Batch 解决方案时，请考虑以下与作业和任务输出相关的因素。

- **计算节点生存期**：计算节点通常是瞬态的，尤其是在启用了自动缩放的池中。 在某个节点上运行的任务的输出仅在该节点存在时才可用，并且仅在为任务设置的文件保留期内可用。 如果在任务完成以后，可能需要任务生成的输出，则该任务必须将其输出文件上传到某个持久性存储，例如 Azure 存储。

- **输出存储**：建议将 Azure 存储用作任务输出的数据存储，但可使用任意持久存储。 将任务输出写入 Azure 存储的功能已集成到 Batch 服务 API 中。 如果使用其他形式的持久性存储，则需编写应用程序逻辑，自行持久保存任务输出。

- **输出检索**：可以直接从池中的计算节点检索任务输出；如果已保存任务输出，则可以从 Azure 存储或其他数据存储检索任务输出。 若要直接从计算节点检索任务输出，需要获取文件名及其在节点上的输出位置。 如果将任务输出持久保存到 Azure 存储，则需获得 Azure 存储中文件的完整路径，然后才能使用 Azure 存储 SDK 下载输出文件。

- **查看输出**：导航到 Azure 门户中的某个 Batch 任务并选择“节点上的文件”  时，将看到与该任务关联的所有文件，而不仅仅是你想要查看的输出文件。 同样，计算节点上的文件仅在该节点存在时才可用，并且仅在为任务设置的文件保留时间范围内才可用。 若要查看已持久保存到 Azure 存储的任务输出，可以使用 Azure 门户，也可以使用 Azure 存储客户端应用程序，例如 [Azure 存储资源管理器][storage_explorer]。 若要使用门户或其他工具查看 Azure 存储中的输出数据，必须知道文件的位置，然后直接导航到该位置。

## <a name="next-steps"></a>后续步骤

- 在[使用 Batch 服务 API 将任务数据持久保存到 Azure 存储](batch-task-output-files.md)一文中，了解如何使用 Batch 服务 API 中的新功能来持久保存任务数据。
- 在[使用适用于 .NET 的 Batch 文件约定库将作业和任务数据保存到 Azure 存储](batch-task-output-file-conventions.md)中，了解如何使用适用于 .NET 的 Batch 文件约定库。
- 请参阅 GitHub 上的 [PersistOutputs][github_persistoutputs] 示例项目，该示例演示了如何使用适用于 .NET 的 Batch 客户端库和适用于 .NET 的文件约定库将任务输出保存到持久存储。

[nuget_package]: https://www.nuget.org/packages/Microsoft.Azure.Batch.Conventions.Files
[portal]: https://portal.azure.cn
[storage_explorer]: http://storageexplorer.com/
[github_persistoutputs]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp/ArticleProjects/PersistOutputs 

<!-- Update_Description: link update -->