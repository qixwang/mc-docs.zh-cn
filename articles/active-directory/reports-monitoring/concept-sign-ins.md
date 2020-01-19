---
title: Azure Active Directory 门户中的“登录活动”报告 | Microsoft Docs
description: Azure Active Directory 门户中的“登录活动”报告简介
services: active-directory
documentationcenter: ''
author: MarkusVi
manager: daveba
editor: ''
ms.assetid: 4b18127b-d1d0-4bdc-8f9c-6a4c991c5f75
ms.service: active-directory
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.subservice: report-monitor
ms.date: 01/08/2020
ms.author: v-junlch
ms.reviewer: dhanyahk
ms.collection: M365-identity-device-management
ms.openlocfilehash: abeaef7562479d15fb4bcf9cd8e4148120b1bcb4
ms.sourcegitcommit: 1bc154c816a5dff47ee051c431cd94826e57aa60
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/09/2020
ms.locfileid: "75777082"
---
# <a name="sign-in-activity-reports-in-the-azure-active-directory-portal"></a>Azure Active Directory 门户中的“登录活动”报告

Azure Active Directory (Azure AD) 中的报告体系结构由以下部分组成：

- **活动** 
    - **登录** - 有关托管应用程序的使用情况和用户登录活动的信息。
    - **审核日志** - [审核日志](concept-audit-logs.md) - 有关用户和组管理、托管应用程序和目录活动的系统活动信息。

本文概述了登录报告。

## <a name="prerequisites"></a>必备条件

### <a name="who-can-access-the-data"></a>谁可以访问该数据？

* 具有“安全管理员”、“安全读取者”、“全局读取者”和“报告读取者”角色的用户
* 全局管理员
* 任何用户（非管理员）都可以访问自己的登录活动 

### <a name="what-azure-ad-license-do-you-need-to-access-sign-in-activity"></a>访问登录活动需要什么 Azure AD 许可证？

* 租户必须具有与之关联的 Azure AD Premium 许可证，才能查看包含所有登录活动的报告。 请参阅 [Azure Active Directory Premium 入门](../fundamentals/active-directory-get-started-premium.md)来升级 Azure Active Directory 版本。 如果在升级之前没有数据活动，则在升级到高级版许可证后，数据需要经过几天才会显示在报表中。

## <a name="sign-ins-report"></a>登录报告

用户登录报告提供了以下问题的答案：

* 什么是用户的登录模式？
* 多少用户超过一周都有登录行为？
* 这些登录的状态怎样？

