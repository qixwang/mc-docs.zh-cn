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
origin.date: 07/31/2020
ms.date: 08/17/2020
ms.openlocfilehash: 6a1498ebc457169a634e511949a9892e703b739e
ms.sourcegitcommit: ac70b12de243a9949bf86b81b2576e595e55b2a6
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/07/2020
ms.locfileid: "87917171"
---
# <a name="release-notes"></a>发行说明

本文提供有关**最新** Azure HDInsight 版本更新的信息。 有关较早版本的信息，请参阅 [HDInsight 发行说明存档](hdinsight-release-notes-archive.md)。

## <a name="summary"></a>摘要

Azure HDInsight 是 Azure 中最受企业客户青睐的开源分析服务之一。

## <a name="release-date-07302020"></a>发行日期：2020/07/30

此发行版适用于 HDInsight 3.6 和 4.0。 HDInsight 发行版在几天后即会在所有区域中推出。 此处的发行日期是指在第一个区域中的发行日期。 如果看不到以下更改，请耐心等待，几天后发行版会在你所在的区域推出。

## <a name="new-features"></a>新增功能
### <a name="support-for-sparkcruise"></a>对 SparkCruise 的支持
SparkCruise 是 Spark 的自动计算重用系统。 它基于过去的查询工作负载选择要具体化的常见子表达式。 SparkCruise 将这些子表达式具体化为查询处理的一部分，系统会在后台自动应用计算重用。 无需对 Spark 代码进行任何修改，就可以通过 SparkCruise 受益。
 
### <a name="support-hive-view-for-hdinsight-40"></a>提供对 HDInsight 4.0 的 Hive 视图支持
Apache Ambari Hive 视图的作用是帮助你通过 Web 浏览器创作、优化和执行 Hive 查询。 从此版本开始，提供对 HDInsight 4.0 群集的本机 Hive 视图支持。 它不适用于现有群集。 需要删除并重新创建群集才能获取内置的 Hive 视图。
 
### <a name="support-tez-view-for-hdinsight-40"></a>支持 HDInsight 4.0 的 Tez 视图
Apache Tez 视图用于跟踪和调试 Hive Tez 作业的执行情况。 从此版本开始，HDInsight 4.0 支持本机 Tez 视图。 它不适用于现有群集。 需要删除并重新创建群集，才能获得内置的 Tez 视图。

## <a name="deprecation"></a>弃用
### <a name="deprecation-of-spark-21-and-22-in-hdinsight-36-spark-cluster"></a>弃用 HDInsight 3.6 Spark 群集中的 Spark 2.1 和 2.2
从 2020 年 7 月 1 日起，客户无法使用 HDInsight 3.6 上的 Spark 2.1 和 2.2 创建新的 Spark 群集。 现有群集将在没有 Microsoft 支持的情况下按原样运行。 请考虑在 2020 年 6 月 30 日之前转移到 HDInsight 3.6 上的 Spark 2.3，以避免潜在的系统/支持中断。
 
### <a name="deprecation-of-spark-23-in-hdinsight-40-spark-cluster"></a>弃用 HDInsight 4.0 Spark 群集中的 Spark 2.3
从 2020 年 7 月 1 日起，客户无法使用 HDInsight 4.0 上的 Spark 2.3 创建新的 Spark 群集。 现有群集将在没有 Microsoft 支持的情况下按原样运行。 请考虑在 2020 年 6 月 30 日之前转移到 HDInsight 4.0 上的 Spark 2.4，避免出现潜在的系统/支持中断。
 
### <a name="deprecation-of-kafka-11-in-hdinsight-40-kafka-cluster"></a>弃用 HDInsight 4.0 Kafka 群集中的 Kafka 1.1
从 2020 年 7 月 1 日开始，客户将无法使用 HDInsight 4.0 上的 Kafka 1.1 创建新的 Kafka 群集。 现有群集将在没有 Microsoft 支持的情况下按原样运行。 请考虑在 2020 年 6 月 30 日之前转移到 HDInsight 4.0 上的 Spark 2.1，避免出现潜在的系统/支持中断。
 
## <a name="behavior-changes"></a>行为更改
### <a name="ambari-stack-version-change"></a>Ambari 堆栈版本更改
从此发布版开始，Ambari 版本从 2.x.x.x 更改为 4.1。 可以通过访问“Ambari UI”>“关于”获取 Ambari 版本。

## <a name="upcoming-changes"></a>即将推出的更改
没有需要注意的即将发生的中断性变更。
 
## <a name="bug-fixes"></a>Bug 修复
HDInsight 会持续改善群集的可靠性和性能。 

已为 Hive 向后移植以下 JIRA：
* [HIVE-23619](https://issues.apache.org/jira/browse/HIVE-23619)
* [HIVE-21223](https://issues.apache.org/jira/browse/HIVE-21223)
* [HIVE-22599](https://issues.apache.org/jira/browse/HIVE-22599)
* [HIVE-22121](https://issues.apache.org/jira/browse/HIVE-22121)
* [HIVE-22136](https://issues.apache.org/jira/browse/HIVE-22136)
* [HIVE-18786](https://issues.apache.org/jira/browse/HIVE-18786)

## <a name="component-version-change"></a>组件版本更改
此发行版未发生组件版本更改。 可以在[此文档](https://docs.microsoft.com/azure/hdinsight/hdinsight-component-versioning#apache-hadoop-components-available-with-different-hdinsight-versions)中查找 HDInsight 4.0 和 HDInsight 3.6 的当前组件版本。