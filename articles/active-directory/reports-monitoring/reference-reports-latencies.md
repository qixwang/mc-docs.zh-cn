---
title: Azure Active Directory 报告延迟 | Microsoft Docs
description: 了解在 Azure 门户中显示报告事件所花费的时间
services: active-directory
documentationcenter: ''
author: cawrites
manager: daveba
editor: ''
ms.assetid: 9b88958d-94a2-4f4b-a18c-616f0617a24e
ms.service: active-directory
ms.devlang: na
ms.topic: reference
ms.tgt_pltfrm: na
ms.workload: identity
ms.subservice: report-monitor
origin.date: 05/13/2019
ms.date: 10/11/2019
ms.author: v-junlch
ms.reviewer: dhanyahk
ms.collection: M365-identity-device-management
ms.openlocfilehash: f5af6a1ec0f618018c6c3c6b79a675f8b2387d13
ms.sourcegitcommit: c1ba5a62f30ac0a3acb337fb77431de6493e6096
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2020
ms.locfileid: "72292137"
---
# <a name="azure-active-directory-reporting-latencies"></a>Azure Active Directory 报告延迟

延迟是 Azure Active Directory (Azure AD) 报告数据在 [Azure 门户](https://portal.azure.cn)中显示所需的时间。 本文列出了不同类型报告的预期延迟。 

## <a name="activity-reports"></a>活动报表

有两种类型的活动报告：

- [登录](concept-sign-ins.md) - 提供有关托管应用程序的使用情况和用户登录活动的信息
- [审核日志](concept-audit-logs.md) - 提供有关用户和组、托管应用程序和目录活动的系统活动信息

下表列出了活动报表的延迟信息。 

> [!NOTE]
> **延迟 (95%)** 是指报告 95% 的日志所用的时间，**延迟 (99%)** 是指报告 99% 的日志所用的时间。 
>

| 报表 | 延迟 (95%) |延迟 (99%)|
| :-- | --- | --- |
| 审核日志 | 2 分钟  | 5 分钟  |
| 登录 | 2 分钟  | 5 分钟 |

### <a name="how-soon-can-i-see-activities-data-after-getting-a-premium-license"></a>获得高级许可证后多久可看见活动数据？

如果已经拥有免费许可证的活动数据，则可在升级时立即看到这些数据。 升级到高级许可证后，如果没有任何数据，则需要在一到两天后，数据才会显示在报告中。

## <a name="next-steps"></a>后续步骤

* [Azure AD 报告概述](overview-reports.md)

<!-- Update_Description: wording update -->
