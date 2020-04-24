---
title: 将本地 Apache Hadoop 群集迁移到 Azure HDInsight - 安全性和 DevOps 最佳做法
description: 了解有关将本地 Hadoop 群集迁移到 Azure HDInsight 的安全性和 DevOps 最佳做法。
services: hdinsight
author: hrasheed-msft
ms.reviewer: ashishth
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
origin.date: 12/19/2019
ms.date: 03/02/2020
ms.author: v-yiso
ms.openlocfilehash: 97929e5393446e9cd210527f0cde3653c535ddb9
ms.sourcegitcommit: c1ba5a62f30ac0a3acb337fb77431de6493e6096
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2020
ms.locfileid: "77563508"
---
# <a name="migrate-on-premises-apache-hadoop-clusters-to-azure-hdinsight---security-and-devops-best-practices"></a>将本地 Apache Hadoop 群集迁移到 Azure HDInsight - 安全性和 DevOps 最佳做法

本文提供有关 Azure HDInsight 系统安全性和 DevOps 的建议。 本文是帮助用户将本地 Apache Hadoop 系统迁移到 Azure HDInsight 的最佳做法系列教程中的其中一篇。



## <a name="implement-end-to-end-enterprise-security"></a>实现端到端企业安全性

使用以下控件可以实现端到端的企业安全性：

- **专用和受保护的数据管道（外围级别安全性）**
    - 可以通过 Azure 虚拟网络、网络安全组和网关服务实现外围级别安全性

- **数据访问的身份验证和授权**
    - 使用 Azure Active Directory 域服务创建已加入域的 HDInsight 群集。 （企业安全性套餐）
    - 使用 Ambari 为 AD 用户提供对群集资源的基于角色的访问
    - 使用 Apache Ranger 在表/列/行级别为 Hive 设置访问控制策略。
    - 此外，只有管理员能够通过 SSH 访问群集。

- **审核**
    - 查看和报告对 HDInsight 群集资源与数据的所有访问。
    - 查看并报告对访问控制策略的所有更改

- **加密**
    - 使用 Microsoft 托管的密钥或客户管理的密钥进行透明的服务器端加密。
    - 使用客户端加密的传输加密、https 和 TLS

有关详细信息，请参阅以下文章：

- [Azure 虚拟网络概述](../../virtual-network/virtual-networks-overview.md)
- [Azure 网络安全组概述](../../virtual-network/security-overview.md)
- [Azure 虚拟网络对等互连](../../virtual-network/virtual-network-peering-overview.md)
- [Azure 存储安全指南](../../storage/blobs/security-recommendations.md)
- [Azure 存储服务静态加密](../../storage/common/storage-service-encryption.md)

## <a name="use-monitoring--alerting"></a>使用监视和警报

有关详细信息，请参阅文章：

[Azure 监视器概述](../../azure-monitor/overview.md)

## <a name="upgrade-clusters"></a>升级群集

定期升级到最新的 HDInsight 版本，以利用最新功能。 可以使用以下步骤将群集升级到最新版本：

1. 使用最新的 HDInsight 版本创建新的 TEST HDInsight 群集。
1. 测试新群集以确保作业和工作负载按预期工作。
1. 根据需要修改作业、应用程序或工作负载。
1. 备份所有存储在本地群集节点上的暂时性数据。
1. 删除现有的群集。
1. 使用与前一个群集相同的默认数据和元存储，在同一虚拟网络子网中创建最新 HDInsight 版本的群集。
1. 导入任何已备份的临时数据。
1. 使用新群集启动作业/继续处理。

有关详细信息，请参阅文章：[将 HDInsight 群集升级到新版本](../hdinsight-upgrade-cluster.md)

## <a name="patch-cluster-operating-systems"></a>修补群集操作系统

作为托管的 Hadoop 服务，HDInsight 负责修补 HDInsight 群集使用的 VM 的 OS。

有关详细信息，请参阅文章：[针对 HDInsight 的 OS 修补](../hdinsight-os-patching.md)

## <a name="post-migration"></a>迁移后

1. **修复应用程序** - 迭代地对作业、进程和脚本进行必要的更改。
2. **执行测试** - 迭代地运行功能和性能测试。
3. **优化** - 根据上述测试结果解决任何性能问题，然后重新测试以确认性能改进。

## <a name="next-steps"></a>后续步骤

- 了解有关 [HDInsight 4.0](/hdinsight/hadoop/apache-hadoop-introduction) 的详细信息