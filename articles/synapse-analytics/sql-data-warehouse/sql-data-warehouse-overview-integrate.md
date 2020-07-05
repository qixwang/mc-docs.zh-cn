---
title: 生成集成的解决方案
description: 与 Synapse SQL 池集成的解决方案工具和合作伙伴。
services: synapse-analytics
author: WenJason
manager: digimobile
ms.service: synapse-analytics
ms.topic: conceptual
ms.subservice: ''
origin.date: 04/17/2018
ms.date: 07/06/2020
ms.author: v-jay
ms.reviewer: igorstan
ms.custom: seo-lt-2019
ms.openlocfilehash: 757e8fe0f396cb45dcafee4682b90a7817350387
ms.sourcegitcommit: 7ea2d04481512e185a60fa3b0f7b0761e3ed7b59
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/02/2020
ms.locfileid: "85845810"
---
# <a name="integrate-other-services-with-a-synapse-sql-pool-data-warehouse"></a>将其他服务与 Synapse SQL 池（数据仓库）集成

借助 Azure Synapse Analytics 中的 SQL 池功能，用户能够与 Azure 中的许多其他服务集成。 使用 Synapse SQL，可以通过其 SQL 池资源创建数据仓库，然后可以使用多个附加服务，其中一些服务包括：

* Azure 数据工厂
* Azure 流分析

## <a name="azure-data-factory"></a>Azure 数据工厂

Azure 数据工厂为用户提供一个托管平台，用于创建复杂的提取和加载管道。 SQL 池与 Azure 数据工厂的集成包括：

* **存储过程**：协调存储过程的执行。
* **复制**：使用 ADF 将数据移动到 SQL 池。 实际上，此操作可以使用 ADF 标准数据移动机制或封面下的 PolyBase。

有关详细信息，请参阅[与 Azure 数据工厂集成](../../data-factory/load-azure-sql-data-warehouse.md?toc=/synapse-analytics/sql-data-warehouse/toc.json&bc=/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json)。

## <a name="azure-stream-analytics"></a>Azure 流分析

Azure 流分析是复杂、完全托管的基础结构，用于处理和使用从 Azure 事件中心生成的事件数据。  通过与 SQL 池集成，可以有效地处理流数据，并将其与关系数据一起存储以实现更深入、更高级的分析。  

* **作业输出：** 将流分析作业的输出直接发送到 SQL 池。

有关详细信息，请参阅[与 Azure 流分析集成](sql-data-warehouse-integrate-azure-stream-analytics.md)。
