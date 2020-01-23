---
title: Azure HDInsight 发行说明
description: Azure HDInsight 的最新发行说明。 获取 Hadoop、Spark、R Server、Hive 和更多工具的开发技巧和详细信息。
services: hdinsight
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.custom: hdinsightactive
ms.service: hdinsight
ms.topic: conceptual
origin.date: 12/17/2019
ms.date: 01/13/2020
ms.openlocfilehash: 5bb234b6896e48599e42cfbc58cf950511441854
ms.sourcegitcommit: 6fb55092f9e99cf7b27324c61f5fab7f579c37dc
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/03/2020
ms.locfileid: "75630972"
---
# <a name="release-notes"></a>发行说明

本文提供有关**最新** Azure HDInsight 版本更新的信息。 有关较早版本的信息，请参阅 [HDInsight 发行说明存档](hdinsight-release-notes-archive.md)。

## <a name="summary"></a>摘要

Azure HDInsight 是 Azure 中最受企业客户青睐的开源 Apache Hadoop 和 Apache Spark 分析服务之一。

## <a name="release-date-12172019"></a>发行日期：2019/12/17

此发行版适用于 HDInsight 3.6 和 4.0。

> [!IMPORTANT]  
> Linux 是 HDInsight 3.4 或更高版本上使用的唯一操作系统。 有关详细信息，请参阅 [HDInsight 版本控制文章](hdinsight-component-versioning.md)。


## <a name="new-features"></a>新增功能

### <a name="service-tags"></a>服务标记
服务标记通过使你轻松限制 Azure 服务的网络访问，以此简化 Azure 虚拟机和 Azure 虚拟网络的安全性。 你可在网络安全组 (NSG) 规则中使用服务标记，允许或拒绝全局或每个 Azure 区域的特定 Azure 服务流量。 Azure 会对每个标记下面的 IP 地址进行维护。 网络安全组 (NSG) 的 HDInsight 服务标记是运行状况和管理服务的 IP 地址组。 这些组有助于尽量降低创建安全规则时的复杂性。 HDInsight 客户可通过 Azure 门户、PowerShell 和 REST API 启用服务标记。 有关详细信息，请参阅 [Azure HDInsight 的网络安全组 (NSG) 服务标记](https://docs.microsoft.com/azure/hdinsight/hdinsight-service-tags)。

### <a name="custom-ambari-db"></a>自定义 Ambari DB
通过 HDInsight，你现在可将自己的 SQL DB 用于 Apache Ambari。 你可通过 Azure 门户或资源管理器模板配置此自定义 Ambari DB。  此功能可让你为处理和容量需求选择合适的 SQL DB。 你还可轻松升级以匹配业务增长需求。 有关详细信息，请参阅[使用自定义 Ambari 数据库设置 HDInsight 群集](hdinsight-custom-ambari-db.md)。

![自定义 Ambari DB](./media/hdinsight-release-notes/custom-ambari-db.png)

## <a name="deprecation"></a>弃用
此版本无弃用。 若要为即将到来的弃用做好准备，请参阅[即将推出的变更](#upcoming-changes)。

## <a name="behavior-changes"></a>行为更改
此版本无行为变更。 若要为即将到来的行为变更做好准备，请参阅[即将推出的变更](#upcoming-changes)。

## <a name="upcoming-changes"></a>即将推出的更改
即将发布的版本中将推出以下变更。 

### <a name="transport-layer-security-tls-12-enforcement"></a>传输层安全性 (TLS) 1.2 强制措施
传输层安全性 (TLS) 和安全套接字层 (SSL) 是提供计算机网络通信安全的加密协议。 有关详细信息，请参阅[传输层安全性](https://en.wikipedia.org/wiki/Transport_Layer_Security#SSL_1.0.2C_2.0_and_3.0)。 虽然 Azure HDInsight 群集接受公共 HTTPS 终结点上的 TLS 1.2 连接，但仍支持 TLS 1.1，以便实现与旧客户端的后向兼容性。

从下一个版本开始，你将可以选择加入和配置新的 HDInsight 群集，以仅接受 TLS 1.2 连接。 

今年晚些时候，预计从 2020/6/30 开始，Azure HDInsight 将为所有 HTTPS 连接强制实行 TLS 2.1 或更高版本。 我们建议你确保所有客户端都已准备好处理 TLS 1.2 或更高版本。

### <a name="moving-to-azure-virtual-machine-scale-sets"></a>迁移到 Azure 虚拟机规模集
HDInsight 目前使用 Azure 虚拟机来预配群集。 从 2020 年 2 月开始（将在稍后传达确切日期），HDInsight 将改为使用 Azure 虚拟机规模集。 详细了解 [Azure 虚拟机规模集](https://docs.microsoft.com/azure/virtual-machine-scale-sets/overview)。

### <a name="esp-spark-cluster-node-size-change"></a>ESP Spark 群集节点大小变更 
在即将推出的版本中：
- 允许的最小 ESP Spark 群集节点大小将变更为 Standard_D13_V2。 
- 将不再推荐使用 A 系列 VM 创建新 ESP 群集，因为 A 系列 VM 的 CPU 和内存容量相对较低，可能导致 ESP 群集问题。

### <a name="hbase-20-to-21"></a>HBase 2.0 到 2.1
在即将推出的 HDInsight 4.0 版本中，HBase 版本将从 2.0 升级到 2.1。

## <a name="bug-fixes"></a>Bug 修复
HDInsight 会持续改善群集的可靠性和性能。 

## <a name="component-version-change"></a>组件版本更改
我们已将 HDInsight 3.6 支持延长到 2020 年 12 月 31 日。 如需了解更多详情，请参阅[支持的 HDInsight 版本](hdinsight-component-versioning.md#supported-hdinsight-versions)。

HDInsight 4.0 无组件版本变更。

HDInsight 3.6 上的 Apache Zeppelin：0.7.0-->0.7.3. 

可以在[此文档](https://docs.microsoft.com/azure/hdinsight/hdinsight-component-versioning#apache-hadoop-components-available-with-different-hdinsight-versions)中获取最新的组件版本。

## <a name="new-regions"></a>新区域

### <a name="uae-north"></a>阿联酋北部
阿拉伯联合酋长国北部的管理 IP 为 `65.52.252.96` 和 `65.52.252.97`。
