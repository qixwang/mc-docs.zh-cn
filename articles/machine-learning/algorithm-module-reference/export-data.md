---
title: 导出数据：模块参考
titleSuffix: Azure Machine Learning
description: 了解如何使用 Azure 机器学习中的“导出数据”模块，将管道中的结果、中间数据和工作数据保存到 Azure 机器学习之外的云存储目标中。
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
author: likebupt
ms.author: v-yiso
origin.date: 10/22/2019
ms.date: 03/09/2020
ms.openlocfilehash: d418192aeb4a98b43a4bb6a7cfe2d7f756a16062
ms.sourcegitcommit: c1ba5a62f30ac0a3acb337fb77431de6493e6096
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2020
ms.locfileid: "78155036"
---
# <a name="export-data-module"></a>导出数据模块

本文介绍 Azure 机器学习设计器（预览版）中的一个模块。

使用该模块将管道中的结果、中间数据和工作数据保存到 Azure 机器学习之外的云存储目标中。 

此模块支持将数据导出到以下云数据服务中：

- Azure Blob 容器
- Azure 文件共享
- Azure Data Lake
- Azure Data Lake Gen2

导出数据之前，首先需要在 Azure 机器学习工作区中注册数据存储。 有关详细信息，请参阅[访问 Azure 存储服务中的数据](../how-to-access-data.md)。

## <a name="how-to-configure-export-data"></a>如何配置“导出数据”

1. 在设计器中将“导出数据”模块添加到管道  。 可以在“输入和输出”类别中找到该模块  。

1. 将“导出数据”连接到包含要导出的数据的模块  。

1. 选择“导出数据”，打开“属性”窗格   。

1. 对于“数据存储”，请从下拉列表中选择现有数据存储  。 还可以创建新的数据存储。 通过访问[访问 Azure 存储服务中的数据](../how-to-access-data.md)来查看方法。

1. 定义数据存储中将数据写入其中的路径。 


1. 对于“文件格式”，请选择数据的存储格式  。
 
1. 运行管道。

## <a name="next-steps"></a>后续步骤

请参阅 Azure 机器学习的[可用模块集](module-reference.md)。 