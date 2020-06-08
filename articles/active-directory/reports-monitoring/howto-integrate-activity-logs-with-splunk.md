---
title: 使用 Azure Monitor 集成 Splunk | Microsoft Docs
description: 了解如何使用 Azure Monitor 将 Azure Active Directory 日志与 SumoLogic 集成
services: active-directory
documentationcenter: ''
author: MarkusVi
manager: daveba
editor: ''
ms.assetid: 2c3db9a8-50fa-475a-97d8-f31082af6593
ms.service: active-directory
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.subservice: report-monitor
ms.date: 05/27/2020
ms.author: v-junlch
ms.reviewer: dhanyahk
ms.collection: M365-identity-device-management
ms.openlocfilehash: 851d9206455cbbb0c7e4bbd1ffc183a30c17e844
ms.sourcegitcommit: 0130a709d934d89db5cccb3b4997b9237b357803
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/29/2020
ms.locfileid: "84186986"
---
# <a name="how-to-integrate-azure-active-directory-logs-with-splunk-using-azure-monitor"></a>如何：使用 Azure Monitor 将 Azure Active Directory 日志与 Splunk 集成

本文介绍如何使用 Azure Monitor 将 Azure Active Directory (Azure AD) 日志与 Splunk 集成。 首先将日志路由到 Azure 事件中心，然后将事件中心与 Splunk 集成。

## <a name="prerequisites"></a>先决条件

若要使用此功能，需满足以下条件:

- 包含 Azure AD 活动日志的 Azure 事件中心。 了解如何[将活动日志流式传输到事件中心](quickstart-azure-monitor-stream-logs-to-event-hub.md)。 

-  [适用于 Splunk 的 Azure 加载项](https://splunkbase.splunk.com/app/3757/)。 

## <a name="integrate-azure-active-directory-logs"></a>集成 Azure Active Directory 日志 

1. 打开 Splunk 实例，并选择“数据摘要”。

    ![“数据摘要”按钮](./media/howto-integrate-activity-logs-with-splunk/DataSummary.png)

2. 选择“Sourcetypes”选项卡，然后选择“amal: aadal:audit” 

    ![数据摘要 Sourcetypes 选项卡](./media/howto-integrate-activity-logs-with-splunk/sourcetypeaadal.png)

    Azure AD 活动日志将如下图中所示：

    ![活动日志](./media/howto-integrate-activity-logs-with-splunk/activitylogs.png)

> [!NOTE]
> 如果无法在 Splunk 实例中安装加载项（例如，如果使用代理或在 Splunk Cloud 上运行），则可以将这些事件转发到 Splunk HTTP 事件收集器。 为此，请使用此 [Azure 函数](https://github.com/Microsoft/AzureFunctionforSplunkVS)，该函数通过事件中心中的新消息触发。 
>

## <a name="next-steps"></a>后续步骤

* [解释 Azure Monitor 中的审核日志架构](reference-azure-monitor-audit-log-schema.md)
* [解释 Azure Monitor 中的登录日志架构](reference-azure-monitor-sign-ins-log-schema.md)
