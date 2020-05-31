---
title: 从 Azure VM 载入“更新管理”、“更改跟踪”和“清单”解决方案
description: 了解如何加入包含属于 Azure 自动化的“更新管理”解决方案的 Azure 虚拟机。
services: automation
origin.date: 03/04/2020
ms.date: 05/25/2020
ms.topic: conceptual
ms.custom: mvc
ms.openlocfilehash: f2c90d2878837b3e3c62137134671cbf9d1475df
ms.sourcegitcommit: 981a75a78f8cf74ab5a76f9e6b0dc5978387be4b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/22/2020
ms.locfileid: "83801832"
---
# <a name="onboard-update-management-solutions-from-an-azure-virtual-machine"></a>从 Azure 虚拟机加入更新管理解决方案

Azure 自动化提供所需的解决方案用于帮助管理计算机上安装的操作系统安全更新。 可通过两种方式加入计算机。 可以从虚拟机或者[从自动化帐户](automation-onboard-solutions-from-automation-account.md)加入解决方案。 本文介绍了如何从 Azure 虚拟机载入这些解决方案。

## <a name="sign-in-to-azure"></a>登录 Azure

通过 https://portal.azure.cn 登录到 Azure 门户。

## <a name="enable-the-solutions"></a>启用解决方案

首先，在 VM 上启用一个或全部三个解决方案：

1. 在 [Azure 门户](https://portal.azure.cn)中选择“虚拟机”，或在“主页”中搜索并选择“虚拟机” 。
2. 选择要为其启用解决方案的 VM。
3. 在 VM 页的“操作”下选择“更新管理”。 无论自动化帐户的位置如何，虚拟机都可以存在于任何区域中。 从 VM 加入解决方案时，需要拥有 `Microsoft.OperationalInsights/workspaces/read` 权限，以确定 VM 是否已加入工作区。 若要了解所需的其他权限，请参阅[加入计算机所需的权限](automation-role-based-access-control.md#onboarding-permissions)。 若要了解如何一次性加入多台计算机，请参阅[加入更新管理解决方案](automation-onboard-solutions-from-automation-account.md)。

4. 选择 Azure Log Analytics 工作区和自动化帐户，然后单击“启用”以启用解决方案。 启用此解决方案最长需要 15 分钟的时间。

![载入“更新管理”解决方案](media/automation-tutorial-update-management/manageupdates-update-enable.png)

## <a name="scope-configuration"></a>范围配置

每个解决方案在工作区中使用范围配置来确定获取解决方案的计算机。 范围配置是包含一个或多个已保存搜索的组，这些搜索用来将解决方案的范围限制为特定计算机。 访问范围配置：

1. 在自动化帐户中的“相关资源”下选择“工作区”。  
2. 在工作区中的“工作区数据源”下选择“范围配置”。 
3. 如果所选工作区中尚无“更新管理”解决方案，将创建以下范围配置：

    * `MicrosoftDefaultScopeConfig-Updates`

    如果所选工作区已有解决方案，则不会重新部署解决方案，也不会添加范围配置。

4. 单击任何配置中的省略号图标 ( **...** )，然后单击“编辑”。 
5. 在“编辑范围配置”窗格中，选择“选择计算机组”。 “计算机组”窗格将显示用来创建范围配置的已保存搜索。

## <a name="saved-searches"></a>保存的搜索

将计算机添加到“更新管理”解决方案后，会将该计算机添加到工作区中的已保存搜索中。 这些已保存搜索是包含这些解决方案所针对的目标计算机的查询。

转到你的工作区。 在“常规”下，选择“保存的搜索”。 下表中将显示这些解决方案使用的两个已保存搜索：

|名称     |Category  |别名  |
|---------|---------|---------|
|MicrosoftDefaultComputerGroup     | 更新        | Updates__MicrosoftDefaultComputerGroup         |

选择已保存的搜索以查看用于填充组的查询。 下图显示了查询及其结果：

![保存的搜索](media/automation-onboard-solutions-from-vm/logsearch.png)

## <a name="unlink-workspace"></a>取消工作区链接

以下解决方案依赖于 Log Analytics 工作区：

* [更新管理](automation-update-management.md)

如果你决定不再将自动化帐户与 Log Analytics 工作区集成，可以直接从 Azure 门户取消链接你的帐户。  在继续之前，首先需要删除前面所述的解决方案，否则此过程将无法继续。 查看已导入的特定解决方案的主题，了解删除该解决方案所需的步骤。

删除这些解决方案后，可以执行以下步骤取消链接自动化帐户。

> [!NOTE]
> 某些解决方案（包括早期版本的 Azure SQL 监视解决方案）可能已创建自动化资产并可能还需要在取消链接工作区之前删除。

1. 从 Azure 门户中打开你的自动化帐户，并在左侧的“相关资源”部分下，选择“链接的工作区” 。

2. 在“取消链接工作区”页上，单击“取消链接工作区”。

   ![“取消链接工作区”页](media/automation-onboard-solutions-from-vm/automation-unlink-workspace-blade.png)上获取。

   系统会提示用户确认是否要继续。

3. 当 Azure 自动化尝试从 Log Analytics 工作区中取消链接该帐户时，可以在菜单中的“通知”下跟踪进度。

（可选）删除解决方案后，你可能想要删除以下不再需要的项。

* 更新计划 - 每个计划都将具有与所创建的更新部署匹配的名称。

* 为解决方案创建的混合辅助角色组 - 每个混合辅助角色组的命名都将类似于 machine1.contoso.com_9ceb8108-26c9-4051-b6b3-227600d715c8。

或者，还可以在 Log Analytics 工作区中从自动化帐户取消链接你的工作区。 在工作区中，选择“相关资源”下的“自动化帐户”。  在“自动化帐户”页上，选择“取消链接帐户”。

## <a name="clean-up-resources"></a>清理资源

从更新管理中删除 VM：

* 在 Log Analytics 工作区中，从范围配置 `MicrosoftDefaultScopeConfig-Updates` 的已保存的搜索中删除 VM。 已保存的搜索位于工作区的“常规”下。
* 删除[适用于 Windows 的 Log Analytics 代理](../azure-monitor/learn/quick-collect-windows-computer.md#clean-up-resources)或[适用于 Linux 的 Log Analytics 代理](../azure-monitor/learn/quick-collect-linux-computer.md#clean-up-resources)。

## <a name="next-steps"></a>后续步骤

继续学习有关解决方案的教程来了解如何使用它们：

* [教程 - 管理 VM 的更新](automation-tutorial-update-management.md)
