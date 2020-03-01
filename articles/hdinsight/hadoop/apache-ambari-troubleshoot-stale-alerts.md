---
title: Azure HDInsight 中的 Apache Ambari 过时警报
description: 讨论和分析 HDInsight 中 Apache Ambari 警报过时的可能原因和解决方法。
author: hrasheed-msft
ms.author: v-yiso
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: troubleshooting
origin.date: 01/22/2020
ms.date: 03/02/2020
ms.openlocfilehash: cbecb453dc7fe70410bc7ebac4d9e293892ed015
ms.sourcegitcommit: 46fd4297641622c1984011eac4cb5a8f6f94e9f5
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/22/2020
ms.locfileid: "77563614"
---
# <a name="scenario-apache-ambari-stale-alerts-in-azure-hdinsight"></a>方案：Azure HDInsight 中的 Apache Ambari 过时警报

本文介绍在与 Azure HDInsight 群集交互时出现的问题的故障排除步骤和可能的解决方案。

## <a name="issue"></a>问题

在 Apache Ambari UI 中，可能会看到下图所示的警报：

![Apache Ambari 过时警报示例](./media/apache-ambari-troubleshoot-stale-alerts/ambari-stale-alerts-example.png)

## <a name="cause"></a>原因

Ambari 代理持续执行运行状况检查来监视多个资源的运行状况。 每个警报配置为按预定义的时间间隔运行。 执行每个警报后，Ambari 代理会将状态报告回到 Ambari 服务器。 此时，如果 Ambari 服务器检测到任何警报未及时运行，则会触发“Ambari 服务器警报”。 运行状况检查不按定义的间隔执行的原因有多种：

* 当主机的资源利用率（CPU 利用率很高）极高时，Ambari 代理可能无法获得足够的系统资源，因此无法及时执行警报。

* 在负载繁重的情况下，群集正忙于执行许多作业/服务。

* 群集中的少量主机可能托管了多个组件，因此需要运行许多警报。 如果组件数量很大，警报作业可能会错过其计划的间隔

## <a name="resolution"></a>解决方法

### <a name="increase-alert-interval-time"></a>增大警报间隔时间

可以根据群集的响应时间及其负载，选择增大单个警报间隔的值。

1. 在 Apache Ambari UI 中，选择“警报”选项卡。 
1. 选择所需的警报定义名称。
1. 在定义中选择“编辑”。 
1. 根据需要修改“检查间隔”值，然后选择“保存”。  

### <a name="increase-alert-interval-time-for-ambari-server-alerts"></a>增大 Ambari 服务器警报的警报间隔时间

1. 在 Apache Ambari UI 中，选择“警报”选项卡。 
1. 在“组”下拉列表中，选择“AMBARI 默认值”。  
1. 选择警报“Ambari Server 警报”。 
1. 在定义中选择“编辑”。 
1. 根据需要修改“检查间隔”值。 
1. 根据需要修改“间隔乘数”值，然后选择“保存”。  

### <a name="disable-and-enable-the-alert"></a>禁用再启用警报

可以禁用然后再启用警报，以丢弃所有过时的警报。

1. 在 Apache Ambari UI 中，选择“警报”选项卡。 
1. 选择所需的警报定义名称。
1. 在定义中，选择位于最右侧的“已启用”。 
1. 在“确认”弹出窗口中，选择“确认禁用”。  
1. 等待几秒，以清除页面上显示的所有警报“实例”。
1. 在定义中，选择位于最右侧的“已禁用”。 
1. 在“确认”弹出窗口中，选择“确认启用”。  

### <a name="increase-alert-grace-time"></a>增大警报宽限期

在 Ambari 代理报告配置的警报错过了其计划之前，会应用一个宽限期。 即使警报错过了计划的时间，但如果在警报宽限期触发了该警报，则也不会激发过时的警报。

默认的 `alert_grace_period` 值为 5 秒。 此 `alert_grace_period` 设置在 `/etc/ambari-agent/conf/ambari-agent.ini` 中可配置。 对于按固定间隔激发过时警报的主机，请尝试将值增大至 10。 然后重启 Ambari 代理

## <a name="next-steps"></a>后续步骤

如果你的问题未在本文中列出，或者无法解决问题，请访问以下渠道以获取更多支持：


* 如果需要更多帮助，可以从 [Azure 门户](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade/)提交支持请求。 从菜单栏中选择“支持”  ，或打开“帮助 + 支持”  中心。 有关更多详细信息，请参阅[如何创建 Azure 支持请求](https://docs.microsoft.com/azure/azure-supportability/how-to-create-azure-support-request)。 Microsoft Azure 订阅包含对订阅管理和计费支持的访问权限，并且通过 [Azure 支持计划](https://azure.microsoft.com/support/plans/)之一提供技术支持。
