---
title: 优化 Spark 作业的性能 - Azure HDInsight
description: 介绍实现 Spark 群集最佳性能的常见策略。
services: hdinsight
documentationcenter: ''
author: maxluk
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.service: hdinsight
ms.custom: hdinsightactive
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
origin.date: 05/21/2020
ms.date: 07/06/2020
ms.author: maxluk
ms.openlocfilehash: 6f550f324d4a8a2ca408566641f3d4271eb6028a
ms.sourcegitcommit: 3a8a7d65d0791cdb6695fe6c2222a1971a19f745
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/28/2020
ms.locfileid: "85516580"
---
# <a name="optimize-apache-spark-jobs-in-hdinsight"></a>在 HDInsight 中优化 Apache Spark 作业

本文简要介绍了用于优化 Azure HDInsight 上的 Apache Spark 作业的策略。

## <a name="overview"></a>概述

Apache Spark 作业的性能由多种因素而定。 这些性能因素包括：数据的存储方式、群集的配置方式，以及处理数据时采用的操作。

可能面临的最常见的难题包括由于大小不恰当的执行程序、长时间运行的操作和引发笛卡尔操作的任务而导致的内存约束。

也有许多优化可帮助你克服这些难题，例如缓存和允许数据倾斜。

在下面的每篇文章中，可找到 Spark 优化的不同方面的信息。

* [优化 Apache Spark 的数据存储](optimize-data-storage.md)
* [优化 Apache Spark 的数据处理](optimize-data-processing.md)
* [优化 Apache Spark 的内存使用率](optimize-memory-usage.md)
* [优化 Apache Spark 的 HDInsight 群集配置](optimize-cluster-configuration.md)

## <a name="next-steps"></a>后续步骤

* [调试 Azure HDInsight 中运行的 Apache Spark 作业](apache-spark-job-debugging.md)
* [管理 HDInsight 上 Apache Spark 群集的资源](apache-spark-resource-manager.md)
* [配置 Apache Spark 设置](apache-spark-settings.md)
