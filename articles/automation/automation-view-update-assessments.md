---
title: 查看 Azure 自动化更新管理更新评估
description: 本文介绍了如何查看针对更新部署的更新评估。
services: automation
ms.subservice: update-management
origin.date: 01/21/2020
ms.date: 05/25/2020
ms.topic: conceptual
ms.openlocfilehash: 4bf14ea7aeca2e8050680d5d52171050ec02fac4
ms.sourcegitcommit: 981a75a78f8cf74ab5a76f9e6b0dc5978387be4b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/22/2020
ms.locfileid: "83801882"
---
# <a name="view-azure-automation-update-management-update-assessments"></a>查看 Azure 自动化更新管理更新评估

在 Azure 自动化帐户中，选择“更新管理”来查看计算机的状态。

此视图提供有关计算机、缺少的更新、更新部署和计划的更新部署的信息。 在“合规性”列中，可以看到计算机的最后评估时间。 在“更新代理准备”列中，可以看到更新代理的运行状况。 如果有问题，请选择相应链接，转到可帮助你解决问题的疑难解答文档。

若要运行日志搜索，以便返回有关计算机、更新或部署的信息，请在列表中选择相应的项。 此时会打开“日志搜索”窗格，其中显示了针对所选项的查询。

![更新管理默认视图](media/automation-update-management/update-management-view.png)

## <a name="view-missing-updates"></a>查看缺少的更新

选择“缺少的更新”可查看计算机中缺少的更新列表。 每个更新都会列出，并且可供选择。 将显示需要此更新的计算机数、操作系统详细信息，以及指向更多信息的链接。 “日志搜索”窗格还显示有关更新的更多详细信息。

![缺少更新](./media/automation-view-update-assessments/automation-view-update-assessments-missing-updates.png)

## <a name="update-classifications"></a>更新分类

下表列出了更新管理中支持的更新分类，以及每个分类的定义。

### <a name="windows"></a>Windows

|分类  |说明  |
|---------|---------|
|关键更新     | 特定问题的更新，用于解决关键的非安全性相关 bug。        |
|安全更新     | 特定于产品的、与安全性相关的问题的更新。        |
|更新汇总     | 一起打包以便于部署的一组修补程序。        |
|功能包     | 在产品版本以外发布的新产品功能。        |
|服务包     | 应用于某个应用程序的一组修补程序。        |
|定义更新     | 对病毒或其他定义文件的更新。        |
|工具     | 可帮助完成一个或多个任务的实用程序或功能。        |
|更新     | 对当前安装的应用程序或文件的更新。        |

### <a name="linux"></a><a name="linux-2"></a>Linux

|分类  |说明  |
|---------|---------|
|关键和安全更新     | 特定问题或产品特定、安全相关问题的更新。         |
|其他更新     | 本质上不是关键更新或不是安全更新的所有其他更新。        |

对于 Linux，更新管理可以区分云中的关键更新和安全更新，同时显示评估数据。 （之所以可以达到这样的粒度，是由于云中的数据扩充。）为了进行修补，更新管理依赖于计算机上提供的分类数据。 与其他发行版不同，CentOS 在产品的 RTM 版本中没有这些信息。 如果已将 CentOS 计算机配置为返回以下命令的安全数据，则更新管理能够基于分类进行修补：

```bash
sudo yum -q --security check-update
```

当前没有受支持的方法可用来在 CentOS 上提供原生分类数据。 目前，只能尽力为自己实现了此功能的客户提供支持。

## <a name="next-steps"></a>后续步骤

查看更新评估后，可以按照[管理 Azure VM 的更新和修补程序](automation-tutorial-update-management.md)中的步骤来计划更新部署。
