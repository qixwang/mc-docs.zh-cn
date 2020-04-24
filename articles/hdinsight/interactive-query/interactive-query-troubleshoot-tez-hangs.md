---
title: Apache Tez 应用程序在 Azure HDInsight 中挂起
description: Apache Tez 应用程序在 Azure HDInsight 中挂起
ms.service: hdinsight
ms.topic: troubleshooting
author: hrasheed-msft
ms.author: v-yiso
origin.date: 08/09/2019
ms.date: 09/23/2019
ms.openlocfilehash: 24fbebb5de3f8c19a4ff470a734582b84fe85126
ms.sourcegitcommit: c1ba5a62f30ac0a3acb337fb77431de6493e6096
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2020
ms.locfileid: "70921355"
---
# <a name="scenario-apache-tez-application-hangs-in-azure-hdinsight"></a>方案：Apache Tez 应用程序在 Azure HDInsight 中挂起

本文介绍在与 Azure HDInsight 群集交互时出现的问题的故障排除步骤和可能的解决方案。

## <a name="issue"></a>问题

提交 Apache Hive 作业后，在 Tez 视图中作业状态为“正在运行”，但似乎没有任何进展。

## <a name="cause"></a>原因

提交的作业太多；长 Yarn 队列。

## <a name="resolution"></a>解决方法

纵向扩展群集，或者只是等到 Yarn 队列排空。

默认情况下，`yarn.scheduler.capacity.maximum-applications` 控制正在运行或挂起的应用程序的最大数量，默认为 `10000`。

## <a name="next-steps"></a>后续步骤

如果你的问题未在本文中列出，或者无法解决问题，请访问以下渠道以获取更多支持：

* 如果需要更多帮助，可以从 [Azure 门户](https://portal.azure.cn/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade/)提交支持请求。 从菜单栏中选择“支持”  ，或打开“帮助 + 支持”  中心。 有关更多详细信息，请参阅[如何创建 Azure 支持请求](https://docs.microsoft.com/azure/azure-supportability/how-to-create-azure-support-request)。 Microsoft Azure 订阅包含对订阅管理和计费支持的访问权限，并且通过 [Azure 支持计划](https://azure.microsoft.com/support/plans/)之一提供技术支持。
