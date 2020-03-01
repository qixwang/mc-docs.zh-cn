---
title: 在 Azure HDInsight 中启动服务时端口冲突
description: 与 Azure HDInsight 群集交互时出现的问题的故障排除步骤和可能的解决方法。
author: hrasheed-msft
ms.author: v-yiso
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: troubleshooting
origin.date: 01/23/2020
ms.date: 03/02/2020
ms.openlocfilehash: 287e318b2ee3dd5ff05c5d3a2a8c58cb06e8ec4d
ms.sourcegitcommit: 46fd4297641622c1984011eac4cb5a8f6f94e9f5
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/22/2020
ms.locfileid: "77563604"
---
# <a name="scenario-port-conflict-when-starting-services-in-azure-hdinsight"></a>方案：在 Azure HDInsight 中启动服务时端口冲突

本文介绍在与 Azure HDInsight 群集交互时出现的问题的故障排除步骤和可能的解决方案。

## <a name="issue"></a>问题

服务无法启动。

## <a name="cause"></a>原因

存在端口冲突。

## <a name="resolution"></a>解决方法

### <a name="method-1"></a>方法 1

使用以下命令获取/终止所有正在运行的受端口问题影响的进程。

```bash
netstat -lntp | grep <port>
ps -ef | grep <service>
kill -9 <service>
```

然后启动服务。

### <a name="method-2"></a>方法 2

重新启动节点。

## <a name="next-steps"></a>后续步骤

如果你的问题未在本文中列出，或者无法解决问题，请访问以下渠道以获取更多支持：

* 如果需要更多帮助，可以从 [Azure 门户](https://portal.azure.cn/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade/)提交支持请求。 从菜单栏中选择“支持”  ，或打开“帮助 + 支持”  中心。 有关更多详细信息，请参阅[如何创建 Azure 支持请求](https://docs.microsoft.com/azure/azure-supportability/how-to-create-azure-support-request)。 Microsoft Azure 订阅包含对订阅管理和计费支持的访问权限，并且通过 [Azure 支持计划](https://azure.microsoft.com/support/plans/)之一提供技术支持。
