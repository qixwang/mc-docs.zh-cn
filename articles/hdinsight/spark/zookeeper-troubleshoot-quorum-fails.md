---
title: Apache ZooKeeper 服务器无法在 Azure HDInsight 中形成仲裁
description: Apache ZooKeeper 服务器无法在 Azure HDInsight 中形成仲裁
ms.service: hdinsight
ms.topic: troubleshooting
author: hrasheed-msft
ms.author: v-yiso
ms.reviewer: jasonh
origin.date: 08/20/2019
ms.date: 12/23/2019
ms.openlocfilehash: 7a052610006f3c7f03fb89a1316031c24156db7d
ms.sourcegitcommit: 4a09701b1cbc1d9ccee46d282e592aec26998bff
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/25/2019
ms.locfileid: "75348543"
---
# <a name="apache-zookeeper-server-fails-to-form-a-quorum-in-azure-hdinsight"></a>Apache ZooKeeper 服务器无法在 Azure HDInsight 中形成仲裁

本文介绍在与 Azure HDInsight 群集交互时出现的问题的故障排除步骤和可能的解决方案。

## <a name="issue"></a>问题

Apache ZooKeeper 服务器运行不正常，症状可能包括：资源管理器/名称节点处于备用模式、简单的 HDFS 操作不起作用、`zkFailoverController` 处于停止状态且无法启动、Yarn/Spark/Livy 作业因 Zookeeper 错误而失败。 可能会看到如下所示的错误消息：

```
19/06/19 08:27:08 ERROR ZooKeeperStateStore: Fatal Zookeeper error. Shutting down Livy server.
19/06/19 08:27:08 INFO LivyServer: Shutting down Livy server.
```

## <a name="cause"></a>原因

当快照文件的卷较大或快照文件损坏时，ZooKeeper 服务器将无法形成仲裁，导致 ZooKeeper 相关服务运行不正常。 ZooKeeper 服务器不会从其数据目录中删除旧的快照文件，而只能由用户定期维护 ZooKeeper 的正常状况。 有关详细信息，请参阅 [ZooKeeper 的优势和限制](https://zookeeper.apache.org/doc/r3.3.5/zookeeperAdmin.html#sc_strengthsAndLimitations)。

## <a name="resolution"></a>解决方法

检查 ZooKeeper 数据目录 `/hadoop/zookeeper/version-2` 和 `/hadoop/hdinsight-zookeepe/version-2`，确定快照文件大小是否过大。 如果存在大的快照，请执行以下步骤：

1. 在 `/hadoop/zookeeper/version-2` 和 `/hadoop/hdinsight-zookeepe/version-2` 中备份快照。

1. 在 `/hadoop/zookeeper/version-2` 和 `/hadoop/hdinsight-zookeepe/version-2` 中清理快照。

1. 在 Apache Ambari UI 中重启所有 ZooKeeper 服务器。

## <a name="next-steps"></a>后续步骤

如果你的问题未在本文中列出，或者无法解决问题，请访问以下渠道以获取更多支持：


- 如果需要更多帮助，可以从 [Azure 门户](https://portal.azure.cn/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade/)提交支持请求。 从菜单栏中选择“支持”  ，或打开“帮助 + 支持”  中心。 有关更多详细信息，请参阅[如何创建 Azure 支持请求](https://docs.microsoft.com/azure/azure-supportability/how-to-create-azure-support-request)。 Microsoft Azure 订阅包含对订阅管理和计费支持的访问权限，并且通过 [Azure 支持计划](https://azure.microsoft.com/support/plans/)之一提供技术支持。
