---
title: 配置高级威胁防护 - Azure SQL 数据库托管实例 | Microsoft Docs
description: 高级威胁防护会检测异常的数据库活动，这些活动指示托管实例中存在对数据库的潜在安全威胁。
services: sql-database
ms.service: sql-database
ms.subservice: security
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: WenJason
ms.author: v-jay
ms.reviewer: vanto
origin.date: 08/05/2019
ms.date: 09/09/2019
ms.openlocfilehash: 21f8aa5c3f540dcd38653244576f3ea7e88d5f28
ms.sourcegitcommit: c1ba5a62f30ac0a3acb337fb77431de6493e6096
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2020
ms.locfileid: "70372968"
---
# <a name="configure-advanced-threat-protection-in-azure-sql-database-managed-instance"></a>在 Azure SQL 数据库托管实例中配置高级威胁防护

[托管实例](sql-database-managed-instance-index.yml)的[高级威胁防护](sql-database-threat-detection-overview.md)可检测异常活动，这些活动指示对数据库的异常和可能有害的访问或利用企图。 高级威胁防护可以识别**潜在的 SQL 注入**、**来自异常位置或数据中心的访问**、**来自陌生主体或可能有害的应用程序的访问**以及**暴力破解 SQL 凭据** - 请在[高级威胁防护警报](sql-database-threat-detection-overview.md#advanced-threat-protection-alerts)中查看更多详细信息。

你可以通过[电子邮件通知](sql-database-threat-detection-overview.md#explore-anomalous-database-activities-upon-detection-of-a-suspicious-event)或 [Azure 门户](sql-database-threat-detection-overview.md#explore-advanced-threat-protection-alerts-for-your-database-in-the-azure-portal)接收有关检测到的威胁的通知

[高级威胁防护](sql-database-threat-detection-overview.md)包含在[高级数据安全](sql-database-advanced-data-security.md) (ADS) 产品/服务中，是一个针对高级 SQL 安全功能的统一软件包。 可通过中心 SQL ADS 门户访问和管理高级威胁防护。

## <a name="set-up-advanced-threat-protection-in-the-azure-portal"></a>在 Azure 门户中设置高级威胁防护

1. 在 [https://portal.azure.cn](https://portal.azure.cn) 中启动 Azure 门户。
2. 导航到要保护的托管实例的配置页。 在“设置”页中，选择“高级数据安全”   。
3. 在“高级数据安全”配置页中，执行以下操作：
   - **启用**高级数据安全。
   - 配置在检测到异常数据库活动时需要接收安全警报的**电子邮件列表**。
   - 选择保存异常的威胁审核记录的 **Azure 存储帐户**。
   - 选择要配置的**高级威胁防护类型**。 详细了解[高级威胁防护警报](sql-database-threat-detection-overview.md)。
4. 单击“保存”，保存新的或更新的高级数据安全策略  。

   ![高级威胁防护](./media/sql-database-managed-instance-threat-detection/threat-detection.png)

   > [!NOTE]
   > 屏幕截图中的价格并非始终反映当前价格，只是一个示例。

## <a name="next-steps"></a>后续步骤

- 详细了解[高级威胁防护](sql-database-threat-detection-overview.md)。
- 如需了解有关托管实例的信息，请参阅[什么是托管实例](sql-database-managed-instance.md)。
- 详细了解[单一数据库的高级威胁防护](sql-database-threat-detection.md)。
- 详细了解[托管实例审核](/sql-database/sql-database-managed-instance-auditing)。
- 详细了解 [Azure 安全中心](/security-center/)。
