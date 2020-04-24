---
title: 排查 Azure Active Directory 活动日志中缺少数据的问题 | Microsoft Docs
description: 为你提供了一种解决方法，解决在 Azure Active Directory 活动日志中缺少数据的问题。
services: active-directory
documentationcenter: ''
author: priyamohanram
manager: mtillman
editor: ''
ms.assetid: 7cbe4337-bb77-4ee0-b254-3e368be06db7
ms.service: active-directory
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: identity
ms.component: report-monitor
origin.date: 01/15/2018
ms.date: 08/28/2018
ms.author: v-junlch
ms.reviewer: dhanyahk
ms.openlocfilehash: 4c246a56d879826d673ace55d8c06b59d9924a01
ms.sourcegitcommit: c1ba5a62f30ac0a3acb337fb77431de6493e6096
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2020
ms.locfileid: "63822798"
---
# <a name="troubleshoot-missing-data-in-the-azure-active-directory-activity-logs"></a>故障排除：Azure Active Directory 活动日志中缺少数据 

## <a name="i-cant-find-audit-logs-for-recent-actions-in-the-azure-portal"></a>在 Azure 门户中找不到针对最近操作的审核日志

### <a name="symptoms"></a>症状

我在 Azure 门户中执行了一些操作，本应在`Activity logs > Audit Logs`边栏选项卡中看到这些操作的审核日志，但却找不到。

 ![报表](./media/troubleshoot-missing-audit-data/01.png)
 
### <a name="cause"></a>原因

操作不会立即显示在活动日志中。 下表枚举了活动日志的延迟数字。 

| 报表 | &nbsp; | 延迟 (P95) | 延迟 (P99) |
|--------|--------|---------------|---------------|
| 目录审核 | &nbsp; | 2 分钟 | 5 分钟 |

### <a name="resolution"></a>解决方法

等待 15 分钟到 2 小时，再看操作是否显示在日志中。

## <a name="next-steps"></a>后续步骤

- [Azure Active Directory 报告延迟](reference-reports-latencies.md)。


