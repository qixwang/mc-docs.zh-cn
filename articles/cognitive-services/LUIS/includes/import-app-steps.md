---
title: 导入应用步骤
services: cognitive-services
author: diberry
manager: nitinme
ms.service: cognitive-services
ms.topic: include
ms.date: 12/03/2019
ms.author: diberry
ms.openlocfilehash: 6bb7f7eb64100c1cd3cd7754e796bc582f60b049
ms.sourcegitcommit: d202f6fe068455461c8756b50e52acd4caf2d095
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/28/2020
ms.locfileid: "78155355"
---
1. 在 [LUIS 门户](https://luis.azure.cn)的“我的应用”  页上，选择“导入”  ，然后选择“导入为 JSON”  。 查找上一步中保存的 JSON 文件。 无需更改应用的名称。 选择“完成” 

1. 在“管理”部分的“版本”选项卡上选择版本，然后选择“克隆”以克隆版本，并为其提供一个新的 10 个字符的名称，然后选择“完成”以完成克隆流程     。 由于版本名称用作 URL 路由的一部分，因此该名称不能包含任何在 URL 中无效的字符。

    > [!TIP]
    > 在修改应用之前，最佳做法是克隆到新版本。 完成针对某个版本的操作后，将该版本导出为 .json 或 .lu 文件，然后将文件签入源代码管理系统中。

1. 选择“生成”，然后选择“意向”，以查看意向（LUIS 应用的主要构建基块）   。

    ![从“版本”页切换到“意向”页。](../media/tutorial-machine-learned-entity/new-version-imported-app.png)