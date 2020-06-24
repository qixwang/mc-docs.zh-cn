---
title: 复制或备份 Azure 流分析作业
description: 本文介绍如何复制或备份 Azure 流分析作业。
author: Johnnytechn
ms.author: v-johya
ms.reviewer: mamccrea
ms.service: stream-analytics
ms.topic: conceptual
origin.date: 09/11/2019
ms.date: 06/12/2020
ms.openlocfilehash: a1dfb38e6bfd6cfe39186f27a30e3d56fa7e4fb9
ms.sourcegitcommit: 3de7d92ac955272fd140ec47b3a0a7b1e287ca14
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/12/2020
ms.locfileid: "84723631"
---
# <a name="copy-or-back-up-azure-stream-analytics-jobs"></a>复制或备份 Azure 流分析作业

可以使用 Visual Studio Code 或 Visual Studio 来复制或备份已部署的 Azure 流分析作业。 将作业复制到其他区域时不会复制上次输出时间。 因此，在启动复制的作业时，不能使用[**上次停止时**](/stream-analytics/start-job#start-options)选项。

## <a name="before-you-begin"></a>准备阶段
* 如果没有 Azure 订阅，请创建一个[试用帐户](https://www.azure.cn/pricing/1rmb-trial/)。

* 登录到 [Azure 门户](https://portal.azure.cn/)。

* 安装[适用于 Visual Studio Code 的 Azure 流分析扩展](quick-create-vs-code.md#install-the-azure-stream-analytics-tools-extension)或[适用于 Visual Studio 的 Azure 流分析工具](quick-create-vs-code.md#install-the-azure-stream-analytics-tools-extension)。  

## <a name="visual-studio-code"></a>Visual Studio Code

1. 单击 Visual Studio Code 活动栏上的 **Azure** 图标，然后展开“流分析”节点****。 你的作业应显示在你的订阅下。

   ![打开流分析资源管理器](./media/vscode-explore-jobs/open-explorer.png)

2. 若要将作业导出到本地项目，请在 Visual Studio Code 的**流分析资源管理器**中找到要导出的作业。 然后为项目选择一个文件夹。

    ![在 Visual Studio Code 中导出 ASA 作业](./media/vscode-explore-jobs/export-job.png)

    项目会导出到所选文件夹并添加到当前工作区。

    ![在 Visual Studio Code 中导出 ASA 作业](./media/stream-analytics-manage-job/copy-backup-stream-analytics-jobs.png)

3. 若要使用另一名称将作业发布到其他区域或备份，请在查询编辑器 (\*.asaql) 中选择“从订阅中选择以发布”****，然后按照说明进行操作。

    ![在 Visual Studio Code 中发布到 Azure](./media/quick-create-vs-code/submit-job.png)

## <a name="visual-studio"></a>Visual Studio

1. 按照[将部署的 Azure 流分析作业导出到项目说明](/stream-analytics/stream-analytics-vs-tools#export-jobs-to-a-project)进行操作。

2. 在查询编辑器中打开 \*.asaql 文件，在脚本编辑器中选择“提交到 Azure”****，然后按照说明使用新名称将作业发布到其他区域或备份。

## <a name="next-steps"></a>后续步骤

* [快速入门：使用 Visual Studio Code 创建流分析作业](quick-create-vs-code.md)
* [快速入门：使用 Visual Studio 创建流分析作业](stream-analytics-quick-create-vs.md)
* [使用 Azure 管道通过 CI/CD 部署 Azure 流分析作业](stream-analytics-tools-visual-studio-cicd-vsts.md)

