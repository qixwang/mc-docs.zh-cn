---
title: Azure 数据资源管理器数据可视化
description: 了解可视化 Azure 数据资源管理器数据的不同方式
services: data-explorer
author: orspod
ms.author: v-tawe
ms.reviewer: gabil
ms.service: data-explorer
ms.topic: conceptual
origin.date: 01/30/2020
ms.date: 03/16/2020
ms.openlocfilehash: e032013ad8922f8f88972a99e1a4c5d38d873eab
ms.sourcegitcommit: c1ba5a62f30ac0a3acb337fb77431de6493e6096
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2020
ms.locfileid: "80243944"
---
# <a name="data-visualization-with-azure-data-explorer"></a>使用 Azure 数据资源管理器进行数据可视化 

Azure 数据资源管理器是一项适用于日志和遥测数据的快速且高度可缩放的数据探索服务，可以用来针对大量数据构建复杂的分析解决方案。 Azure 数据资源管理器集成各种可视化工具，因此可以用来可视化数据，并在组织内共享结果。 该数据可以转换成能够对业务造成影响的可操作见解。

数据可视化和报告是数据分析过程中的关键步骤。 Azure 数据资源管理器支持许多项 BI 服务，因此你可以使用最适合自己方案和预算的一项。

## <a name="kusto-query-language-visualizations"></a>Kusto 查询语言可视化

Kusto 查询语言 [`render operator`](https://docs.microsoft.com/azure/kusto/query/renderoperator) 提供各种可视化效果（如表格、饼图和条形图）来描述查询结果。 查询可视化用于异常情况检测和预测、机器学习等。

## <a name="power-bi"></a>Power BI

Azure 数据资源管理器提供使用各种方法连接到 [Power BI](https://powerbi.microsoft.com) 的功能： 

  * [内置的本机 Power BI 连接器](/data-explorer/power-bi-connector)

  * [将查询从 Azure 数据资源管理器导入 Power BI](/data-explorer/power-bi-imported-query)
 
  * [SQL 查询](/data-explorer/power-bi-sql-query)。

## <a name="microsoft-excel"></a>Microsoft Excel

Azure 数据资源管理器提供了使用[内置本机 Excel 连接器](https://products.office.com/excel)连接到 [Microsoft Excel](excel-connector.md)，或者从 Azure 数据资源管理器[导入查询](excel-blank-query.md)到 Excel 的功能。

<!-- ## Grafana -->

<!-- [Grafana](https://grafana.com) provides an Azure Data Explorer plugin that enables you to visualize data from Azure Data Explorer. You [set up Azure Data Explorer as a data source for Grafana, and then visualize the data](/data-explorer/grafana).  -->

## <a name="odbc-connector"></a>ODBC 连接器

Azure 数据资源管理器提供了[开放式数据库连接 (ODBC) 连接器](connect-odbc.md)，因此任何支持 ODBC 的应用程序都可以连接到 Azure 数据资源管理器。

## <a name="tableau"></a>Tableau

Azure 数据资源管理器提供使用 [ODBC 连接器](https://www.tableau.com)连接到 [Tableau](/data-explorer/connect-odbc) 的功能，然后可[在 Tableau 中直观显示数据](tableau.md)。

## <a name="qlik"></a>Qlik

Azure 数据资源管理器提供使用 [ODBC 连接器](https://www.qlik.com)连接到 [Qlik](/data-explorer/connect-odbc) 的功能，然后可创建 Qlik Sense 仪表板并直观显示数据。


## <a name="sisense"></a>Sisense

Azure 数据资源管理器提供使用 JDBC 连接器连接到 [Sisense](https://www.sisense.com) 的功能。 请[将 Azure 数据资源管理器设置为 Sisense 的数据源，然后将数据可视化](/data-explorer/sisense)。

## <a name="redash"></a>Redash

可以使用 [Redash](https://redash.io/) 来构建仪表板以及将数据可视化。 [将 Azure 数据资源管理器设置为 Redash 的数据源，然后将数据可视化](/data-explorer/redash)。