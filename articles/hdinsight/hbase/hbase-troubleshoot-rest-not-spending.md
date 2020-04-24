---
title: Azure HDInsight 中的 Apache HBase REST 未响应请求
description: 解决了 Azure HDInsight 中的 Apache HBase REST 未响应请求的问题。
ms.service: hdinsight
ms.topic: troubleshooting
author: hrasheed-msft
ms.author: v-yiso
origin.date: 08/01/2019
ms.date: 11/11/2019
ms.openlocfilehash: 5f82a1fbc0aba25ae9cd56b4e69d639d2ee5bc87
ms.sourcegitcommit: c1ba5a62f30ac0a3acb337fb77431de6493e6096
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2020
ms.locfileid: "73426062"
---
# <a name="scenario-apache-hbase-rest-not-responding-to-requests-in-azure-hdinsight"></a>方案：Azure HDInsight 中的 Apache HBase REST 未响应请求

本文介绍在与 Azure HDInsight 群集交互时出现的问题的故障排除步骤和可能的解决方法。

## <a name="issue"></a>问题

Azure HDInsight 中的 Apache HBase REST 服务未响应请求。

## <a name="cause"></a>原因

此处的可能原因是 Apache HBase REST 服务正在泄漏套接字，这种情况在服务长时间运行（例如运行了几个月）时尤其常见。 在客户端 SDK 中，可以看到如下所示的错误消息：

```
System.Net.WebException : Unable to connect to the remote server --->
System.Net.Sockets.SocketException : A connection attempt failed because the connected party did not properly respond after a period of time, or established connection failed because connected host has failed to respond 10.0.0.19:8090
```

## <a name="resolution"></a>解决方法

通过 SSH 连接到主机后，使用以下命令重启 HBase REST。 也可以使用脚本操作在所有工作器节点上重启此服务：

```bash
sudo service hdinsight-hbrest restart
```

此命令将停止同一主机上的 HBase 区域服务器。 可以通过 Ambari 手动启动 HBase 区域服务器，或者让 Ambari 的自动重启功能自动恢复 HBase 区域服务器。

如果此问题仍旧出现，你可以安装以下缓解脚本 - 每隔 5 分钟在每个工作器节点上运行的 CRON 作业。 此缓解脚本将 ping REST 服务，并在 REST 服务无响应时将它重启。

```bash
#!/bin/bash
nc localhost 8090 < /dev/null
if [ $? -ne 0 ]
    then
    echo "RESTServer is not responding. Restarting"
    sudo /usr/hdp/current/hbase-regionserver/bin/hbase-daemon.sh restart rest
fi
```

## <a name="next-steps"></a>后续步骤

如果你的问题未在本文中列出，或者无法解决问题，请访问以下渠道以获取更多支持：

* 如果需要更多帮助，可以从 [Azure 门户](https://portal.azure.cn/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade/)提交支持请求。 从菜单栏中选择“支持”  ，或打开“帮助 + 支持”  中心。 有关更多详细信息，请参阅[如何创建 Azure 支持请求](https://docs.microsoft.com/azure/azure-supportability/how-to-create-azure-support-request)。 在 Microsoft Azure 订阅中可以访问订阅管理和计费支持；通过 [Azure 支持计划](https://azure.microsoft.com/support/plans/)之一提供技术支持。
