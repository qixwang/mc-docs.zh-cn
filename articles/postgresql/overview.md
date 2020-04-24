---
title: Azure Database for PostgreSQL 关系数据库服务概述
description: 概述了用于 PostgreSQL 关系数据库服务的 Azure 数据库。
author: WenJason
ms.author: v-jay
ms.custom: mvc
ms.service: postgresql
ms.topic: overview
origin.date: 11/25/2019
ms.date: 12/09/2019
ms.openlocfilehash: 2b74aa81acfe1dcaa2060d5ee65ea6385e37142a
ms.sourcegitcommit: c1ba5a62f30ac0a3acb337fb77431de6493e6096
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2020
ms.locfileid: "74838567"
---
# <a name="what-is-azure-database-for-postgresql"></a>什么是用于 PostgreSQL 的 Azure 数据库？
Azure Database for PostgreSQL 是 Azure 云中为开发人员构建的关系型数据库服务。 它基于开源 [PostgreSQL](https://www.postgresql.org/) 数据库引擎的社区版本。

## <a name="azure-database-for-postgresql---single-server"></a>Azure Database for PostgreSQL - 单一服务器
单一服务器部署选项提供：

- 没有额外费用的内置[高可用性](concepts-high-availability.md) (99.99% SLA)
- 使用非独占预付费定价，实现可预测性能
- [根据需要在数秒内进行垂直缩放](concepts-pricing-tiers.md)
- 用于评估服务器的[监视和警报](concepts-monitoring.md)
- 企业级安全性和符合性
- [保护](concepts-security.md)静态和动态敏感数据的安全
- 长达 35 天的[自动备份和时间点还原](concepts-business-continuity.md)


所有这些功能几乎都不需要进行任何管理，并且都是在不另外收费的情况下提供的。 借助这些功能，用户可将注意力集中在如何快速进行应用程序开发、加快推向市场，而不需要花费宝贵的时间和资源来管理虚拟机与基础结构。 可以使用所选的开源工具和平台继续开发应用程序，不需学习新技能。

“单一服务器”部署选项提供三个定价层：“基本”、“常规用途”和“内存优化”。 每个层提供不同的资源功能以支持数据库工作负荷。 可以在一个月内花费很少的费用基于小型数据库构建第一个应用，然后根据解决方案的需求调整缩放。 动态可伸缩性使得数据库能够以透明方式对不断变化的资源需求做出响应。 只需在需要资源时为所需的资源付费。 有关详细信息，请参阅 [定价层](concepts-pricing-tiers.md)。

## <a name="contacts"></a>联系人
若要联系 Azure 支持部门或修复帐户问题，请[通过 Azure 门户提交票证](https://portal.azure.cn/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade)。

## <a name="next-steps"></a>后续步骤
- 有关成本比较和计算器，请参阅[定价页](https://azure.cn/pricing/details/postgresql/)。
- 请从创建第一个 Azure Database for PostgreSQL [单一服务器](./quickstart-create-server-database-portal.md)开始
- 使用 Python、PHP、Ruby、C\#、Java、Node.js 构建第一个应用：[连接库](./concepts-connection-libraries.md)
