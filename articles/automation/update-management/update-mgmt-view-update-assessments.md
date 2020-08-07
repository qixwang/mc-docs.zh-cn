---
title: 查看 Azure 自动化更新评估
description: 本文介绍如何查看更新管理部署的更新评估。
services: automation
ms.subservice: update-management
origin.date: 07/28/2020
ms.date: 08/10/2020
ms.topic: conceptual
ms.openlocfilehash: 56d6d61b3472969dbc78380223cd70d7ead4ae05
ms.sourcegitcommit: e6b216b180734783219378410e13192e314a4497
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/05/2020
ms.locfileid: "87790534"
---
# <a name="view-update-assessments"></a>查看更新评估

在更新管理中，可以查看有关计算机、缺少的更新、更新部署和计划的更新部署的信息。

## <a name="sign-in-to-the-azure-portal"></a>登录到 Azure 门户

登录到 [Azure 门户](https://portal.azure.cn)

## <a name="view-update-assessment"></a>查看更新评估

在更新管理中，可以查看有关计算机、缺少的更新、更新部署和计划的更新部署的信息。

[![更新管理默认视图](./media/update-mgmt-overview/update-management-view.png)](./media/update-mgmt-overview/update-management-view-expanded.png#lightbox)

若要查看更新评估，请执行以下步骤：

1. 在 Azure 门户中，导航到“自动化帐户”，并从列表中选择启用了更新管理的自动化帐户。

2. 在自动化帐户中，从左侧窗格选择“更新管理”。

3. “更新管理”页上会列出你的环境所具有的更新。 如果发现缺少任何更新，则会在“缺少的更新”选项卡上显示缺少的更新列表。

   在“合规性”列中，可以看到计算机的最后评估时间。 在“更新代理就绪情况”列中，可以看到更新代理的运行状况。 如果有问题，请选择相应链接，转到可帮助你解决问题的疑难解答文档。

4. 在“信息链接”下，选择更新的链接以打开支持文章，获取有关更新的重要信息。

     [![查看更新状态](./media/update-mgmt-view-update-assessments/missing-updates.png)](./media/update-mgmt-view-update-assessments/missing-updates-expanded.png#lightbox)

5. 单击更新的其他位置，打开“日志搜索”窗格。 日志搜索的查询是为该特定更新预定义的。 可以修改此查询或创建自己的查询，以查看详细信息。

    [![查看日志查询结果](./media/update-mgmt-view-update-assessments/logsearch-results.png)](./media/update-mgmt-view-update-assessments/logsearch-results-expanded.png#lightbox)

## <a name="view-missing-updates"></a>查看缺少的更新

选择“缺少的更新”可查看计算机中缺少的更新列表。 每个更新都会列出，并且可供选择。 有关需要更新的计算机数的信息、操作系统详细信息，以及指向更多信息的链接都会显示。 “日志搜索”窗格还会显示有关更新的更多详细信息。

![缺少更新](./media/update-mgmt-view-update-assessments/automation-view-update-assessments-missing-updates.png)

## <a name="work-with-update-classifications"></a>使用更新分类

下表列出了更新管理中支持的更新分类，以及每个分类的定义。

### <a name="windows"></a>Windows

|分类  |说明  |
|---------|---------|
|关键更新     | 解决关键、非安全相关错误的特定问题的更新。        |
|安全更新     | 产品特定、安全相关问题的更新。        |
|更新汇总     | 一起打包以便于部署的一组修补程序。        |
|功能包     | 在产品版本以外发布的新产品功能。        |
|服务包     | 应用于应用程序的一组修补程序。        |
|定义更新     | 对病毒或其他定义文件的更新。        |
|工具     | 可帮助完成一个或多个任务的实用工具或功能。        |
|更新     | 对当前安装的应用程序或文件的更新。        |

### <a name="linux"></a>Linux

|分类  |说明  |
|---------|---------|
|关键和安全更新     | 特定问题或产品特定、安全相关问题的更新。         |
|其他更新     | 本质上不是关键更新或不是安全更新的所有其他更新。        |

对于 Linux，更新管理可以区分云中的关键更新和安全更新，同时显示评估数据。 （由于云中的数据扩充，可能会出现这种情况。）为了进行修补，更新管理依赖于计算机上提供的分类数据。 与其他发行版不同，CentOS 在产品的 RTM 版本中未提供此信息。 如果已将 CentOS 计算机配置为返回以下命令的安全数据，则更新管理可以基于分类进行修补：

```bash
sudo yum -q --security check-update
```

当前没有受支持的方法可用来在 CentOS 上提供原生分类数据。 目前，只能尽力为自己实现了此功能的客户提供支持。

## <a name="next-steps"></a>后续步骤

此过程中的下一阶段是 [将更新部署到非合规计算机并查看部署结果](update-mgmt-deploy-updates.md)。
