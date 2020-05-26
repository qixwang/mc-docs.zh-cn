---
title: Azure 安全中心中的 IaaS 高级数据安全 | Azure
description: " 了解如何在 Azure 安全中心实现 IaaS 的高级数据安全。 "
services: security-center
documentationcenter: na
author: memildin
manager: rkarlin
ms.assetid: ba46c460-6ba7-48b2-a6a7-ec802dd4eec2
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/09/2020
ms.author: v-tawe
origin.date: 11/11/2019
ms.openlocfilehash: 34d46c0b907bb5d8ca3b1d43051c092be2be9849
ms.sourcegitcommit: 134afb420381acd8d6ae56b0eea367e376bae3ef
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/15/2020
ms.locfileid: "83423008"
---
# <a name="advanced-data-security-for-sql-servers-on-azure-virtual-machines-preview"></a>Azure 虚拟机上的 SQL Server 高级数据安全（预览版）
Azure 虚拟机上的 SQL Server 高级数据安全是高级 SQL 安全功能的统一包。 此预览功能包括用于标识和减少潜在数据库漏洞，以及检测可能表明数据库有威胁的异常活动的功能。 

此安全产品/服务适用于 Azure VM SQL Server，基于 [Azure SQL 数据库高级数据安全包](https://docs.azure.cn/sql-database/sql-database-advanced-data-security)中所使用的相同基础技术。


## <a name="overview"></a>概述

高级数据安全提供一组高级 SQL 安全功能，包括漏洞评估和高级威胁防护。

* [漏洞评估](https://docs.azure.cn/sql-database/sql-vulnerability-assessment)是一项易于配置的服务，可以发现、跟踪并帮助修正潜在的数据库漏洞。 它可直观查看安全状态，包括解决安全问题的步骤，并可加强数据库的防御工事。
* [高级威胁防护](https://docs.azure.cn/sql-database/sql-database-threat-detection-overview)检测异常活动，指出尝试访问或利用 SQL Server 的行为异常且可能有害。 它不断监视数据库的可疑活动，并针对异常数据库访问模式提供操作导向的安全警报。 这些警报提供可疑活动的详细信息，并建议如何调查和缓解威胁。

## <a name="get-started-with-advanced-data-security-for-sql-on-azure-vms"></a>开始使用 Azure VM 上的 SQL 高级数据安全

以下步骤可帮助你开始使用 Azure VM 上的 SQL 高级数据安全公共预览版。

### <a name="set-up-advanced-data-security-for-sql-on-azure-vms"></a>设置 Azure VM 上的 SQL 高级数据安全

在订阅/工作区级别，启用“虚拟机上的 SQL Server 高级数据安全”：
 
1. 在安全中心边栏中，打开“定价和设置”页。

1. 选择要启用 Azure VM 上的 SQL 高级数据安全的订阅或工作区。

1. 将“VM 上的 SQL Server(预览版)”选项切换为“已启用”。 

    （单击屏幕截图以展开）

    [![Windows 管理中心中显示的安全中心建议和警报](./media/security-center-advanced-iaas-data/sql-servers-on-vms-in-pricing-small.png)](./media/security-center-advanced-iaas-data/sql-servers-on-vms-in-pricing-large.png#lightbox)

    连接到所选工作区或所选订阅的默认工作区的所有 SQL Server 上将启用 SQL Server 的高级数据安全。

    >[!NOTE]
    > 首次重启 SQL Server 后，解决方案将处于完全激活状态。 

若要新建工作区，请按照[创建 Log Analytics 工作区](https://docs.azure.cn/azure-monitor/learn/quick-create-workspace)中的说明进行操作。

若要将 SQL Server 的主机连接到工作区，请按照[将 Windows 计算机连接到 Azure Monitor](https://docs.azure.cn/azure-monitor/platform/agent-windows) 中的说明进行操作。


## <a name="set-up-email-notification-for-security-alerts"></a>设置安全警报的电子邮件通知 

可设置在生成安全中心警报时接收电子邮件通知的收件人列表。 电子邮件包含指向 Azure 安全中心警报的直接链接，以及所有的相关详细信息。 

1. 转到“安全中心” > “定价和设置”，然后单击相关订阅 

    ![订阅设置](./media/security-center-advanced-iaas-data/subscription-settings.png)

1. 在“设置”菜单中，单击“电子邮件通知”。 
1. 在“电子邮件地址”文本框中，输入电子邮件地址以接收通知。 可以输入多个电子邮件地址，方法是使用逗号 (,) 分隔电子邮件地址。  例如：admin1@mycompany.com, admin2@mycompany.com, admin3@mycompany.com

    ![电子邮件设置](./media/security-center-advanced-iaas-data/email-settings.png)

1. 在“电子邮件通知”设置中，设置以下选项：
  
    * **发送高严重性警报的电子邮件通知**：只发送高严重性警报的电子邮件，而不是所有警报的电子邮件。
    * **同时向订阅所有者发送电子邮件通知**：同时将通知发送给订阅所有者。

1. 在“电子邮件通知”屏幕顶部，单击“保存” 。

  > [!NOTE]
  > 关闭窗口之前，务必单击“保存”，否则不会保存新的“电子邮件通知”设置 。

## <a name="explore-vulnerability-assessment-reports"></a>浏览漏洞评估报告

漏洞评估仪表板概述了所有数据库的评估结果。 你可以根据 SQL Server 版本查看数据库的分布，也可以根据风险分布查看失败的与通过的数据库的摘要以及失败检查的总体摘要。

可以直接在安全中心查看漏洞评估结果。

1. 在安全中心边栏的资源安全引擎下，选择“数据和存储”。

1. 选择“应在 VM 中修正 SQL 数据库上的漏洞(预览版)”建议。 有关详细信息，请参阅[安全中心建议](security-center-recommendations.md)。 

    [![**应在 VM 中修正 SQL 数据库上的漏洞(预览版)**建议](media/security-center-advanced-iaas-data/data-and-storage-sqldb-vulns-on-vm.png)](media/security-center-advanced-iaas-data/data-and-storage-sqldb-vulns-on-vm.png#lightbox)

    此时显示此建议的详细视图。

    [![**应在 VM 中修正 SQL 数据库上的漏洞(预览版)**建议的详细视图](media/security-center-advanced-iaas-data/all-servers-view.png)](media/security-center-advanced-iaas-data/all-servers-view.png#lightbox)

1. 深入了解更多详细信息：

    * 有关已扫描资源（数据库）的概述和已测试的安全检查的列表，请单击相关服务器。
    [![按 SQL Server 分组的漏洞](media/security-center-advanced-iaas-data/single-server-view.png)](media/security-center-advanced-iaas-data/single-server-view.png#lightbox)

    * 有关按特定 SQL 数据库分组的漏洞的概述，请单击相关数据库。
    [![按 SQL Server 分组的漏洞](media/security-center-advanced-iaas-data/single-database-view.png)](media/security-center-advanced-iaas-data/single-database-view.png#lightbox)

    在每个视图中，安全检查按照严重性排序。 单击特定安全检查即可查看详细信息窗格，其中包含说明、修正方法以及影响或基准等其他相关信息   。

## <a name="advanced-threat-protection-for-sql-servers-on-azure-vms-alerts"></a>Azure VM 警报上的 SQL Server 高级威胁防护
警报是因访问或攻击 SQL Server 的异常且可能有害的尝试而生成的。 这些事件可以触发以下警报：

### <a name="anomalous-access-pattern-alerts-preview"></a>异常访问模式警报（预览版）

* **来自异常位置的访问：** 当 SQL Server 的访问模式发生更改，有人从异常的地理位置登录到 SQL Server 时，会触发此警报。 可能的原因：
    * 攻击者或以前的恶意使用者访问了你的 SQL Server。
    * 合法用户从新位置访问了你的 SQL Server。
* **来自可能有害的应用程序的访问**：当使用可能有害的应用程序访问数据库时，会触发此警报。 可能的原因：
    * 攻击者尝试使用常见攻击工具破坏你的 SQL。
    * 合法的渗透测试正在进行中。
* **来自陌生主体的访问**：当 SQL Server 的访问模式发生更改，有人使用异常的主体（SQL 用户）登录到 SQL Server 时，会触发此警报。 可能的原因：
    * 攻击者或以前的恶意使用者访问了你的 SQL Server。 
    * 合法用户从新主体访问了你的 SQL Server。
* **暴力破解 SQL 凭据**：当有人使用不同的凭据异常登录并失败很多次时，会触发此警报。 可能的原因：
    * 攻击者尝试使用暴力攻击方式破坏你的 SQL。
    * 合法的渗透测试正在进行中。

### <a name="potential-sql-injection-attacks-supported-in-sql-server-2019"></a>潜在的 SQL 注入攻击（在 SQL Server 2019 中受支持）

* **存在易受 SQL 注入攻击的漏洞**：当某个应用程序在数据库中生成错误的 SQL 语句时，会触发此警报。 此警报会指示可能存在易受 SQL 注入攻击的漏洞。 可能的原因：
    * 应用程序代码中的缺陷导致构造出错误的 SQL 语句
    * 应用程序代码或存储过程在构造错误的 SQL 语句时无法清理用户输入，使该语句被 SQL 注入攻击利用
* **潜在 SQL 注入**：当 SQL 攻击有效利用已识别到的应用程序漏洞时，会触发此警报。 这意味着，攻击者正在尝试使用有漏洞的应用程序代码或存储过程注入恶意 SQL 语句。


### <a name="unsafe-command-supported-in-sql-server-2019"></a>不安全命令（在 SQL Server 2019 中受支持）

* **潜在的不安全操作**：执行具有高特权且可能不安全的命令将触发此警报。 可能的原因：
    * 启用了建议禁用以实现更好的安全状况的命令。
    * 攻击者尝试利用 SQL 访问或提升权限。   


## <a name="explore-and-investigate-security-alerts"></a>浏览并调查安全警报

可以在安全中心的警报页、资源的安全选项卡或通过警报电子邮件中的直接链接获取数据安全警报。

1. 查看警报：

    * 在安全中心，单击侧栏中的“安全警报”，然后选择警报。
    * 在资源范围中，打开相关资源页，然后在侧栏中单击“安全”。 

1. 警报各自独立，每个警报都包含详细的修正步骤和调查信息。 可以使用其他 Azure 安全中心和 Azure Sentinel 功能进一步调查，从而更深入地了解：

    * 启用 SQL Server 的审核功能以进一步调查。 如果你是 Azure Sentinel 用户，则可以将 SQL 审核日志从 Windows 安全日志事件上传到 Sentinel，并享受内容丰富的调查体验。
    * 若要改善安全状况，请采纳每个警报中针对主机的安全中心建议。 这将降低未来攻击的风险。 


<!--not available -->