---
title: 概述 - Azure Database for MySQL
description: 了解 Azure Database for MySQL 服务，这是 Microsoft 云中基于 MySQL 社区版的关系数据库服务。
author: WenJason
ms.service: mysql
ms.author: v-jay
ms.custom: mvc
ms.topic: overview
origin.date: 3/18/2020
ms.date: 08/17/2020
ms.openlocfilehash: 73d7423a9a28efee0254558df66d3591c7d65860
ms.sourcegitcommit: 3cf647177c22b24f76236c57cae19482ead6a283
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/10/2020
ms.locfileid: "88029681"
---
# <a name="what-is-azure-database-for-mysql"></a>什么是用于 MySQL 的 Azure 数据库？

> [!NOTE] 
> 将要查看的是 Azure Database for MySQL 的新服务。 若要查看经典 MySQL Database for Azure 的文档，请访问[此页](https://docs.azure.cn/zh-cn/mysql-database-on-azure/)。

Azure Database for MySQL 是 Azure 中基于 [MySQL 社区版](https://www.mysql.com/products/community/)（可在 GPLv2 许可证下使用）数据库引擎版本 5.6、5.7 和 8.0 的一种关系数据库服务。 用于 MySQL 的 Azure 数据库提供以下功能：

- 没有额外费用的内置高可用性。
- 使用非独占预付费定价，实现可预测性能。
- 根据需要在数秒内缩放。
- 保护静态和动态敏感数据的安全。
- 长达 35 天的自动备份和时间点还原。
- 企业级安全性和符合性。

这些功能几乎都不需要进行任何管理，并且都是在不另外收费的情况下提供的。 借助这些功能，用户可将注意力集中在如何快速进行应用开发、加快推向市场，而不需要投入宝贵的时间和资源来管理虚拟机与基础结构。 此外，可以继续使用选择的开源工具和平台来开发应用程序，以提供业务所需的速度和效率，这些都不需要学习新技能。

![用于 MySQL 的 Azure 数据库概念图](media/overview/1-azure-db-for-mysql-conceptual-diagram.png)

本文介绍了用于 MySQL 的 Azure 数据库在性能、伸缩性和易管理性方面的核心概念与功能，并提供了可进一步了解详细信息的链接。 请参阅这些快速入门，以便尽快入门：

- [使用 Azure 门户创建用于 MySQL 服务器的 Azure 数据库](quickstart-create-mysql-server-database-using-azure-portal.md)
- [使用 Azure CLI 创建用于 MySQL 服务器的 Azure 数据库](quickstart-create-mysql-server-database-using-azure-cli.md)

如需一组 Azure CLI 示例，请参阅：

- [用于 MySQL 的 Azure 数据库的 Azure CLI 示例](sample-scripts-azure-cli.md)

## <a name="adjust-performance-and-scale-within-seconds"></a>几秒钟内调整性能和规模
Azure Database for MySQL 服务提供多个服务层级：“基本”、“常规用途”和“内存优化”。 每个层提供不同的性能和功能，以支持从轻型到重型的数据库工作负荷。 可以在一个月内花费很少的费用基于小型数据库构建第一个应用，然后根据解决方案的需求调整缩放。 动态可伸缩性使得数据库能够以透明方式对不断变化的资源需求做出响应。 只需在需要资源时为所需的资源付费。 有关详细信息，请参阅 [定价层](concepts-service-tiers.md)。

## <a name="monitoring-and-alerting"></a>监视和警报
如何决定何时调高和调低？ 将内置的性能监视和警报功能与基于 vCore 的性能等级组合使用。 使用这些工具，可以根据当前的或预计的性能需求，快速评估调高或调低 vCore 数产生的影响。 有关详细信息，请参阅[警报](howto-alert-on-metric.md)。

## <a name="keep-your-app-and-business-running"></a>使应用和业务持续运转
Azure 行业领先的 99.99% 可用性服务级别协议 (SLA)（由 Microsoft 管理的数据中心的全球网络提供支持），有助于保持应用全天候运行。 对于每个用于 MySQL 服务器的 Azure 数据库，可以利用内置的安全性、容错和数据保护功能。使用其他产品时可能需要另外购买或设计、构建并管理这些功能。 使用用于 MySQL 的 Azure 数据库，可以使用时间点还原将服务器还原到以前的状态，最多可还原到 35 天前。

## <a name="secure-your-data"></a>保护数据
Azure 数据库沿袭了 Azure Database for MySQL 的若干数据安全性功能，包括限制访问、保护静止和移动数据，以及帮助监视活动等。 有关 Azure 平台安全性的信息，请访问 [Azure 信任中心](https://www.trustcenter.cn/)。 有关 Azure Database for MySQL 安全功能的详细信息，请参阅[安全概述](concepts-security.md)。

## <a name="contacts"></a>联系人
若要联系 Azure 支持，请[从 Azure 门户提交票证](https://portal.azure.cn/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade)。

## <a name="next-steps"></a>后续步骤
现在，已阅读了 Azure Database for MySQL 的简介并回答了“什么是 Azure Database for MySQL？”这个问题， 因此可以继续完成以下内容：

- 有关成本比较和计算器，请参阅定价页。 [定价](https://azure.cn/pricing/details/mysql/)
- 从创建第一个服务器开始。 [使用 Azure 门户创建用于 MySQL 服务器的 Azure 数据库](quickstart-create-mysql-server-database-using-azure-portal.md)
- 使用首选语言生成首个应用：
  - [Python](./connect-python.md)
  - [Node.JS](./connect-nodejs.md)
  - [Java](./connect-java.md)
  - [Ruby](./connect-ruby.md)
  - [PHP](./connect-php.md)
  - [.NET (C#)](./connect-csharp.md)
  - [Go](./connect-go.md)
