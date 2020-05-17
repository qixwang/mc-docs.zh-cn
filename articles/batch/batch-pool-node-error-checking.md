---
title: 检查池和节点错误 - Azure Batch
description: 本文介绍在创建池和节点时可能发生的后台操作、要检查的错误以及如何避免这些错误。
services: batch
ms.service: batch
author: mscurrell
ms.author: v-tawe
origin.date: 08/23/2019
ms.date: 04/27/2020
ms.topic: conceptual
ms.openlocfilehash: 905950972a7baf485097366ad1c49ce0a37ffd39
ms.sourcegitcommit: 1fbdefdace8a1d3412900c6c3f89678d8a9b29bc
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/07/2020
ms.locfileid: "82886891"
---
# <a name="check-for-pool-and-node-errors"></a>检查池和节点错误

创建和管理 Azure Batch 池时，某些操作是即时发生的。 但是，某些操作是异步的且在后台运行，需要数分钟才能完成。

检测即时发生的失败操作非常简单，因为 API、CLI 或 UI 会立即返回任何错误。

本文介绍可对池和池节点执行的后台操作， 其中说明了如何检测和避免错误。

## <a name="pool-errors"></a>池错误

### <a name="resize-timeout-or-failure"></a>调整大小超时或失败

创建新池或调整现有池大小时，需指定目标节点数。  创建或重设大小操作是即时完成的，但实际分配新节点或删除现有节点可能需要几分钟时间。  在 [create](https://docs.microsoft.com/rest/api/batchservice/pool/add) 或 [resize](https://docs.microsoft.com/rest/api/batchservice/pool/resize) API 中指定大小调整超时。 如果 Batch 在大小调整超时期限内无法获取目标节点数，池会进入稳定状态并报告重设大小错误。

最近执行的评估的 [ResizeError](https://docs.microsoft.com/rest/api/batchservice/pool/get#resizeerror) 属性会列出发生的错误。

重设大小错误的常见原因包括：

- 调整大小超时过短
  - 在大多数情况下，默认超时 15 分钟已足够长，在此期限内可以完成池节点的分配或删除。
  - 若要分配大量的节点，我们建议将大小调整超时设置为 30 分钟。 例如，要在 Azure 市场映像中将大小调整为 1,000 个以上的节点，或者在自定义 VM 映像中将大小调整为 300 个以上的节点时。
- 核心配额不足
  - Batch 帐户在所有池中可分配的核心数有限制。 一旦达到该配额，Batch 就会停止分配节点。 [可以提高](/batch/batch-quota-limit)核心配额，使 Batch 能够分配更多的节点。
- 当[池处于虚拟网络](https://docs.azure.cn/batch/batch-virtual-network)，子网 IP 不足
  - 虚拟网络子网必须能够提供可分配到每个请求的池节点的、尚未分配的足够 IP 地址。 否则无法创建节点。
- 当[池处于虚拟网络](https://docs.azure.cn/batch/batch-virtual-network)，资源不足
  - 你可能会在 Batch 帐户所在的同一订阅中创建负载均衡器、公共 IP 和网络安全组等资源。 请检查这些资源的订阅配额是否足够。
- 使用自定义 VM 映像的大型池
  - 使用自定义 VM 映像的大型池可能需要更长的时间进行分配，并且可能发生大小调整超时。  有关限制和配置的建议，请参阅[使用共享映像库创建池](batch-sig-images.md)。

### <a name="automatic-scaling-failures"></a>自动缩放功能

也可以将 Azure Batch 设置为自动缩放池中的节点数。 定义[池自动缩放公式](https://docs.azure.cn/batch/batch-automatic-scaling)的参数。 Batch 服务使用该公式定期评估池中的节点数，并设置新的目标数。 可能会发生以下类型问题：

- 自动缩放评估失败。
- 生成的大小调整操作失败和超时。
- 自动缩放公式的问题导致节点目标值不正确。 大小调整操作要么正常运行，要么超时。

可以使用 [autoScaleRun](https://docs.microsoft.com/rest/api/batchservice/pool/get#autoscalerun) 属性获取有关上次自动缩放评估的信息。 此属性将报告评估时间、值和结果，以及任何性能错误。

[池大小调整完成事件](https://docs.azure.cn/batch/batch-pool-resize-complete-event)捕获有关所有评估的信息。

### <a name="delete"></a>Delete

当你删除包含节点的池时，Batch 首先会删除节点， 然后再删除池对象本身。 可能需要等待数分钟才会删除池节点。

在删除过程中，Batch 会将[池状态](https://docs.microsoft.com/rest/api/batchservice/pool/get#poolstate)设置为 **deleting**。 调用方应用程序可以使用 **state** 和 **stateTransitionTime** 属性来检测池删除时间是否过长。

## <a name="pool-compute-node-errors"></a>池计算节点错误

即使 Batch 在池中成功分配了节点，各种问题仍可能会导致某些节点不正常，无法运行任务。 这些节点仍会产生费用，因此务必要检测问题，避免不能使用的节点产生费用。 除了常见的节点错误以外，了解当前的[作业状态](https://docs.microsoft.com/rest/api/batchservice/job/get#jobstate)对于故障排除也很有帮助。

### <a name="start-task-failures"></a>启动任务失败

你可能想要为某个池指定可选的[启动任务](https://docs.microsoft.com/rest/api/batchservice/pool/add#starttask)。 与指定任何任务一样，可以使用命令行和资源文件从存储下载。 启动任务在启动后，将针对每个节点运行。 **waitForSuccess** 属性指定 Batch 是否要等到启动任务成功完成，然后才为节点计划任何任务。

如果已将节点配置为等待启动任务成功完成，但启动任务失败，将会发生什么情况？ 在这种情况下，该节点将不可用，但仍会产生费用。

可以使用顶级 [startTaskInfo](https://docs.microsoft.com/rest/api/batchservice/computenode/get#starttaskinformation) 节点属性的 [result](https://docs.microsoft.com/rest/api/batchservice/computenode/get#taskexecutionresult) 和 [failureInfo](https://docs.microsoft.com/rest/api/batchservice/computenode/get#taskfailureinformation) 属性来检测启动任务失败。

如果已将 **waitForSuccess** 设置为 **true**，启动任务失败还会导致 Batch 将节点[状态](https://docs.microsoft.com/rest/api/batchservice/computenode/get#computenodestate)设置为 **starttaskfailed**。

与任何任务一样，开始任务失败可能有许多原因。  若要进行故障排除，请检查 stdout、stderr 和其他任何特定于任务的日志文件。

启动任务必须可重入，因为启动任务可能会在同一节点上多次运行；节点重置映像或重启时，启动任务就会运行。 在罕见情况下，启动任务会在某个事件导致节点重启后运行，此时某个操作系统或临时磁盘已重置映像，而另一个并没有这样做。 由于 Batch 启动任务（与所有 Batch 任务一样）从临时磁盘运行，因此通常这不是一个问题。但在某些情况下，启动任务将应用程序安装到操作系统磁盘，而将其他数据保留在临时磁盘上，这就可能会导致问题，因为会出现不同步现象。如果使用两种磁盘，请对应用程序进行相应的保护。

### <a name="application-package-download-failure"></a>应用程序包下载失败

可为某个池指定一个或多个应用程序包。 在启动节点之后，在对任务进行计划之前，Batch 会将指定的包文件下载到每个节点并解压缩这些文件。 通常会将启动任务命令行与应用程序包结合使用。 例如，将文件复制到其他位置，或运行安装程序。

节点 [errors](https://docs.microsoft.com/rest/api/batchservice/computenode/get#computenodeerror) 属性会报告下载和解压缩应用程序包失败；节点状态会设置为 **unusable**。

### <a name="container-download-failure"></a>容器下载失败

可以在池上指定一个或多个容器引用。 Batch 可将指定的容器下载到每个节点。 节点的 [errors](https://docs.microsoft.com/rest/api/batchservice/computenode/get#computenodeerror) 属性报告容器下载失败，并将节点状态设置为“不可用”  。

### <a name="node-in-unusable-state"></a>处于“不可用”状态的节点

Azure Batch 可能出于多种原因将[节点状态](https://docs.microsoft.com/rest/api/batchservice/computenode/get#computenodestate)设置为 **unusable**。 将节点状态设置为 **unusable** 后，无法为该节点计划任务，但它仍会产生费用。

节点处于“unusable”状态但  没有 [errors](https://docs.microsoft.com/rest/api/batchservice/computenode/get#computenodeerror)，这意味着 Batch 无法与 VM 通信。 在这种情况下，Batch 始终尝试恢复 VM。 Batch 不会自动尝试恢复无法安装应用程序包或容器的 VM，即使这些 VM 的状态为“unusable”  。

如果 Batch 可以确定原因，则节点 [errors](https://docs.microsoft.com/rest/api/batchservice/computenode/get#computenodeerror) 属性会报告此原因。

节点**不可用**的其他原因示例包括：

- 自定义 VM 映像无效。 例如，未正确准备某个映像。

- 由于基础结构故障或低级别升级而移动了 VM。 Batch 将恢复节点。

- VM 映像部署在不支持它的硬件上。 例如，尝试在 [Standard_D1_v2](../virtual-machines/dv2-dsv2-series.md) VM 上运行 CentOS HPC 映像。

- VM 位于 [Azure 虚拟网络](batch-virtual-network.md)中，并且流量已被阻止，无法发送到关键端口。

- VM 位于虚拟网络中，但流向 Azure 存储的出站流量被阻止。

- VM 位于使用客户 DNS 配置的虚拟网络中，DNS 服务器不能解析 Azure 存储。

### <a name="node-agent-log-files"></a>节点代理日志文件

每个池节点上运行的 Batch 代理进程可以提供日志文件。如果你需要就池节点问题联系支持人员，这些日志文件可能很有帮助。 可以通过 Azure 门户、Batch Explorer 或 [API](https://docs.microsoft.com/rest/api/batchservice/computenode/uploadbatchservicelogs) 上传节点的日志文件。 最好是上传并保存日志文件。 以后，可以删除节点或池，以节省运行中节点的成本。

### <a name="node-disk-full"></a>节点磁盘已满

Batch 使用池节点 VM 的临时驱动器来存储作业文件、任务文件和共享文件。

- 应用程序包文件
- 任务资源文件
- 下载到某个 Batch 文件夹的特定于应用程序的文件
- 每次用于执行任务应用程序的 stdout 和 stderr 文件
- 特定于应用程序的输出文件。其中某些文件（例如，池应用程序包或池启动任务资源文件）仅在创建池节点时写入一次。 即使是仅在创建节点时写入一次，但如果这些文件太大，它们也可能会填满临时驱动器。

将为在节点上运行的每个任务写出其他文件，例如 stdout 和 stderr。 如果在同一节点上运行大量任务并且/或者任务文件太大，这些文件可能会填满临时驱动器。

临时驱动器的大小取决于 VM 大小。 选择 VM 大小时，一个注意事项是确保临时驱动器具有足够的空间。
- 在 Azure 门户中添加池时，可以显示 VM 大小的完整列表，其中包含一个“资源磁盘大小”列。
- 介绍所有 VM 大小的文章（例如[计算优化的 VM 大小](/virtual-machines/windows/sizes-compute)）中提供了包含“临时存储”列的表格。对于确定任务文件在被系统自动清理之前要保留多长时间的每个任务，可以指定保留时间。 可以缩短保留时间以降低存储需求。
如果临时磁盘的空间耗尽（或者快要耗尽），节点将转换为[不可用](https://docs.microsoft.com/rest/api/batchservice/computenode/get#computenodestate)状态，并且会报告节点错误，指出磁盘已满。
### <a name="what-to-do-when-a-disk-is-full"></a>磁盘已满时怎么办
确定磁盘已满的原因：如果不确定哪些内容占用了节点上的空间，建议通过远程方式连接到节点，并手动调查空间耗尽的原因。 还可以利用 [Batch 列出文件 API](https://docs.microsoft.com/rest/api/batchservice/file/listfromcomputenode) 来检查 Batch 托管文件夹中的文件（例如任务输出）。 请注意，此 API 只会列出 Batch 托管目录中的文件。如果任务在其他位置创建了文件，则你看不到这些文件。
确保已从节点中检索到所需的数据或已将其上传到持久存储。 磁盘填满问题的所有缓解措施都涉及到通过删除数据来释放空间。
### <a name="recovering-the-node"></a>恢复节点
1. 如果你的池是 [C.loudServiceConfiguration](https://docs.microsoft.com/rest/api/batchservice/pool/add#cloudserviceconfiguration) 池，可以通过 [Batch 重置映像 API](https://docs.microsoft.com/rest/api/batchservice/computenode/reimage) 来重置节点的映像。这会清理整个磁盘。 [VirtualMachineConfiguration](https://docs.microsoft.com/rest/api/batchservice/pool/add#virtualmachineconfiguration) 池目前不支持重置映像。
2. 如果你的池是 [VirtualMachineConfiguration](https://docs.microsoft.com/rest/api/batchservice/pool/add#virtualmachineconfiguration)，可以使用[删除节点 API](https://docs.microsoft.com/rest/api/batchservice/pool/removenodes) 从池中删除节点。 然后，可以再次增大池，以将不良的节点替换为全新的节点。
3.  删除其任务数据仍保留在节点上的旧的已完成作业或旧的已完成任务。 若要了解哪些作业/任务数据会保留在节点上，可以查看节点上的 [RecentTasks 集合](https://docs.microsoft.com/rest/api/batchservice/computenode/get#taskinformation)或[节点上的文件](https://docs.microsoft.com//rest/api/batchservice/file/listfromcomputenode)。 删除作业会删除该作业中的所有任务，而删除作业中的任务会触发所要删除节点上的任务目录中的数据，从而释放空间。 释放足够的空间后，重新启动该节点，然后，该节点会摆脱“不可用”状态，再次进入“空闲”状态。
## <a name="next-steps"></a>后续步骤

检查是否已将应用程序设置为实施全面的错误检查，尤其是对于异步操作。 及时检测和诊断问题有时是很关键的。
