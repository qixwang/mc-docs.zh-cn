---
title: JDBC/ODBC 和 Apache Thrift 框架问题 - Azure HDInsight
description: 无法在 Azure HDInsight 中使用 JDBC/ODBC 和 Apache Thrift 软件框架下载大型数据集
author: hrasheed-msft
ms.author: v-yiso
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: troubleshooting
origin.date: 05/14/2020
ms.date: 07/06/2020
ms.openlocfilehash: 012a3d8cc85e08db0ecdf762b50d49e78774e44b
ms.sourcegitcommit: 3a8a7d65d0791cdb6695fe6c2222a1971a19f745
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/28/2020
ms.locfileid: "85516442"
---
# <a name="unable-to-download-large-data-sets-using-jdbcodbc-and-apache-thrift-software-framework-in-hdinsight"></a>无法在 HDInsight 中使用 JDBC/ODBC 和 Apache Thrift 软件框架下载大型数据集

本文介绍在 Azure HDInsight 群集中使用 Apache Spark 组件遇到问题时的故障排除步骤和可能的解决方案。

## <a name="issue"></a>问题

尝试在 Azure HDInsight 中使用 JDBC/ODBC 和 Apache Thrift 软件框架下载大型数据集时，收到如下错误消息：

```
org.apache.spark.SparkException: Kryo serialization failed:
Buffer overflow. Available: 0, required: 36518. To avoid this, increase spark.kryoserializer.buffer.max value.
```

## <a name="cause"></a>原因

此异常由尝试使用超出允许的缓冲区空间的序列化进程引起。 在 Spark 2.0.0 中，通过 Apache Thrift 软件框架访问数据时，`org.apache.spark.serializer.KryoSerializer` 类用于序列化对象。 将通过网络发送或以序列化格式缓存的数据使用不同的类。

## <a name="resolution"></a>解决方法

增加 `Kryoserializer` 缓冲区值。 添加名为 `spark.kryoserializer.buffer.max` 的密钥，并在 spark2 配置的 `Custom spark2-thrift-sparkconf` 下将其设置为 `2047`。 重启所有受影响的组件。

> [!IMPORTANT]
> `spark.kryoserializer.buffer.max` 的值必须小于 2048。 不支持小数值。

## <a name="next-steps"></a>后续步骤

如果你的问题未在本文中列出，或者无法解决问题，请访问以下渠道以获取更多支持：

* 如果需要更多帮助，可以从 [Azure 门户](https://portal.azure.cn/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade/)提交支持请求。 从菜单栏中选择“支持”****，或打开“帮助 + 支持”**** 中心。 有关更多详细信息，请参阅[如何创建 Azure 支持请求](https://docs.microsoft.com/azure/azure-supportability/how-to-create-azure-support-request)。 在 Microsoft Azure 订阅中可以访问订阅管理和计费支持；通过 [Azure 支持计划](https://azure.microsoft.com/support/plans/)之一提供技术支持。
