---
title: 用于在 Azure HDInsight 中查询 Apache HBase 的 REST API
description: ''
author: hrasheed-msft
ms.author: v-yiso
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: troubleshooting
origin.date: 04/08/2020
ms.date: 06/22/2020
ms.openlocfilehash: 2c7d651baee64d6a5c311dc4c6fd8b4e8e645776
ms.sourcegitcommit: 3de7d92ac955272fd140ec47b3a0a7b1e287ca14
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/12/2020
ms.locfileid: "84723902"
---
# <a name="rest-api-to-query-apache-hbase-in-azure-hdinsight"></a>用于在 Azure HDInsight 中查询 Apache HBase 的 REST API

本文介绍在与 Azure HDInsight 群集交互时出现的问题的故障排除步骤和可能的解决方案。

## <a name="issue"></a>问题

使用 Apache HBase REST 接口查询非默认命名空间下的表会导致运行时错误（HTTP 状态 500）。

## <a name="cause"></a>原因

HBase REST API 仅在使用默认命名空间时才受支持。 对于使用 HBase 命名空间或使用 HDInsight 上的 REST 服务器对具有列族的列进行调用以引用特定 GET，这是一个已知问题。 这是因为 HDInsight 网关存在安全问题。 使用 API 创建具有命名空间的表，通过列族访问列时，需要指定 `:` 字符，这在 IIS 网关模块中被认为是一个安全问题。

## <a name="mitigation"></a>缓解措施

通过在与 HDInsight 群集位于同一 Azure VNet 中的 VM 上部署应用程序，绕过网关/REST 服务器。 然后，可以直接通过 RPC 与 HBase 通信（完全绕过 REST 服务器），也可以绕过 HDInsight 网关，与运行在工作器节点上的各个 REST 服务器通信。

## <a name="next-steps"></a>后续步骤

如果你的问题未在本文中列出，或者无法解决问题，请访问以下渠道以获取更多支持：


* 如果需要更多帮助，可以从 [Azure 门户](https://portal.azure.cn/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade/)提交支持请求。 
