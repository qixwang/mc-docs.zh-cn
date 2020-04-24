---
title: Apache Spark 流式处理应用程序在执行 24 天后停止处理数据，Azure HDInsight 的日志中没有已知错误
description: Apache Spark 流式处理应用程序在执行 24 天后停止，日志文件中没有错误。
ms.service: hdinsight
ms.topic: troubleshooting
author: hrasheed-msft
ms.author: v-yiso
origin.date: 07/29/2019
ms.date: 09/23/2019
ms.openlocfilehash: 1b102d123d813ea54262cc6e9a23fd3d5ab4b2db
ms.sourcegitcommit: c1ba5a62f30ac0a3acb337fb77431de6493e6096
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2020
ms.locfileid: "70921328"
---
# <a name="scenario-apache-spark-streaming-application-stops-after-executing-for-24-days-in-azure-hdinsight"></a>方案：Apache Spark 流式处理应用程序在 Azure HDInsight 中执行 24 天后停止

本文介绍在 Azure HDInsight 群集中使用 Apache Spark 组件时出现的问题的故障排除步骤和可能的解决方案。

## <a name="issue"></a>问题

Apache Spark 流式处理应用程序在执行 24 天后停止，日志文件中没有错误。

## <a name="cause"></a>原因

`livy.server.session.timeout` 值控制 Apache Livy 应等待会话完成的时间。 会话长度达到 `session.timeout` 值后，会自动终止 Livy 会话和应用程序。

## <a name="resolution"></a>解决方法

对于长时间运行的作业，请使用 Ambari UI 增加 `livy.server.session.timeout` 值。 可使用 URL `https://<yourclustername>.azurehdinsight.cn/#/main/services/LIVY/configs` 从 Ambari UI 访问 Livy 配置。

将 `<yourclustername>` 替换为门户中显示的 HDInsight 群集的名称。

## <a name="next-steps"></a>后续步骤

如果你的问题未在本文中列出，或者无法解决问题，请访问以下渠道以获取更多支持：

* 如果需要更多帮助，可以从 [Azure 门户](https://portal.azure.cn/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade/)提交支持请求。 从菜单栏中选择“支持”  ，或打开“帮助 + 支持”  中心。 有关更多详细信息，请参阅[如何创建 Azure 支持请求](https://docs.microsoft.com/azure/azure-supportability/how-to-create-azure-support-request)。 在 Microsoft Azure 订阅中可以访问订阅管理和计费支持；通过 [Azure 支持计划](https://azure.microsoft.com/support/plans/)之一提供技术支持。
