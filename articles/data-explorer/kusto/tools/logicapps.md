---
title: 使用逻辑应用自动运行 Kusto 查询
description: 了解如何使用逻辑应用自动运行 Kusto 查询和命令并对其进行计划
author: orspod
ms.author: v-tawe
ms.reviewer: docohe
ms.service: data-explorer
ms.topic: conceptual
origin.date: 04/14/2020
ms.date: 07/01/2020
ms.openlocfilehash: 35838e0d9b1adea3fafef3b5ccc9a7d89775f45d
ms.sourcegitcommit: c17e965d4ffd82fd7cd86b2648fcb0053a65df00
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/20/2020
ms.locfileid: "86470522"
---
# <a name="microsoft-logic-app-and-azure-data-explorer"></a>Microsoft 逻辑应用和 Azure 数据资源管理器

Azure Kusto 逻辑应用连接器使你能够使用 [Microsoft 逻辑应用](https://docs.azure.cn/logic-apps/)连接器自动运行 Kusto 查询和命令，作为已计划或已触发的任务的一部分。

逻辑应用和 Flow 的构建基于同一连接器。 因此，适用于 Flow 的[限制](flow.md#limitations)、[操作](flow.md#azure-kusto-flow-actions)、[身份验证](flow.md#authentication)和[用法示例](flow.md#azure-kusto-flow-actions)也同样适用于逻辑应用，如 [Flow 文档页](flow.md)中所述。

## <a name="how-to-create-a-logic-app-with-azure-data-explorer"></a>如何使用 Azure 数据资源管理器创建逻辑应用

1. 打开 [Microsoft Azure 门户](https://portal.azure.cn/)。
1. 搜索 `logic app` 并将其选中。

   [![](./Images/logicapps/logicapp-search.png "Search for logic app")](./Images/logicapps/logicapp-search.png#lightbox)

1. 选择“+添加”。

   ![添加逻辑应用](./Images/logicapps/logicapp-add.png)

1. 输入所需的详细窗体信息：
   - 订阅
   - 资源组
   - 逻辑应用名称
   - 区域或集成服务环境
   - 位置
   - 日志分析打开或关闭
1. 选择“查看 + 创建”。

   ![创建逻辑应用](./Images/logicapps/logicapp-create-new.png)

1. 创建逻辑应用后，选择“编辑”。

   ![编辑逻辑应用设计器](./Images/logicapps/logicapp-editdesigner.png "logicapp-editdesigner")

1. 创建空白逻辑应用。

   ![逻辑应用空白模板](./Images/logicapps/logicapp-blanktemplate.png "logicapp-blanktemplate")

1. 添加重复操作并选择“Azure Kusto”。

   ![逻辑应用 Kusto Flow 连接器](./Images/logicapps/logicapp-kustoconnector.png "logicapp-kustoconnector")

## <a name="next-steps"></a>后续步骤

- 若要了解有关配置定期操作的详细信息，请参阅 [Flow 文档页](flow.md)
- 查看一些[用法示例](flow.md#azure-kusto-flow-actions)，了解如何配置逻辑应用操作
