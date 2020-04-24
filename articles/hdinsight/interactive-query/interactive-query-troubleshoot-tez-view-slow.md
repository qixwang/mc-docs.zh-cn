---
title: Apache Ambari Tez 视图在 Azure HDInsight 中加载缓慢
description: Apache Ambari Tez 视图可能加载缓慢或根本无法加载
ms.service: hdinsight
ms.topic: troubleshooting
author: hrasheed-msft
ms.author: v-yiso
origin.date: 07/30/2019
ms.date: 09/23/2019
ms.openlocfilehash: d0afb4409758a8fae8307b92284fb252a24f35e9
ms.sourcegitcommit: c1ba5a62f30ac0a3acb337fb77431de6493e6096
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2020
ms.locfileid: "70921350"
---
# <a name="scenario-apache-ambari-tez-view-loads-slowly-in-azure-hdinsight"></a>方案：Apache Ambari Tez 视图在 Azure HDInsight 中加载缓慢

本文介绍在 Azure HDInsight 群集中使用交互式查询组件时出现的问题的故障排除步骤和可能的解决方案。

## <a name="issue"></a>问题

Apache Ambari Tez 视图可能加载缓慢或根本无法加载。 加载 Ambari Tez 视图时，可能会看到头节点上的进程变得无响应。

## <a name="cause"></a>原因

当群集上运行大量 Hive 作业时，访问 Yarn ATS API 有时可能会使 2017 年 10 月之前创建的群集产生不良性能。

## <a name="resolution"></a>解决方法

此问题已在 2017 年 10 月修复。 重新创建群集后，将不会再遇到此问题。

## <a name="next-steps"></a>后续步骤

如果你的问题未在本文中列出，或者无法解决问题，请访问以下渠道之一获取更多支持：

* 如果需要更多帮助，可以从 [Azure 门户](https://portal.azure.cn/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade/)提交支持请求。 从菜单栏中选择“支持”  ，或打开“帮助 + 支持”  中心。 有关更多详细信息，请参阅[如何创建 Azure 支持请求](https://docs.microsoft.com/azure/azure-supportability/how-to-create-azure-support-request)。 在 Microsoft Azure 订阅中可以访问订阅管理和计费支持；通过 [Azure 支持计划](https://azure.microsoft.com/support/plans/)之一提供技术支持。
