---
title: 快速入门：通过 Azure 门户下载审核报表 | Microsoft Docs
description: 了解如何通过 Azure 门户下载审核报表
services: active-directory
documentationcenter: ''
author: priyamohanram
manager: daveba
editor: ''
ms.assetid: 4de121ea-f4aa-4c8a-aae4-700c2c5e97a2
ms.service: active-directory
ms.devlang: na
ms.topic: quickstart
ms.tgt_pltfrm: na
ms.workload: identity
ms.subservice: report-monitor
origin.date: 11/13/2018
ms.date: 03/19/2019
ms.author: v-junlch
ms.reviewer: dhanyahk
ms.collection: M365-identity-device-management
ms.openlocfilehash: 8c0132436bea1249bcb5d7d41434cdeca198be33
ms.sourcegitcommit: c1ba5a62f30ac0a3acb337fb77431de6493e6096
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2020
ms.locfileid: "63822801"
---
# <a name="quickstart-download-an-audit-report-using-the-azure-portal"></a>快速入门：通过 Azure 门户下载审核报表

在此快速入门中，你将学习如何下载过去 24 小时内租户的审核日志的 CSV 文件。 最多可从 Azure 门户下载 250,000 条记录。 记录按最近时间进行排序，因此默认情况下获取的是最近 250,000 条记录。 

## <a name="prerequisites"></a>必备条件

需要：

* 一个 Azure Active Directory 租户。 
* 一位属于该租户的安全管理员、安全读取者或全局管理员角色的用户。    此外，该租户中的任何用户均可访问其自己的审核日志。

## <a name="quickstart-download-an-audit-report"></a>快速入门：下载审核报表

1. 导航到 [Azure 门户](https://portal.azure.cn)。
2. 从左侧导航窗格中选择“Azure Active Directory”，然后使用“切换目录”按钮选择活动目录   。
3. 从仪表板中，选择“Azure Active Directory”，然后选择“审核日志”   。 
4. 在“日期范围”筛选器下拉列表中选择“过去 24 小时”，再选择“应用”以查看过去 24 小时的审核日志    。 
5. 选择“下载”  按钮，选择“CSV”  作为文件格式，并指定文件名以下载包含已筛选记录的 CSV 文件。 

![报表](./media/quickstart-download-audit-report/download-audit-logs.png)

## <a name="next-steps"></a>后续步骤

- [Azure Active Directory 报告延迟](reference-reports-latencies.md)

<!-- Update_Description: wording update -->