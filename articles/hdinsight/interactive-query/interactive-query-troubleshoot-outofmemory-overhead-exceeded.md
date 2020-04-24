---
title: Apache Hive 中的联接导致 Azure HDInsight 中出现 OutOfMemory 错误
description: 处理 OutOfMemory 错误“超过 GC 开销限制”错误
ms.service: hdinsight
ms.topic: troubleshooting
author: hrasheed-msft
ms.author: v-yiso
origin.date: 07/30/2019
ms.date: 11/11/2019
ms.openlocfilehash: 9fabe49cb65ccee2ce8c0a1aca7e7b757a652d76
ms.sourcegitcommit: c1ba5a62f30ac0a3acb337fb77431de6493e6096
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2020
ms.locfileid: "73425673"
---
# <a name="scenario-joins-in-apache-hive-leads-to-an-outofmemory-error-in-azure-hdinsight"></a>方案：Apache Hive 中的联接导致 Azure HDInsight 中出现 OutOfMemory 错误

本文介绍在 Azure HDInsight 群集中使用交互式查询组件时出现的问题的故障排除步骤和可能的解决方案。

## <a name="issue"></a>问题

Apache Hive 联接的默认行为是将表的全部内容加载到内存中，以便无需执行 Map/Reduce 步骤即可执行联接。 如果 Hive 表太大而无法放入内存中，则查询可能会失败。

## <a name="cause"></a>原因

在足够大的 Hive 中运行联接时，会遇到以下错误：

```
Caused by: java.lang.OutOfMemoryError: GC overhead limit exceeded error.
```

## <a name="resolution"></a>解决方法

通过设置以下 Hive 配置值，防止 Hive 在联接时将表加载到内存中（而不是执行 Map/Reduce 步骤）：

```
hive.auto.convert.join=false
```

## <a name="next-steps"></a>后续步骤

如果设置此值不能解决问题，请访问以下...

* 如果需要更多帮助，可以从 [Azure 门户](https://portal.azure.cn/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade/)提交支持请求。 从菜单栏中选择“支持”  ，或打开“帮助 + 支持”  中心。 有关更多详细信息，请参阅[如何创建 Azure 支持请求](https://docs.microsoft.com/azure/azure-supportability/how-to-create-azure-support-request)。 在 Microsoft Azure 订阅中可以访问订阅管理和计费支持；通过 [Azure 支持计划](https://azure.microsoft.com/support/plans/)之一提供技术支持。
