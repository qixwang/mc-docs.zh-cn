---
title: Azure 数据资源管理器工具 - Azure 数据资源管理器 | Microsoft Docs
description: 本文介绍了 Azure 数据资源管理器中的 Azure 数据资源管理器工具。
services: data-explorer
author: orspod
ms.author: v-tawe
ms.reviewer: rkarlin
ms.service: data-explorer
ms.topic: reference
origin.date: 04/01/2020
ms.date: 07/01/2020
ms.openlocfilehash: 55ef46c52e64487a7d87a3bbccd7ba1100ed3bbe
ms.sourcegitcommit: c17e965d4ffd82fd7cd86b2648fcb0053a65df00
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/20/2020
ms.locfileid: "86470411"
---
# <a name="azure-data-explorer-tools"></a>Azure 数据资源管理器工具

## <a name="ad-hoc-query-tools"></a>临时查询工具

- Kusto.Explorer
  - [Kusto 安装和用户界面](./kusto-explorer.md) - 用于查询和控制 Kusto 的主要桌面工具
  - [使用 Kusto.Explorer](./kusto-explorer-using.md)
  - [Kusto.Explorer 故障排除](kusto-explorer-troubleshooting.md)
- [Web UI](../../web-query-data.md) - 用于查询 Kusto 的 Web UI

## <a name="visualizations-dashboards-and-reporting-tools"></a>可视化、仪表板和报告工具

- [Azure Notebooks](../../azure-notebooks.md) - 使用 Azure Notebooks 可分析 Azure 数据资源管理器中的数据。
- Excel

  - [Excel 空查询](../../excel-blank-query.md) - 将 Kusto 查询作为 Excel 数据源添加
  - [Excel 连接器](../../excel-connector.md) - 用于 Azure 数据资源管理器的 Excel 连接器

- PowerBI

  - [PowerBI 最佳实践](../../power-bi-best-practices.md)
  - [PowerBI 连接器](../../power-bi-connector.md)
  - [PowerBI 导入的查询](../../power-bi-imported-query.md)
  - [PowerBI SQL 查询](../../power-bi-sql-query.md)

- [Grafana](../../grafana.md)
- [K2Bridge 开源连接器](../../k2bridge.md) - 在 Kibana 中可视化来自 Azure 数据资源管理器的数据

## <a name="orchestration-tools"></a>业务流程工具

- Microsoft Flow
  - [Microsoft Flow 连接器](../../flow.md)
  - [Microsoft Flow 连接器用法示例](../../flow-usage.md)
- [Microsoft 逻辑应用](./logicapps.md) - 在 [Microsoft 逻辑应用](https://docs.azure.cn/logic-apps/)中自动执行 Kusto 查询

## <a name="data-ingestion-tools"></a>数据引入工具

- [LightIngest](../../lightingest.md) - 一种帮助实用程序，可以将数据临时引入 Azure 数据资源管理器
- [一键式引入](../../ingest-data-one-click.md) - 这种工具可快速引入数据并自动推荐表和映射结构
- [Azure 数据工厂](azure-data-factory.md)

## <a name="source-control-integration-tools"></a>源代码管理集成工具

- [Azure Pipelines](../../devops.md) - 在管道中调用控制命令
- [Sync Kusto](./synckusto.md) - 将 Kusto 存储函数同步到 Git 或从其同步
