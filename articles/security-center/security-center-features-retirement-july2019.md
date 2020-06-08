---
title: 停用的安全中心功能（2019 年 7 月）
description: 本文介绍 2019 年 7 月 31 日安全中心内停用的功能。
services: security-center
author: memildin
manager: rkarlin
ms.service: security-center
ms.topic: article
ms.date: 05/15/2020
ms.author: v-tawe
origin.date: 09/10/2019
ms.openlocfilehash: 8b62ca3cac692e0d3a5c5c03fba81a5a2f871eab
ms.sourcegitcommit: cbaa1aef101f67bd094f6ad0b4be274bbc2d2537
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/28/2020
ms.locfileid: "84126567"
---
# <a name="retirement-of-security-center-features-july-2019"></a>停用的安全中心功能（2019 年 7 月）

> [!NOTE]
> 本文档详细介绍了 2019 年 7 月 31 日从 Azure 安全中心停用的功能列表。
>
>

在截至 2019 年 7 月的六个月内，我们对 Azure 安全中心进行了多项[改进](https://updates.azure.cn/?product=security-center)。
伴随着这些改进的功能，我们于 2019 年 7 月 31 日从安全中心内删除了一些冗余功能和相关 API。

大多数停用的功能都可以由 Azure 安全中心或 Azure Log Analytics 中的其他功能所替代。

停用的安全中心功能包括：

- [事件仪表板](#menu_events)
- [搜索菜单项](#menu_search)
- [在“标识和访问”上查看经典标识和访问链接（预览）](#menu_classicidentity)




- [编辑安全策略的安全配置](#menu_securityconfigurations)
- [Log Analytics 工作区的安全和审核仪表板（最初在 OMS 门户中使用）](#menu_securityomsdashboard)

本文提供了每个停用功能的详细信息，以及实现替换功能可以采取的步骤。

## <a name="events-dashboard"></a>事件仪表板<a name="menu_events"></a>

安全中心使用 Log Analytics 代理收集计算机中各种与安全有关的配置和事件。 它将这些事件存储在工作区中。 [事件仪表板](https://docs.microsoft.com/azure/security-center/security-center-events-dashboard)让你能够查看这些数据，并为你提供 Log Analytics 的入口点。

我们停用了选择工作区时显示的事件仪表板：

![事件仪表板][2]

### <a name="events-dashboard---the-new-experience"></a>事件仪表板 - 新体验

我们鼓励你使用 Azure Log Analytics 的本机功能来查看工作区中的重要事件。

如果已在安全中心中创建了自定义的重要事件，则可以访问这些事件。 在 Log Analytics 中，转到“选择工作区” > “保存的搜索” 。 你的数据不会丢失或被修改。 Log Analytics 中的同一屏幕也提供了本机的重要事件。

![工作区 - 保存的搜索][3]

## <a name="search-menu-entry"></a>搜索菜单项<a name="menu_search"></a>

Azure 安全中心目前使用 Azure Monitor 日志搜索来检索和分析安全数据。 此屏幕用作 Log Analytics 搜索页的窗口，使用户能够在选定的工作区上运行搜索查询。 有关详细信息，请参阅 [Azure 安全中心搜索](https://docs.azure.cn/security-center/security-center-search)。 我们停用了此搜索窗口：

![“搜索”页][4]

### <a name="search-menu-entry---the-new-experience"></a>搜索菜单项 - 新体验

我们鼓励你使用 Azure Log Analytics 本机功能在工作区上执行搜索查询。 转到 Azure Log Analytics 并选择“日志”。

![Log Analytics 日志页][5]

## <a name="classic-identity--access-preview"></a>经典标识和访问（预览）<a name="menu_classicidentity"></a>

安全中心中的经典标识和访问体验当前在 Log Analytics 中显示标识和访问信息的仪表板。 若要查看此仪表板：

1. 选择“查看经典标识和访问”。

   ![标识页][6]

1. 查看“标识和访问仪表板”。

    ![标识页 - 工作区选择][7]

1. 选择工作区，在 Log Analytics 中打开“标识和访问”仪表板以查看工作区上的标识和访问信息。

   ![标识页面 - 仪表板][8]

我们停用了上述步骤中显示的三个屏幕。 你的数据在 Log Analytics 安全解决方案中仍然可用，并且没有被修改或删除。

### <a name="classic-identity--access-preview---the-new-experience"></a>经典标识和访问（预览）- 新体验

Log Analytics 仪表板在单个工作区中显示了见解。 但是，本机安全中心功能提供对所有订阅及其关联的所有工作区的可见性。 你可以访问易于使用的视图，该视图根据安全功能分数对建议进行排序，让你专注于重要事项。

可以通过在安全中心选择“标识和访问(预览)”来访问 Log Analytics 中的“标识和访问”仪表板的所有功能 。

![标识页 - 经典体验停用][9]




## <a name="edit-security-configurations-for-security-policies"></a>编辑安全策略的安全配置<a name="menu_securityconfigurations"></a>

Azure 安全中心应用 [150 多种建议的规则](https://gallery.technet.microsoft.com/Azure-Security-Center-a789e335)来监视安全配置，以便强化 OS。 这些规则适用于防火墙、审核、密码策略等。 如果发现计算机中的某项配置有漏洞，则安全中心会生成安全建议。 [编辑安全配置屏幕](https://docs.azure.cn/security-center/security-center-customize-os-security-config)使客户能够在安全中心内自定义默认的 OS 安全配置。

我们停用了此预览功能。 若要在停用日期后将安全配置重置回默认值，请使用[以下说明](https://github.com/Azure/Azure-Security-Center/tree/master/Powershell%20scripts/Reset%20security%20configurations%20customization)通过 API 和 Powershell 执行此操作。

![编辑安全配置][17]

### <a name="edit-security-configurations---the-new-experience"></a>编辑安全配置 - 新体验

我们打算让安全中心支持[来宾配置代理](https://docs.azure.cn/governance/policy/concepts/guest-configuration)。 这样的更新将实现更丰富的功能集，包括支持更多操作系统，以及为来宾配置集成 Azure 来宾策略。 启用这些更改后，你还可以大规模控制配置并自动将它们应用于新资源。

## <a name="security-and-audit-dashboard-for-log-analytics-workspaces"></a>Log Analytics 工作区的安全和审核仪表板<a name="menu_securityomsdashboard"></a>

安全和审核仪表板最初在 OMS 门户中使用。 在 Log Analytics 中，仪表板提供每个工作区重要安全事件和威胁的概述、威胁情报映射以及保存在工作区中的安全事件的标识和访问评估。 我们删除了该仪表板。 正如仪表板 UI 中提供的建议，我们建议你转到 Azure 安全中心。

![Log Analytics 安全仪表板][18]

### <a name="security-and-audit-dashboard---the-new-experience"></a>安全和审核仪表板 - 新体验

建议切换到 Azure 安全中心。 它跨多个订阅及关联的工作区，提供相同的安全概况以及更丰富的功能集。

你可以在 [GitHub 存储库](https://github.com/Azure/Azure-Security-Center/tree/master/Legacy%20Log%20Analytics%20dashboards)中获取填充安全和审核仪表板的原始 Log Analytics 查询，并将这些查询用于安全中心。

## <a name="next-steps"></a>后续步骤

- 了解有关 [Azure 安全中心](https://docs.azure.cn/security-center/)的详细信息。


<!--Image references - events-->
[1]: ./media/security-center-features-retirement-july2019/asc_events_dashboard.png
[2]: ./media/security-center-features-retirement-july2019/asc_events_dashboard_inner.png
[3]: ./media/security-center-features-retirement-july2019/workspace_saved_searches.png
<!--Image references - search-->
[4]: ./media/security-center-features-retirement-july2019/asc_search.png
[5]: ./media/security-center-features-retirement-july2019/workspace_logs.png
<!--Image references - classic identity and access-->
[6]: ./media/security-center-features-retirement-july2019/asc_identity.png
[7]: ./media/security-center-features-retirement-july2019/asc_identity_workspace_selection.png
[8]: ./media/security-center-features-retirement-july2019/loganalytics_dashboard_identity.png
[9]: ./media/security-center-features-retirement-july2019/asc_identity_nobuttonhighlight.png
<!--Image references - alerts map-->
[10]: ./media/security-center-features-retirement-july2019/asc_security_alerts_map.png
[11]: ./media/security-center-features-retirement-july2019/asc_threat_intellignece_dashboard.png
[12]: ./media/security-center-features-retirement-july2019/loganalytics_security_alerts_map.png
<!--Image references - custom alerts-->
[13]: ./media/security-center-features-retirement-july2019/asc_custom_alerts.png
<!--Image references - Investigation-->
[14]: ./media/security-center-features-retirement-july2019/asc-security-incident.png
[15]: ./media/security-center-features-retirement-july2019/loganalytics_investigation_dashboard.png
<!--Image references - Solutions-->
[16]: ./media/security-center-features-retirement-july2019/asc_security_solutions.png
<!--Image references - Edit security configurations-->
[17]: ./media/security-center-features-retirement-july2019/asc_edit_security_configurations.png
<!--Image references - Security dashboard in log analytics-->
[18]: ./media/security-center-features-retirement-july2019/loganalytics_security_dashboard.png
