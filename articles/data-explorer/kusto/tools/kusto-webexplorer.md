---
title: Kusto.WebExplorer - Azure 数据资源管理器 | Microsoft Docs
description: 本文介绍 Azure 数据资源管理器中的 Kusto.WebExplorer。
services: data-explorer
author: orspod
ms.author: v-tawe
ms.reviewer: rkarlin
ms.service: data-explorer
ms.topic: reference
origin.date: 02/24/2020
ms.date: 07/01/2020
ms.openlocfilehash: 0f12e7d81bc076badbd0e5b16d66f29722b84f74
ms.sourcegitcommit: c17e965d4ffd82fd7cd86b2648fcb0053a65df00
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/20/2020
ms.locfileid: "86470525"
---
# <a name="kustowebexplorer"></a>Kusto.WebExplorer

Kusto.WebExplorer 是一个 Web 应用程序，可用于向 Kusto 服务发送查询和控制命令。 应用程序托管在 https://dataexplorer.azure.cn/ 。



Kusto.WebExplorer 也可以由其他 Web 门户在 HTML IFRAME 中托管。
（例如，此操作由 [Azure 门户](https://portal.azure.cn)完成。）请参阅 [Monaco IDE](../api/monaco/monaco-kusto.md)，详细了解如何托管它以及它使用的 Monaco 编辑器。

## <a name="connect-to-multiple-clusters"></a>连接到多个群集

现在可以连接多个群集并在数据库和群集之间切换。
该工具旨在轻松识别你所连接的群集和数据库。

![替换文字](./Images/KustoTools-WebExplorer/AddingCluster.gif "AddingCluster")

## <a name="recall-results"></a>重新调用结果

通常在分析期间，我们运行多个查询，并且可能需要重新访问前一个查询的结果。 可以使用此功能重新调用结果，而不必重新运行查询。 数据通过本地客户端缓存提供。

![替换文字](./Images/KustoTools-WebExplorer/RecallResults.gif "RecallResults")

## <a name="enhanced-results-grid-control"></a>增强的结果网格控制

表格网格使你能够选择多个行、列和单元格。 通过选择多个单元格（例如 Excel）来计算聚合并透视数据。

![替换文字](./Images/KustoTools-WebExplorer/EnhancedGrid.gif "EnhancedGrid")

## <a name="intellisense--formatting"></a>Intellisense 和格式设置

可以使用美观的打印格式，方法是使用“Shift+Alt+F”快捷键、代码折叠（大纲）和 IntelliSense。

![替换文字](./Images/KustoTools-WebExplorer/Formating.gif "Formating")

## <a name="deep-linking"></a>深层链接

可以只复制深层链接或深层链接和查询。 还可以使用以下模板设置 URL 格式，使其包括群集、数据库和查询：

`https://dataexplorer.azure.cn/` [`clusters/` Cluster [`/databases/` Database [`?` Options]]]  

可以指定以下选项：

* `workspace=empty`：指示创建新的空工作区（无需重新调用以前的群集、选项卡和查询）。



![替换文字](./Images/KustoTools-WebExplorer/DeepLink.gif "DeepLink")

## <a name="feedback"></a>反馈

可以通过该工具提交反馈。
![替换文字](./Images/KustoTools-WebExplorer/Feedback.gif "反馈")
