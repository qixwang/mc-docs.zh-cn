---
title: Azure HDInsight 中的 Apache Ambari 目录警报
description: 讨论和分析 HDInsight 中 Apache Ambari 目录警报的可能原因和解决方案。
author: hrasheed-msft
ms.author: v-yiso
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: troubleshooting
origin.date: 01/22/2020
ms.date: 03/02/2020
ms.openlocfilehash: 051186f624a8e821363037ba59d668123fb5b1d7
ms.sourcegitcommit: 46fd4297641622c1984011eac4cb5a8f6f94e9f5
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/22/2020
ms.locfileid: "77563567"
---
# <a name="scenario-apache-ambari-directory-alerts-in-azure-hdinsight"></a>方案：Azure HDInsight 中的 Apache Ambari 目录警报

本文介绍在与 Azure HDInsight 群集交互时出现的问题的故障排除步骤和可能的解决方案。

## <a name="issue"></a>问题

从 Apache Ambari 收到类似于以下内容的错误：

```
1/1 local-dirs have errors: [ /mnt/resource/hadoop/yarn/local : Cannot create directory: /mnt/resource/hadoop/yarn/local ]
1/1 log-dirs have errors: [ /mnt/resource/hadoop/yarn/log : Cannot create directory: /mnt/resource/hadoop/yarn/log ]
```

## <a name="cause"></a>原因

受影响的工作器节点上缺少 Ambari 警报中提到的目录。

## <a name="resolution"></a>解决方法

在受影响的工作器节点上手动创建缺少的目录。

1. 通过 SSH 连接到相关的工作器节点。

1. 获取根用户：`sudo su`。

1. 以递归方式创建所需目录。

1. 更改这些目录的所有者和组。

    ```bash
    chown -R yarn /mnt/resource/hadoop/yarn/local
    chgrp -R hadoop /mnt/resource/hadoop/yarn/local
    chown -R yarn /mnt/resource/hadoop/yarn/log
    chgrp -R hadoop /mnt/resource/hadoop/yarn/log
    ```

1. 从 Apache Ambari UI 禁用然后启用警报。

## <a name="next-steps"></a>后续步骤

如果你的问题未在本文中列出，或者无法解决问题，请访问以下渠道以获取更多支持：


* 如果需要更多帮助，可以从 [Azure 门户](https://portal.azure.cn/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade/)提交支持请求。 从菜单栏中选择“支持”  ，或打开“帮助 + 支持”  中心。 有关更多详细信息，请参阅[如何创建 Azure 支持请求](https://docs.microsoft.com/azure/azure-supportability/how-to-create-azure-support-request)。 Microsoft Azure 订阅包含对订阅管理和计费支持的访问权限，并且通过 [Azure 支持计划](https://azure.microsoft.com/support/plans/)之一提供技术支持。
