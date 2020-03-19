---
title: 管理 Azure HDInsight 中的磁盘空间
description: 与 Azure HDInsight 群集交互时出现的问题的故障排除步骤和可能的解决方法。
author: hrasheed-msft
ms.author: v-yiso
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: troubleshooting
origin.date: 02/17/2020
ms.date: 03/23/2020
ms.openlocfilehash: 6cfaff77ed9897b56a9a5d926161b4e67b9618ae
ms.sourcegitcommit: 32997a7d7585deaeb0ab7b8f928d397b18b343fa
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/13/2020
ms.locfileid: "79296260"
---
# <a name="manage-disk-space-in-azure-hdinsight"></a>管理 Azure HDInsight 中的磁盘空间

本文介绍在与 Azure HDInsight 群集交互时出现的问题的故障排除步骤和可能的解决方案。

## <a name="hive-log-configurations"></a>Hive 日志配置

1. 在 Web 浏览器中，导航到 `https://CLUSTERNAME.azurehdinsight.cn`，其中 `CLUSTERNAME` 是群集的名称。

1. 导航到“Hive”   >   “配置” >   “高级” >   “高级 hive-log4j”。 查看以下设置：

    * `hive.root.logger=DEBUG,RFA`。 这是默认值，将[日志级别](https://logging.apache.org/log4j/2.x/log4j-api/apidocs/org/apache/logging/log4j/Level.html)修改为 `INFO` 即可输出较少的日志条目。

    * `log4jhive.log.maxfilesize=1024MB`。 这是默认值，可以根据需要进行修改。

    * `log4jhive.log.maxbackupindex=10`。 这是默认值，可以根据需要进行修改。 如果省略了该参数，则生成的日志文件会是无限的。

## <a name="yarn-log-configurations"></a>Yarn 日志配置

查看以下配置：

* Apache Ambari

    1. 在 Web 浏览器中，导航到 `https://CLUSTERNAME.azurehdinsight.cn`，其中 `CLUSTERNAME` 是群集的名称。

    1. 导航到“Hive”   >   “配置” > “高级”   >   “资源管理器”。 确保选中“启用日志聚合”  。 如果禁用该项，则当应用程序完成或终止时，名称节点会将日志保存在本地，而不会将它们聚合到远程存储中。

* 确保群集大小适合工作负荷。 工作负载可能最近发生了变化，或者群集的大小可能已重设。 [纵向扩展](../hdinsight-scaling-best-practices.md)群集，使之与更高的工作负载匹配。

* `/mnt/resource` 中可能填充了孤立的文件（资源管理器重启时就是如此）。 如有必要，请手动清理 `/mnt/resource/hadoop/yarn/log` 和 `/mnt/resource/hadoop/yarn/local`。

## <a name="next-steps"></a>后续步骤

如果你的问题未在本文中列出，或者无法解决问题，请访问以下渠道以获取更多支持：

* 如果需要更多帮助，可以从 [Azure 门户](https://portal.azure.cn/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade/)提交支持请求。 