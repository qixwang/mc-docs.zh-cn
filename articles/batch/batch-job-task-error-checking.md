---
title: 检查作业和任务错误 - Azure Batch | Microsoft Docs
description: 要检查的错误以及对作业和任务进行故障排除
services: batch
author: mscurrell
ms.service: batch
ms.topic: article
ms.date: 04/29/2020
ms.author: markscu
origin.date: 03/10/2019
ms.openlocfilehash: 5651f02ac12b5364fd61807d4af5a73fad7db214
ms.sourcegitcommit: 1fbdefdace8a1d3412900c6c3f89678d8a9b29bc
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/07/2020
ms.locfileid: "82886990"
---
# <a name="job-and-task-error-checking"></a>作业和任务错误检查

添加作业和任务时，可能会发生各种错误。 检测这些操作的失败非常简单，因为 API、CLI 或 UI 会立即返回任何失败结果。  但是，以后在计划并运行作业和任务时，也可能会发生失败。

本文介绍了在提交作业和任务后可能会发生的错误。 其中列出并解释了需要检查和处理的错误。

## <a name="jobs"></a>作业

作业是包含一个或多个任务的分组，任务实际指定了要运行的命令行。

添加作业时，可以指定以下参数来影响作业的失败方式：

- [作业约束](https://docs.microsoft.com/rest/api/batchservice/job/add#jobconstraints)
  - 可以选择性地指定 `maxWallClockTime` 属性，以设置作业可以处于活动或运行状态的最长时间。 如果超过此时间，则会根据作业的 [executionInfo](https://docs.microsoft.com/rest/api/batchservice/job/get#cloudjob) 中设置的 `terminateReason` 属性终止作业。
- [作业准备任务](https://docs.microsoft.com/rest/api/batchservice/job/add#jobpreparationtask)
  - 如果指定了此项，则首次在节点上为某个作业运行任务时，会运行作业准备任务。 作业准备任务可能会失败，导致任务不会运行并且作业不会完成。
- [作业释放任务](https://docs.microsoft.com/rest/api/batchservice/job/add#jobreleasetask)
  - 仅当配置了作业准备任务时，才能指定作业释放任务。 终止某个作业时，将会在运行了作业准备任务的每个池节点上运行作业释放任务。 作业释放任务可能会失败，但作业仍会转换为 `completed` 状态。

### <a name="job-properties"></a>作业属性

应检查以下作业属性来查找错误：

- '[executionInfo](https://docs.microsoft.com/rest/api/batchservice/job/get#jobexecutioninformation)'：
  - `terminateReason` 属性可以包含相应的值来指示是否已超出作业约束中指定的 `maxWallClockTime`，因而终止了作业。 还可以设置此属性来指示任务失败（如果正确设置了作业 `onTaskFailure` 属性）。
  - 如果发生了调度错误，将会设置 [schedulingError](https://docs.microsoft.com/rest/api/batchservice/job/get#jobschedulingerror) 属性。
 
### <a name="job-preparation-tasks"></a>作业准备任务

如果为某个作业指定了作业准备任务，则首次在节点上运行该作业的任务时，将运行作业准备任务的一个实例。 可将作业中配置的作业准备任务视为任务模板，其中包含所要运行的多个作业准备任务实例（实例数目不超过池中的节点数）。

应检查作业准备任务实例以确定是否存在错误：
- 运行作业准备任务时，触发作业准备任务的任务将转换为 `preparing` [状态](https://docs.microsoft.com/rest/api/batchservice/task/get#taskstate)；如果作业准备任务随后失败，则触发任务将还原为 `active` 状态且不会运行。  
- 可以使用[列出准备和释放任务状态](https://docs.microsoft.com/rest/api/batchservice/job/listpreparationandreleasetaskstatus) API 从作业中获取已运行的作业准备任务的所有实例。 与任何任务一样，可以使用 `failureInfo`、`exitCode` 和 `result` 等属性获取[执行信息](https://docs.microsoft.com/rest/api/batchservice/job/listpreparationandreleasetaskstatus#jobpreparationandreleasetaskexecutioninformation)。
- 如果作业准备任务失败，则触发作业任务不会运行，作业不会完成且会停滞。 如果没有任何其他作业包含可调度的任务，则池可能会进入未利用状态。

### <a name="job-release-tasks"></a>作业释放任务

如果为某个作业指定了作业释放任务，当终止某个作业时，将在运行了作业准备任务的每个池节点上运行作业释放任务的实例。  应检查作业释放任务实例以确定是否存在错误：
- 可以使用[列出准备和释放任务状态](https://docs.microsoft.com/rest/api/batchservice/job/listpreparationandreleasetaskstatus) API 从作业中获取正在运行的作业释放任务的所有实例。 与任何任务一样，可以使用 `failureInfo`、`exitCode` 和 `result` 等属性获取[执行信息](https://docs.microsoft.com/rest/api/batchservice/job/listpreparationandreleasetaskstatus#jobpreparationandreleasetaskexecutioninformation)。
- 如果一个或多个作业释放任务失败，则该作业仍会终止并转换为 `completed` 状态。

## <a name="tasks"></a>任务

作业任务可能因多种原因而失败：

- 任务命令行失败，并返回非零退出代码。
- 为任务指定了 `resourceFiles`，但发生了表示未下载一个或多个文件的失败。
- 为任务指定了 `outputFiles`，但发生了表示未上传一个或多个文件的失败。
- 超出了任务[约束](https://docs.microsoft.com/rest/api/batchservice/task/add#taskconstraints)中的 `maxWallClockTime` 属性为任务指定的已用时间。

在所有情况下，都必须检查以下属性来查找错误以及有关错误的信息：
- 任务 [executionInfo](https://docs.microsoft.com/rest/api/batchservice/task/get#taskexecutioninformation) 属性包含用于提供有关错误的信息的多个属性。 [result](https://docs.microsoft.com/rest/api/batchservice/task/get#taskexecutionresult) 指示任务是否因任何原因而失败，`exitCode` 和 `failureInfo` 提供有关失败的详细信息。
- 任务不管是成功还是失败，都会转换为 `completed` [状态](https://docs.microsoft.com/rest/api/batchservice/task/get#taskstate)。

必须考虑任务失败对作业和所有任务依赖项的影响。  可为任务指定 [exitConditions](https://docs.microsoft.com/rest/api/batchservice/task/add#exitconditions) 属性，以配置针对依赖项和作业的操作。
- 对于依赖项，[DependencyAction](https://docs.microsoft.com/rest/api/batchservice/task/add#dependencyaction) 控制是要阻止还是运行依赖于失败任务的任务。
- 对于作业，[JobAction](https://docs.microsoft.com/rest/api/batchservice/task/add#jobaction) 控制失败的任务是导致作业被禁用、终止还是保持不变。

### <a name="task-command-line-failures"></a>任务命令行失败

运行任务命令行时，输出将写入到 `stderr.txt` 和 `stdout.txt`。 此外，应用程序还可以写入到应用程序特定的日志文件。

如果运行任务的池节点仍然存在，则可以获取和查看日志文件。 例如，Azure 门户会列出任务或池节点的日志文件供审查。 多个 API（例如[从任务获取](https://docs.microsoft.com/rest/api/batchservice/file/getfromtask)）还允许列出和获取任务文件。

由于池和池节点往往是暂时性的，如果需要持续添加和删除节点，则我们建议保留日志文件。 可以通过[任务输出文件](https://docs.azure.cn/batch/batch-task-output-files)方便地将日志文件保存到 Azure 存储。

### <a name="output-file-failures"></a>输出文件失败
每次上传文件时，Batch 都会向计算节点写入两个日志文件，即 `fileuploadout.txt` 和 `fileuploaderr.txt`。 可以检查这些日志文件，详细了解具体的故障。 如果从未尝试过上传文件（例如，因任务本身无法运行而导致这种情况），则这些日志文件不会存在。  

## <a name="next-steps"></a>后续步骤

检查应用程序是否实施了全面的错误检查；及时检测和诊断问题有时至关重要。
