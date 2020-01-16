---
title: 转换为 CSV：模块参考
titleSuffix: Azure Machine Learning
description: 了解如何使用 Azure 机器学习中的“转换为 CSV”模块将数据集转换为可下载、导出或与 R 或 Python 脚本模块共享的 CSV 格式。
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
author: xiaoharper
ms.author: zhanxia
ms.date: 10/22/2019
ms.openlocfilehash: 5bdaa82012c318ba6ae9c8a953dadf18fe7124b0
ms.sourcegitcommit: 623d64ef33e80d5f84b6dcf6d1ef4120fe4b8c08
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/02/2020
ms.locfileid: "75598769"
---
# <a name="convert-to-csv-module"></a>“转换为 CSV”模块

本文介绍了 Azure 机器学习设计器（预览版）中的一个模块。

使用此模块，可以将数据集转换为可下载、导出或与 R 或 Python 脚本模块共享的 CSV 格式。

### <a name="more-about-the-csv-format"></a>有关 CSV 格式的详细信息 

CSV 格式（代表“逗号分隔值”）是许多外部机器学习工具使用的一种文件格式。 CSV 是使用开源语言（例如 R 或 Python）时常用的一种交换格式。

虽然你可以在 Azure 机器学习中执行大部分工作，但有时你可能会发现将数据集转换为 CSV 以在外部工具中使用非常方便。 例如：

+ 下载 CSV 文件以使用 Excel 打开它，或将其导入到关系数据库中。  
+ 将 CSV 文件保存到云存储，并从 Power BI 连接到它以创建可视化效果。  
+ 使用 CSV 格式准备要在 R 和 Python 中使用的数据。 只需右键单击模块的输出即可生成直接从 Python 或 Jupyter 笔记本访问数据所需的代码。 

将数据集转换为 CSV 时，该文件将保存在 Azure ML 工作区中。 你可以使用 Azure 存储实用工具直接打开并使用该文件，也可以右键单击模块输出并将 CSV 文件下载到你的计算机上，或在 R 或 Python 代码中使用它。  

## <a name="how-to-configure-convert-to-csv"></a>如何配置“转换为 CSV”


1.  将[转换为 CSV](./convert-to-csv.md) 模块添加到管道。 可以在设计器中的**数据格式转换**组中找到此模块。 

2. 将它连接到可以输出数据集的任何模块。   
  
3.  运行管道。

### <a name="results"></a>结果
  

双击[转换为 CSV](./convert-to-csv.md) 的输出，然后选择以下选项之一。  

 + **结果数据集 -> 下载**：立即以 CSV 格式打开数据的副本，可以将该副本保存到本地文件夹。 如果未指定文件夹，则会应用默认文件名，并将 CSV 文件保存在本地**下载**库中。


 + **结果数据集 -> 另存为数据集**：将 CSV 文件作为单独的数据集保存回 Azure ML 工作区。

 + **生成数据访问代码**：Azure ML 会生成两组代码，以便你使用 Python 或使用 R 访问数据。若要访问数据，请将代码片段复制到应用程序中。 （ *“生成数据访问代码”即将推出。* ）

## <a name="next-steps"></a>后续步骤

请参阅 Azure 机器学习的[可用模块集](module-reference.md)。 