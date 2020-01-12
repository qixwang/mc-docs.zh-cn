---
title: 使用 Visual Studio 在本地测试 Azure 流分析查询
description: 本文介绍如何使用针对 Visual Studio 的 Azure 流分析工具在本地测试查询。
author: rockboyfor
ms.author: v-yeche
manager: digimobile
ms.reviewer: mamccrea
ms.service: stream-analytics
ms.topic: conceptual
origin.date: 07/10/2018
ms.date: 09/30/2018
ms.openlocfilehash: 28c85b79eae6827d2ab8468c1b42a5fbf37f7045
ms.sourcegitcommit: e0b57f74aeb9022ccd16dc6836e0db2f40a7de39
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/10/2020
ms.locfileid: "75857293"
---
# <a name="test-stream-analytics-queries-locally-with-visual-studio"></a>使用 Visual Studio 在本地测试流分析查询

可使用针对 Visual Studio 的 Azure 流分析工具通过示例数据在本地测试流分析作业。

使用此[快速入门](stream-analytics-quick-create-vs.md)了解如何使用 Visual Studio 创建流分析作业。

## <a name="test-your-query"></a>测试查询

在 Azure 流分析项目中双击“Script.asaql”，在编辑器中打开脚本  。 可以编译查询，查看是否存在任何语法错误。 查询编辑器支持 IntelliSense、语法颜色设置和错误标记。

![查询编辑器](./media/stream-analytics-vs-tools-local-run/stream-analytics-tools-for-vs-query-01.png)

### <a name="add-local-input"></a>添加本地输入

若要针对本地静态数据验证查询，右键单击输入并选择“添加本地输入”  。

![添加本地输入](./media/stream-analytics-vs-tools-local-run/stream-analytics-tools-for-vs-add-local-input-01.png)

在弹出窗口中，从本地路径选择示例数据，然后单击“保存”  。

![添加本地输入](./media/stream-analytics-vs-tools-local-run/stream-analytics-tools-for-vs-add-local-input-02.png)

名为 local_EntryStream.json  的文件会自动添加到输入文件夹。

![本地输入文件夹的文件列表](./media/stream-analytics-vs-tools-local-run/stream-analytics-tools-for-vs-add-local-input-03.png)

在查询编辑器中选择“本地运行”  。 或者按 F5。

![在本地运行](./media/stream-analytics-vs-tools-local-run/stream-analytics-tools-for-vs-local-run-01.png)

可以直接从 Visual Studio 中以表格形式查看输出。

![表格形式的输出](./media/stream-analytics-vs-tools-local-run/stream-analytics-for-vs-local-result.png)

可从控制台输出中找到输出路径。 按任意键打开结果文件夹。

![本地运行](./media/stream-analytics-vs-tools-local-run/stream-analytics-tools-for-vs-local-run-02.png)

在本地文件夹中检查结果。

![本地文件夹结果](./media/stream-analytics-vs-tools-local-run/stream-analytics-tools-for-vs-local-run-03.png)

### <a name="sample-input"></a>示例输入
还可以将输入源中的示例输入数据收集到本地文件。 右键单击输入配置文件并选择“示例数据”  。 

![数据采样](./media/stream-analytics-vs-tools-local-run/stream-analytics-tools-for-vs-sample-data-01.png)

仅可从事件中心或 IoT 中心采样数据流。 其他输入源不受支持。 在弹出对话框中，填写用于保存示例数据的本地路径，然后选择“示例”  。

![示例数据配置](./media/stream-analytics-vs-tools-local-run/stream-analytics-tools-for-vs-sample-data-02.png)

可在“输出”窗口中查看进度。  

![示例数据输出](./media/stream-analytics-vs-tools-local-run/stream-analytics-tools-for-vs-sample-data-03.png)

## <a name="next-steps"></a>后续步骤

* [使用 Visual Studio 查看 Azure 流分析作业](/stream-analytics/stream-analytics-vs-tools)
* [快速入门：使用 Visual Studio 创建流分析作业](/stream-analytics/stream-analytics-quick-create-vs)
* [使用流分析工具进行持续集成和开发](stream-analytics-tools-for-visual-studio-cicd.md)

<!-- Update_Description: new articles on stream analytics vs tools local run -->

<!--ms.date: 09/30/2018-->

