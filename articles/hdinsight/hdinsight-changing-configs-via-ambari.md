---
title: 使用 Ambari 优化群集配置
description: 使用 Ambari Web UI 来配置和优化 HDInsight 群集。
documentationcenter: ''
author: ashishthaps
manager: jhubbard
editor: cgronlun
ms.assetid: ''
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
origin.date: 05/04/2020
ms.date: 06/22/2020
ms.author: ashish
ms.openlocfilehash: d84772dfb3b14032cab00c800cfe9294ada883b9
ms.sourcegitcommit: 3de7d92ac955272fd140ec47b3a0a7b1e287ca14
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/12/2020
ms.locfileid: "84723613"
---
# <a name="optimize-clusters-with-apache-ambari-in-azure-hdinsight"></a>在 Azure HDInsight 中使用 Apache Ambari 优化群集

HDInsight 为大规模数据处理应用程序提供 Apache Hadoop 群集。 对这些复杂的多节点群集进行管理、监视和优化可能充满挑战。 Apache Ambari 是可用于管理和监视 HDInsight Linux 群集的 Web 界面。

有关使用 Ambari Web UI 的简介，请参阅[使用 Apache Ambari Web UI 管理 HDInsight 群集](hdinsight-hadoop-manage-ambari.md)

使用群集凭据通过 `https://CLUSTERNAME.azurehdidnsight.cn` 登录到 Ambari。 初始屏幕显示了概述仪表板。

![Ambari 仪表板](./media/hdinsight-changing-configs-via-ambari/apache-ambari-dashboard.png)

Ambari Web UI 可用于管理主机、服务、警报、配置和视图。 Ambari 不能用于创建 HDInsight 群集或升级服务。 它也无法管理堆栈和版本、停用或重新启用主机，或向群集添加服务。

## <a name="manage-your-clusters-configuration"></a>管理群集的配置

配置设置可帮助优化特定服务。 若要修改某个服务的配置设置，请从“服务”边栏（左侧）中选择该服务，然后在服务详细信息页中导航到“配置”选项卡。**** ****

![“服务”边栏](./media/hdinsight-changing-configs-via-ambari/ambari-services-sidebar.png)

### <a name="modify-namenode-java-heap-size"></a>修改 NameNode Java 堆大小

NameNode Java 堆大小取决于许多因素，例如群集上的负载。 此外，还有文件数和块数。 默认大小 1 GB 能够很好地满足大多数群集的需要，不过，某些工作负荷可能需要更多或更少的内存。

修改 NameNode Java 堆大小：

1. 从“服务”边栏中选择“HDFS”，然后导航到“配置”选项卡。**** ****

    ![HDFS 配置](./media/hdinsight-changing-configs-via-ambari/ambari-apache-hdfs-config.png)

1. 找到“NameNode Java 堆大小”设置。**** 也可以使用“筛选器”文本框键入和查找特定的设置。**** 选择设置名称旁边的**笔**图标。

    ![NameNode Java 堆大小](./media/hdinsight-changing-configs-via-ambari/ambari-java-heap-size.png)

1. 在文本框中键入新值，然后按 **Enter** 保存更改。

    ![编辑 NameNode Java 堆大小 1](./media/hdinsight-changing-configs-via-ambari/java-heap-size-edit1.png)

1. NameNode Java 堆大小已从 2 GB 更改为 1 GB。

    ![已编辑 NameNode Java 堆大小 2](./media/hdinsight-changing-configs-via-ambari/java-heap-size-edited.png)

1. 单击配置屏幕顶部的绿色“保存”按钮保存所做的更改。****

    ![保存更改](./media/hdinsight-changing-configs-via-ambari/ambari-save-changes1.png)

## <a name="next-steps"></a>后续步骤

* [使用 Apache Ambari Web UI 管理 HDInsight 群集](hdinsight-hadoop-manage-ambari.md)
* [Apache Ambari REST API](hdinsight-hadoop-manage-ambari-rest-api.md)
* [优化 Apache HBase](./optimize-hbase-ambari.md)
* [优化 Apache Hive](./optimize-hive-ambari.md)
* [优化 Apache Pig](./optimize-pig-ambari.md)
