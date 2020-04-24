---
title: include 文件
description: include 文件
services: azure-monitor
author: rboucher
tags: azure-service-management
ms.topic: include
ms.date: 07/22/2019
ms.author: bwren
ms.custom: include file
ms.openlocfilehash: af673ca911ca5b515614658e7f63b4055d323a01
ms.sourcegitcommit: c1ba5a62f30ac0a3acb337fb77431de6493e6096
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2020
ms.locfileid: "71151857"
---
| 限制 | 说明 |
|:---|:---|
| 查询语言 | Azure Monitor 使用与 Azure 数据资源管理器相同的 [Kusto 查询语言](https://docs.microsoft.com/azure/kusto/query/)。 有关 Azure Monitor 中不支持的 KQL 语言元素，请参阅 [Azure Monitor 日志查询语言差异](../articles/azure-monitor/log-query/data-explorer-difference.md)。 |
| Azure 区域 | 当数据跨多个 Azure 区域中的 Log Analytics 工作区时，日志查询可能会遇到过多的开销。 有关详细信息，请参阅[查询限制](../articles/azure-monitor/log-query/scope.md#query-limits)。 |
| 跨资源查询 | 单个查询中的 Application Insights 资源和 Log Analytics 工作区的最大数量限制为 100。<br>视图设计器不支持跨资源查询。<br>新的 scheduledQueryRules API 支持日志警报中的跨资源查询。<br>有关详细信息，请参阅[跨资源查询限制](../articles/azure-monitor/log-query/cross-workspace-query.md#cross-resource-query-limits)。 |