在 [Azure 门户](https://portal.azure.cn)菜单中，选择“Azure Active Directory”，或从任意页搜索并选择“Azure Active Directory”   。

![选择“Azure Active Directory”](./media/concept-sign-ins/select-azure-active-directory.png "Azure Active Directory")

在“监视”  下，选择“登录”  以打开[登录报告](https://portal.azure.cn/#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/SignIns)。

![登录活动](./media/concept-sign-ins/monitoring-sign-ins-in-azure-active-directory.png "登录活动")

某些登录记录最多可能需要两个小时才会显示在门户中。

> [!IMPORTANT]
> 登录报告仅显示“交互式”登录，即用户使用其用户名和密码进行的手动登录  。 登录报告中不会显示服务到服务身份验证等非交互式登录。 

登录日志有一个默认列表视图，用于显示：

- 登录日期
- 相关的用户
- 用户登录到的应用程序
- 登录状态
- 风险检测的状态
- 多重身份验证 (MFA) 要求的状态

![登录活动](./media/concept-sign-ins/sign-in-activity.png "登录活动")

单击工具栏中的“列”即可自定义列表视图。 

![登录活动](./media/concept-sign-ins/19.png "登录活动")

通过“列”  对话框，可以访问可选属性。 在登录报告中，对于给定的登录请求，不能将具有多个值的字段作为列。 例如，“身份验证详细信息”、“条件访问数据”和“网络位置”就是这样的。   

![登录活动](./media/concept-sign-ins/columns.png "登录活动")

选择列表视图中的某个项可获得更详细的信息。

![登录活动](./media/concept-sign-ins/basic-sign-in.png "登录活动")

## <a name="filter-sign-in-activities"></a>筛选登录活动

首先，将所报告数据的范围缩小到适当的级别。 接下来，使用充当默认筛选器的日期字段筛选登录数据。 Azure AD 提供了一系列可以设置的其他筛选器。

![登录活动](./media/concept-sign-ins/04.png "登录活动")

“用户”筛选器用于指定所关注的用户的名称或用户主体名称 (UPN)。 

“应用程序”  筛选器用于指定所关注的应用程序的名称。

“登录状态”筛选器用于选择： 

- 全部
- Success
- 失败

“日期”筛选器用于定义已返回数据的时间范围。   
可能的值包括：

- 一个月
- 7 天
- 24 小时
- 自定义时间范围

选择自定义时间范围时，可以配置开始时间和结束时间。

如果向登录视图添加其他字段，这些字段会自动添加到筛选器列表。 例如，如果向列表添加“客户端应用”字段，则还会获得另一筛选器选项，用于设置以下筛选器：   
![登录活动](./media/concept-sign-ins/12.png "登录活动")

- **浏览器**  
    此筛选器显示使用浏览器流执行登录尝试的所有事件。
- **Exchange ActiveSync（受支持）**  
    此筛选器显示从支持的平台（如 iOS、Android 和 Windows Phone）尝试 Exchange ActiveSync (EAS) 协议的所有登录尝试。
- **Exchange ActiveSync（不受支持）**  
    此筛选器显示从不受支持的平台（如 Linux 发行版）尝试 EAS 协议的所有登录尝试。
- **移动应用和桌面客户端** 此筛选器显示未使用浏览器流的所有登录尝试。 例如，使用任何协议的任何平台上的移动应用，或者 Windows 或 MacOS 上的 Office 等桌面客户端应用。
  
- **其他客户端**
    - **IMAP**  
        使用 IMAP 检索电子邮件的旧版邮件客户端。
    - **MAPI**  
        启用了 ADAL 并在使用 MAPI 的 Office 2013。
    - **旧式 Office 客户端**  
        默认配置为 Office 2013（未启用 ADAL，使用 MAPI），或 Office 2016（已禁用 ADAL）。
    - **POP**  
        使用 POP3 检索电子邮件的旧版邮件客户端。
    - **SMTP**  
        使用 SMTP 发送电子邮件的旧版邮件客户端。

## <a name="download-sign-in-activities"></a>下载登录活动

单击“下载”  选项即可创建包含最近 250,000 条记录的 CSV 或 JSON 文件。 如果想要在 Azure 门户外部使用登录活动数据，请先[下载登录数据](quickstart-download-sign-in-report.md)。  

![下载](./media/concept-sign-ins/71.png "下载")

> [!IMPORTANT]
> 可以下载的记录数受 [Azure Active Directory 报告保留策略](reference-reports-data-retention.md)的限制。  


## <a name="sign-ins-data-shortcuts"></a>登录数据快捷方式

Azure AD 和 Azure 门户都提供登录数据的其他入口点：

- 标识安全保护概述
- 用户
- 组
- 企业应用程序

### <a name="users-sign-ins-data-in-identity-security-protection"></a>标识安全保护中的用户登录数据

“标识安全保护”  概述页上的用户登录图显示了按周汇总的登录信息。默认时间为 30 天。

![登录活动](./media/concept-sign-ins/06.png "登录活动")

单击登录图中的某一天时，可以获得该天的登录活动的概览。

登录活动列表中的每一行显示以下内容：

* 登录者是谁？
* 登录的目标应用程序是哪个？
* 登录的状态是什么？
* 登录的 MFA 状态是什么？

单击某个项即可获得有关登录操作的更多详情：

- 用户 ID
- User
- 用户名
- 应用程序 ID
- 应用程序
- 客户端
- 位置
- IP 地址
- Date
- 需要 MFA
- 登录状态

> [!NOTE]
> IP 地址的发布方式是，在 IP 地址和使用该地址的计算机所在的物理位置之间没有确定的连接。 从中心池发布 IP 地址的移动运营商和 VPN 通常与实际使用客户端设备的位置距离很远，这会导致 IP 地址映射变得复杂。 目前，在 Azure AD 报告中，最好是基于跟踪、注册表数据、反向查看和其他信息将 IP 地址转换为物理位置。

在“用户”页中单击“活动”部分的“登录”即可完全了解所有用户登录活动。   

![登录活动](./media/concept-sign-ins/08.png "登录活动")

## <a name="usage-of-managed-applications"></a>托管应用程序的使用情况

通过登录数据的以应用程序为中心的视图，可以回答如下问题：

* 谁正在使用我的应用程序？
* 组织中最常用的三个应用程序是哪些？
* 我的最新应用程序的情况如何？

此数据的入口点是组织中最常用的三个应用程序。 数据包含在“企业应用程序”下“概览”部分过去 30 天的报告中   。

![登录活动](./media/concept-sign-ins/10.png "登录活动")

应用使用情况图显示指定时间内最常用的三个应用程序的按周汇总的登录信息。 默认时间为 30 天。

![登录活动](./media/concept-sign-ins/graph-chart.png "登录活动")

如果需要，可以将焦点设置在特定应用程序上。

![报告](./media/concept-sign-ins/single-app-usage-graph.png "报告")

单击应用程序使用情况图中的某一天时，可以获取登录活动的详细列表。

**登录** 选项可提供应用程序的所有登录事件的完整概览。

## <a name="office-365-activity-logs"></a>Office 365 活动日志

可以从 [Microsoft 365 管理中心](https://docs.microsoft.com/office365/admin/admin-overview/about-the-admin-center)查看 Office 365 活动日志。 要考虑到 Office 365 活动和 Azure AD 活动日志共享大量的目录资源。 只有 Microsoft 365 管理中心提供 Office 365 活动日志的完整视图。 

还可以使用 [Office 365 管理 API](https://docs.microsoft.com/office/office-365-management-api/office-365-management-apis-overview) 以编程方式访问 Office 365 活动日志。

## <a name="next-steps"></a>后续步骤

* [登录活动报告错误代码](reference-sign-ins-error-codes.md)
* [Azure AD 数据保留策略](reference-reports-data-retention.md)
* [Azure AD 报告延迟](reference-reports-latencies.md)

<!-- Update_Description: wording update -->
