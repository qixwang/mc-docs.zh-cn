---
title: 高级数据安全
description: 了解用于发现敏感数据并分类、管理数据库漏洞以及检测可能对 Azure SQL 数据库、Azure SQL 托管实例或 Azure Synapse 中数据库造成威胁的异常活动的功能。
services: sql-database
ms.service: sql-database
ms.subservice: security
ms.devlang: ''
ms.custom: sqldbrb=2
ms.topic: conceptual
ms.author: v-jay
manager: digimobile
author: WenJason
ms.reviewer: vanto
origin.date: 04/23/2020
ms.date: 07/13/2020
ms.openlocfilehash: cc94697ff85ec19accaa85a9e3b1bd8a2e1bb1bf
ms.sourcegitcommit: fa26665aab1899e35ef7b93ddc3e1631c009dd04
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/10/2020
ms.locfileid: "86227483"
---
# <a name="advanced-data-security"></a>高级数据安全
[!INCLUDE[appliesto-sqldb-sqlmi-asa](../includes/appliesto-sqldb-sqlmi-asa.md)]


高级数据安全 (ADS) 是一个面向高级 SQL 安全功能的统一包。 ADS 可用于 Azure SQL 数据库、Azure SQL 托管实例和 Azure Synapse Analytics。 它包括用于发现和分类敏感数据、呈现和减少潜在数据库漏洞，以及检测可能表明数据库有威胁的异常活动的功能。 它提供用于启用和管理这些功能的一个转到位置。

## <a name="overview"></a>概述

ADS 提供一组高级 SQL 安全功能，包括数据发现和分类、SQL 漏洞评估和高级威胁防护。
- [数据发现和分类](data-discovery-and-classification-overview.md)提供了内置于 Azure SQL 数据库、Azure SQL 托管实例和 Azure Synapse 的功能，用于发现、标记和报告数据库中的敏感数据并对其进行分类。 它可用于直观查看数据库分类状态，以及跟踪对数据库内和其边界外的敏感数据的访问。
- [漏洞评估](sql-vulnerability-assessment.md)是一项易于配置的服务，可以发现、跟踪并帮助修正潜在的数据库漏洞。 它可用于直观查看安全状态，包括解决安全问题的可操作步骤，并可加强数据库的防御工事。
- [高级威胁防护](threat-detection-overview.md)检测异常活动，指出尝试访问或利用数据库的行为异常且可能有害。 它连续监视数据库中的可疑活动，并针对潜在漏洞、Azure SQL 注入攻击和异常数据库访问模式提供即时安全警报。 高级威胁防护警报提供可疑活动的详细信息，并建议如何调查和缓解威胁。

一旦启用高级数据安全，其包含的所有功能都会启用。 只需单击一次，即可为 Azure 或 SQL 托管实例中[服务器](logical-servers.md)上的所有数据库启用 ADS。 需要属于 [SQL 安全管理器](/role-based-access-control/built-in-roles#sql-security-manager)角色或者数据库或服务器管理员角色才能启用或管理 ADS 设置。

ADS 定价遵循 Azure 安全中心“标准”层级定价，其中每个受保护的服务器或托管实例视为一个节点。 新的受保护资源符合安全中心标准层免费试用版资格。 有关详细信息，请参阅 [Azure 安全中心定价页](https://azure.cn/pricing/details/security-center/)。

## <a name="getting-started-with-ads"></a>ADS 入门

以下步骤帮助你开始使用 ADS。

## <a name="1-enable-ads"></a>1.启用 ADS

通过导航到服务器或托管实例的“安全”标题下的“高级数据安全”来启用 ADS 。

> [!NOTE]
> 系统会自动创建一个存储帐户用于存储**漏洞评估**的扫描结果。 如果为同一个资源组和区域中的另一台服务器启用了 ADS，则使用现有的存储帐户。

![启用 ADS](./media/advanced-data-security/enable_ads.png)

> [!NOTE]
> ADS 的成本遵循每个节点的 Azure 安全中心“标准”层级定价，其中节点是整个服务器或托管实例。 因此，只需支付一次即可使用 ADS 保护服务器或托管实例上的所有数据库。 你可以从免费试用版开始试用 ADS。

## <a name="2-start-classifying-data-tracking-vulnerabilities-and-investigating-threat-alerts"></a>2.开始对数据分类、跟踪漏洞和调查威胁警报

单击“数据发现和分类”卡，查看建议进行分类的敏感列，并使用永久性敏感标签对数据分类。 单击“漏洞评估”卡，查看和管理漏洞扫描和报告，并跟踪安全状况。 如果收到安全警报，请单击“高级威胁防护”卡，查看警报的详细信息，并通过 Azure 安全中心安全警报页面查看 Azure 订阅中所有警报的综合报告。

## <a name="3-manage-ads-settings"></a>3.管理 ADS 设置

要查看和管理 ADS 设置，请导航到服务器或托管实例的“安全”标题下的“高级数据安全” 。 在此页面上，可以启用或禁用 ADS，并修改整个服务器或托管实例的漏洞评估和高级威胁防护设置。

![服务器设置](./media/advanced-data-security/server_settings.png)

## <a name="4-manage-ads-settings-for-a-database"></a>4.管理数据库的 ADS 设置

要重写特定数据库的 ADS 设置，请勾选“在数据库级别启用高级数据安全”复选框。 仅当有接收单个数据库的单独高级威胁防护警报或漏洞评估结果这一特殊要求时才使用此选项，以代替或补充为服务器或托管实例上的所有数据库接收的警报和结果。

选中该复选框后，可以配置此数据库的相关设置。

![数据库和高级威胁防护设置](./media/advanced-data-security/database_threat_detection_settings.png)

还可以从 ADS 数据库窗格中访问服务器或托管实例的高级数据安全设置。 单击主 ADS 窗格中的“设置”，然后单击“查看高级数据安全服务器设置”。

![数据库设置](./media/advanced-data-security/database_settings.png)

## <a name="next-steps"></a>后续步骤

- 详细了解[数据发现和分类](data-discovery-and-classification-overview.md)
- 详细了解[漏洞评估](sql-vulnerability-assessment.md)
- 详细了解[高级威胁防护](threat-detection-configure.md)
- 了解有关 [Azure 安全中心](/security-center/security-center-intro)的详细信息
