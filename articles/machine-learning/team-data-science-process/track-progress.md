---
title: 跟踪 Team Data Science Process 项目的进度
description: 数据科学组管理员、团队主管和项目主管如何跟踪数据科学项目的进度。
author: marktab
manager: cgronlun
editor: cgronlun
ms.service: machine-learning
ms.subservice: team-data-science-process
ms.topic: article
ms.date: 09/26/2019
ms.author: tdsp
ms.custom: seodec18, previous-author=deguhath, previous-ms.author=deguhath
ms.openlocfilehash: a6d9be9506e7ac86ca798c439da8414db52a9356
ms.sourcegitcommit: 623d64ef33e80d5f84b6dcf6d1ef4120fe4b8c08
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/02/2020
ms.locfileid: "75599205"
---
# <a name="track-the-progress-of-data-science-projects"></a>跟踪数据科学项目的进度

数据科学组管理员、团队主管和项目主管可以跟踪其项目的进度，例如完成了什么工作、谁完成了工作，以及剩余了什么工作。 

## <a name="azure-devops-dashboards"></a>Azure DevOps 仪表板

如果使用的是 Azure DevOps，则可以生成仪表板来跟踪与给定敏捷项目关联的活动和工作项。 有关仪表板的详细信息，请参阅[仪表板、报表和小组件](https://docs.microsoft.com/azure/devops/report/dashboards/)。

有关如何在 Azure DevOps 中创建和自定义仪表板与小组件的说明，请参阅以下快速入门：

- [添加和管理仪表板](https://docs.microsoft.com/azure/devops/report/dashboards/dashboards)
- [将小组件添加到仪表板](https://docs.microsoft.com/azure/devops/report/dashboards/add-widget-to-dashboard)

## <a name="example-dashboard"></a>示例仪表板

下面是一个简单的示例仪表板，用于跟踪敏捷数据科学项目的冲刺活动，包括目标为关联的存储库的提交的数目。 

- “倒计时”磁贴显示当前冲刺 (sprint) 中剩余的天数。  

- 这两个**代码磁贴**显示过去七天内两个项目存储库中的提交数。 

- **TDSP 客户项目的工作项**显示所有工作项的查询结果及其状态。 

- **累积流关系图** (CFD) 显示已关闭的和活动的工作项的数目。

- **燃尽图**显示仍需在冲刺 (sprint) 的剩余时间内完成的工作。

- **燃耗图表**显示已完成工作与冲刺 (sprint) 中的总工时数的比较。

![仪表板](./media/track-progress/dashboard.png)

## <a name="next-steps"></a>后续步骤

[执行 Team Data Science Process 的演练](walkthroughs.md)列出了可演示特定情况下过程中的所有步骤的演练，其中包含链接和缩略图说明。 链接的方案展示了如何将云、本地工具以及服务合并到工作流或管道中，以此创建智能应用程序。 
