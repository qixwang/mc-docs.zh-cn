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
origin.date: 06/11/2020
ms.date: 07/06/2020
ms.openlocfilehash: e3003ccdb92efca19366071741d98f69bcd19244
ms.sourcegitcommit: 3a8a7d65d0791cdb6695fe6c2222a1971a19f745
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/28/2020
ms.locfileid: "85516559"
---
# <a name="release-notes"></a>发行说明

本文提供有关**最新** Azure HDInsight 版本更新的信息。 有关较早版本的信息，请参阅 [HDInsight 发行说明存档](hdinsight-release-notes-archive.md)。

## <a name="summary"></a>摘要

Azure HDInsight 是 Azure 中最受企业客户青睐的开源分析服务之一。

## <a name="release-date-06112020"></a>发行日期：2020/06/11

此发行版适用于 HDInsight 3.6 和 4.0。 HDInsight 发行版在几天后即会在所有区域中推出。 此处的发行日期是指在第一个区域中的发行日期。 如果看不到以下更改，请耐心等待，几天后发行版会在你所在的区域推出。

## <a name="new-features"></a>新增功能
### <a name="moving-to-azure-virtual-machine-scale-sets"></a>迁移到 Azure 虚拟机规模集
HDInsight 目前使用 Azure 虚拟机来预配群集。 从此版本起，新创建的 HDInsight 群集开始使用 Azure 虚拟机规模集。 此更改将逐步推出。 预计不会有中断性变更。 详细了解 [Azure 虚拟机规模集](https://docs.microsoft.com/azure/virtual-machine-scale-sets/overview)。
 
### <a name="reboot-vms-in-hdinsight-cluster"></a>重启 HDInsight 群集中的 VM
在此版本中，我们支持重启 HDInsight 群集中的 VM 以重启无响应的节点。 目前只能通过 API 完成此操作，即将支持使用 PowerShell 和 CLI。 有关此 API 的详细信息，请参阅[此文档](https://github.com/Azure/azure-rest-api-specs/codeowners/master/specification/hdinsight/resource-manager/Microsoft.HDInsight/stable/2018-06-01-preview/virtualMachines.json)。
 
## <a name="deprecation"></a>弃用
### <a name="deprecation-of-spark-21-and-22-in-hdinsight-36-spark-cluster"></a>弃用 HDInsight 3.6 Spark 群集中的 Spark 2.1 和 2.2
从 2020 年 7 月 1 日起，客户无法使用 HDInsight 3.6 上的 Spark 2.1 和 2.2 创建新的 Spark 群集。 现有群集将在没有 Microsoft 支持的情况下按原样运行。 请考虑在 2020 年 6 月 30 日之前转移到 HDInsight 3.6 上的 Spark 2.3，以避免潜在的系统/支持中断。
 
### <a name="deprecation-of-spark-23-in-hdinsight-40-spark-cluster"></a>弃用 HDInsight 4.0 Spark 群集中的 Spark 2.3
从 2020 年 7 月 1 日起，客户无法使用 HDInsight 4.0 上的 Spark 2.3 创建新的 Spark 群集。 现有群集将在没有 Microsoft 支持的情况下按原样运行。 请考虑在 2020 年 6 月 30 日之前转移到 HDInsight 4.0 上的 Spark 2.4，避免出现潜在的系统/支持中断。
 
### <a name="deprecation-of-kafka-11-in-hdinsight-40-kafka-cluster"></a>弃用 HDInsight 4.0 Kafka 群集中的 Kafka 1.1
从 2020 年 7 月 1 日开始，客户将无法使用 HDInsight 4.0 上的 Kafka 1.1 创建新的 Kafka 群集。 现有群集将在没有 Microsoft 支持的情况下按原样运行。 请考虑在 2020 年 6 月 30 日之前转移到 HDInsight 4.0 上的 Spark 2.1，避免出现潜在的系统/支持中断。
 
## <a name="behavior-changes"></a>行为更改
### <a name="esp-spark-cluster-head-node-size-change"></a>ESP Spark 群集头节点大小更改 
允许的最小 ESP Spark 群集头节点大小已更改为 Standard_D13_V2。 如果作为头节点的 VM 具有较低的核心和内存，则可能会由于 CPU 和内存容量相对较低而导致 ESP 群集问题。 从此版本起，将使用高于 Standard_D13_V2 和 Standard_E16_V3 的 SKU 作为 ESP Spark 群集的头节点。
 
### <a name="a-minimum-4-core-vm-is-required-for-head-node"></a>提供至少有 4 个核心的 VM 作为头节点 
头节点至少需要 4 核 VM，以确保 HDInsight 群集的高可用性和可靠性。 从 2020 年 4 月 6 日开始，客户只能选择至少有 4 个核心的 VM 作为新 HDInsight 群集的头节点。 现有群集将继续按预期方式运行。 
 
### <a name="cluster-worker-node-provisioning-change"></a>群集工作器节点预配更改
当 80% 的工作器节点准备就绪时，群集将进入**可运行**阶段。 在此阶段中，客户可以执行所有数据平面操作，例如运行脚本和作业。 但客户不能执行任何控制平面操作，例如纵向扩展/缩减。 仅支持删除。
 
在进入**可运行**阶段后，群集会再等待 60 分钟，等待的对象是其余的 20% 的工作器节点。 在 60 分钟结束时，即使仍有部分工作节点不可用，群集也会进入**正在运行**阶段。 在群集进入**正在运行**阶段后，你可以正常使用它。 控制平面操作（例如纵向扩展/缩减）和数据平面操作（例如运行脚本和作业）都会被接受。 如果所请求的某些工作器节点不可用，则群集会被标记为部分成功。 你需要为已成功部署的节点付费。 
 
### <a name="create-new-service-principal-through-hdinsight"></a>通过 HDInsight 创建新的服务主体
以前，在创建群集的过程中，客户可以创建新的服务主体来访问 Azure 门户中已连接的 ADLS 第 1 代帐户。 从 2020 年 6 月 15 日起，客户无法在 HDInsight 创建工作流中创建新的服务主体，我们只支持现有的服务主体。 请参阅[使用 Azure Active Directory 创建服务主体和证书](https://docs.microsoft.com/azure/active-directory/develop/howto-create-service-principal-portal)。
 
## <a name="upcoming-changes"></a>即将推出的更改
没有需要注意的即将发生的中断性变更。
 
## <a name="bug-fixes"></a>Bug 修复
HDInsight 会持续改善群集的可靠性和性能。 
 
## <a name="component-version-change"></a>组件版本更改
### <a name="hbase-20-to-216"></a>HBase 2.0 到 2.1.6
HBase 版本已从 2.0 升级到 2.1.6。
 
### <a name="spark-240-to-244"></a>Spark 2.4.0 到 2.4.4
Spark 版本已从 2.4.0 升级到 2.4.4。
 
### <a name="kafka-210-to-211"></a>Kafka 2.1.0 到 2.1.1
Kafka 版本已从 2.1.0 升级到 2.1.1。
 
可以在[此文档](https://docs.microsoft.com/azure/hdinsight/hdinsight-component-versioning#apache-hadoop-components-available-with-different-hdinsight-versions)中查找 HDInsight 4.0 和 HDInsight 3.6 的当前组件版本

