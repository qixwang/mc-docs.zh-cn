---
title: evaluate 插件运算符 - Azure 数据资源管理器 | Microsoft Docs
description: 本文介绍了 Azure 数据资源管理器中的 evaluate 插件运算符。
services: data-explorer
author: orspod
ms.author: v-tawe
ms.reviewer: alexans
ms.service: data-explorer
ms.topic: reference
origin.date: 10/30/2019
ms.date: 08/06/2020
ms.openlocfilehash: 6f7ace92245ab814f2bcd07ef99f90ca858f1ccb
ms.sourcegitcommit: 7ceeca89c0f0057610d998b64c000a2bb0a57285
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/06/2020
ms.locfileid: "87841462"
---
# <a name="evaluate-operator-plugins"></a>evaluate 运算符插件

调用服务端查询扩展（插件）。

`evaluate` 运算符是一个表格运算符，它提供了调用查询语言扩展（称为**插件**）的功能。 插件可以启用或禁用（与始终可用的其他语言构造不同），并且不受语言的关系特性“约束”（例如，它们可能没有预定义的、静态确定的输出架构）。

**语法** 

[*T* `|`] `evaluate` [ *evaluateParameters* ] *PluginName* `(` [*PluginArg1* [`,` *PluginArg2*]... `)`

其中：

* T 是插件的可选表格输入。 （某些插件不接受任何输入，而充当表格数据源。）
* PluginName 是调用的插件的名称，为必需项。
* *PluginArg1*, ... 是插件的可选参数。
* evaluateParameters：零个或零个以上（以空格分隔）以 Name `=` Value 形式表示的参数，用于控制评估操作和执行计划的行为 。 每个插件可能会以不同方式确定如何处理每个参数。 有关具体行为，请参阅每个插件的文档。  

支持以下参数： 

  |名称                |值                           |说明                                |
  |--------------------|---------------------------------|-------------------------------------------|
  |`hint.distribution` |`single`, `per_node`, `per_shard`| [分布提示](#distributionhints) |
  |`hint.pass_filters` |`true`, `false`| 允许 `evaluate` 运算符在插件之前传递任何匹配筛选器。 如果筛选器引用存在于 `evaluate` 运算符之前的列，则认为它是“匹配的”。 默认：`false` |
  |`hint.pass_filters_column` |column_name| 允许插件运算符传递筛选器来引用插件前的 column_name。 参数可多次用于不同的列名称。 |

**备注**

* 从语法上讲，`evaluate` 的行为类似于 [invoke 运算符](./invokeoperator.md)，后者可调用表格函数。
* 通过 evaluate 运算符提供的插件不受查询执行或参数评估的常规规则约束。
* 特定插件可能有特定的限制。 例如，在执行跨群集查询时，不能使用输出架构依赖于数据的插件（例如 [bag_unpack 插件](./bag-unpackplugin.md)和 [pivot 插件](./pivotplugin.md)）。

<a id="distributionhints"> **分布提示** </a>

分布提示指定如何在多个群集节点之间分布插件执行。 每个插件可能会针对分布提供不同的支持。 插件的文档指定了插件支持的分布选项。

可能的值：

* `single`：插件的单个实例将针对整个查询数据运行。
* `per_node`：如果在插件调用之前的查询分布在节点之间，则会在每个节点上针对其包含的数据运行插件的一个实例。
* `per_shard`：如果在插件调用之前的数据分布在分片之间，则会针对数据的每个分片运行插件的一个实例。
