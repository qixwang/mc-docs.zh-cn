---
title: Apache Ambari UI 显示 Azure HDInsight 中的主机和服务已关闭
description: Apache Ambari UI 显示 Azure HDInsight 中的主机和服务已关闭
ms.service: hdinsight
ms.topic: troubleshooting
author: hrasheed-msft
ms.author: v-yiso
origin.date: 08/02/2019
ms.date: 09/23/2019
ms.openlocfilehash: 796d145f6ac98e4b7c09658e044cf6c0908a2288
ms.sourcegitcommit: c1ba5a62f30ac0a3acb337fb77431de6493e6096
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2020
ms.locfileid: "70921322"
---
# <a name="scenario-apache-ambari-ui-shows-down-hosts-and-services-in-azure-hdinsight"></a>方案：Apache Ambari UI 显示 Azure HDInsight 中的主机和服务已关闭

本文介绍在与 Azure HDInsight 群集交互时出现的问题的故障排除步骤和可能的解决方案。

## <a name="issue"></a>问题

Apache Ambari UI 可以访问，但 UI 显示几乎所有服务都已关闭，所有主机都显示检测信号丢失。

## <a name="cause"></a>原因

在大多数情况下，这是 Ambari 服务器未在活动头节点上运行的问题。 检查哪个头节点是活动头节点，并确保 Ambari 服务器在正确的头节点上运行。 请勿手动启动 Ambari 服务器，让故障转移控制器服务负责在正确的头节点上启动 Ambari 服务器。 重新启动活动头节点以强制进行故障转移。

网络问题也可能导致此问题。 从每个群集节点，看看是否可以 ping 通 `headnodehost`。 在极少数情况下，任何群集节点都无法连接到 `headnodehost`：

```
$>telnet headnodehost 8440
... No route to host
```

## <a name="resolution"></a>解决方法

通常，重新启动活动头节点会缓解此问题。 如果未缓解，请与 HDInsight 支持团队联系。

## <a name="next-steps"></a>后续步骤

如果你的问题未在本文中列出，或者无法解决问题，请访问以下渠道之一获取更多支持：


* 如果需要更多帮助，可以从 [Azure 门户](https://portal.azure.cn/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade/)提交支持请求。 从菜单栏中选择“支持”  ，或打开“帮助 + 支持”  中心。 有关更多详细信息，请参阅[如何创建 Azure 支持请求](https://docs.microsoft.com/azure/azure-supportability/how-to-create-azure-support-request)。 在 Microsoft Azure 订阅中可以访问订阅管理和计费支持；通过 [Azure 支持计划](https://azure.microsoft.com/support/plans/)之一提供技术支持。
