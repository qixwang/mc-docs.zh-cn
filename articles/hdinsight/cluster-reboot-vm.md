---
title: 重新启动 Azure HDInsight 群集的 VM
description: 了解如何重新启动 HDInsight 群集的无响应的 VM。
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.custom: hdinsightactive
ms.service: hdinsight
ms.topic: how-to
ms.date: 06/22/2020
ms.openlocfilehash: 58bb53cd3f3e3c542be3fc1daea906ba7aa6a9c1
ms.sourcegitcommit: ac70b12de243a9949bf86b81b2576e595e55b2a6
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/07/2020
ms.locfileid: "87919222"
---
# <a name="reboot-vms-for-hdinsight-cluster"></a>重新启动 HDInsight 群集的 VM

HDInsight 群集包含作为群集节点的 VM 组。 对于长时间运行的群集，这些节点可能会因各种原因而无法响应。 本文介绍如何在 HDInsight 群集中重新启动无响应的 VM。

## <a name="when-to-reboot"></a>何时重新启动

> [!WARNING]  
> 重新启动群集中的 VM 会使节点停机，并会在节点上重新启动服务。 

节点重新启动时，群集可能会运行不正常，作业运行速度可能会减慢或作业会失败。 如果你正在尝试重新启动活动头节点，则所有正在运行的作业都将终止，并且在服务重新启动并再次运行之前，你无法将作业提交到群集。 只应在必要时考虑重新启动 VM。 下面是有关何时考虑重启 VM 的一些指导。

- 你无法通过 SSH 连接到该节点，但它会响应 ping。
- 工作节点在 Ambari UI 中没有检测信号的情况下关闭。
- 节点上的临时磁盘已满。
- VM 上的进程表中有很多条目的进程已完成，但状态显示为“终止状态”。

> [!WARNING]  
> 重新启动 HBase 和 Kafka 集群的 VM 时应小心 ，因为这可能会导致数据丢失。

## <a name="use-powershell-to-reboot-vms"></a>使用 PowerShell 重新启动 VM

使用“节点重新启动”操作需要执行两个步骤：列出节点和重新启动节点。

1. 列出节点。 可以通过 [Get-AzHDInsightHost](https://docs.microsoft.com/powershell/module/az.hdinsight/get-azhdinsighthost) 获取群集节点列表。 

  ```
  Get-AzHDInsightHost -ClusterName myclustername
  ```

2. 重新启动主机。 获取想要重新启动的节点的名称后，请使用 [Restart-AzHDInsightHost](https://docs.microsoft.com/powershell/module/az.hdinsight/restart-azhdinsighthost) 重启节点。

  ```
  Restart-AzHDInsightHost -ClusterName myclustername -Name wn0-myclus, wn1-myclus
  ```

## <a name="use-rest-api-to-reboot-vms"></a>使用 REST API 重新启动 VM

可以使用 API 文档中的“试一试”功能将请求发送到 HDInsight 。 使用“节点重新启动”操作需要执行两个步骤：列出节点和重新启动节点。

1. 列出节点。 可以在 REST API 或 Ambari 中获取群集节点列表。 有关详细信息，请参阅 [HDInsight 列表主机 REST API 操作](https://docs.microsoft.com/rest/api/hdinsight/virtualmachines/listhosts)。

    ```
    POST https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.HDInsight/clusters/{clusterName}/listHosts?api-version=2018-06-01-preview
    ```

2. 重新启动主机。 获取想要重新启动的节点的名称后，请使用重新启动节点 REST API 重启节点。节点名称遵循“NodeType(wn/hn/zk/gw)”+“x”+“群集名称的前 6 个字符”的模式。 有关详细信息，请参阅 [HDInsight 重新启动主机 REST API 操作](https://docs.microsoft.com/rest/api/hdinsight/virtualmachines/restarthosts)。

    ```
    POST https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.HDInsight/clusters/{clusterName}/restartHosts?api-version=2018-06-01-preview
    ```

要重新启动的节点的实际名称在请求正文的 JSON 数组中指定。

```json
[
  "wn0-abcdef",
  "zk1-abcdef"
]
```

## <a name="next-steps"></a>后续步骤

* [Restart-AzHDInsightHost](https://docs.microsoft.com/powershell/module/az.hdinsight/restart-azhdinsighthost)
* [HDInsight 虚拟机 REST API](https://docs.microsoft.com/rest/api/hdinsight/virtualmachines)
* [HDInsight REST API](https://docs.microsoft.com/rest/api/hdinsight/)
