---
title: 将内置分析与 Azure Cosmos DB 配合使用的用例。
description: 了解如何在不同用例中将内置分析与 Azure Cosmos DB 配合使用。
author: rockboyfor
ms.topic: conceptual
ms.service: cosmos-db
origin.date: 04/20/2020
ms.date: 06/15/2020
ms.author: v-yeche
ms.reviewer: sngun
ms.openlocfilehash: 34dab88821d8b120c801ae875cc48a8e9003b684
ms.sourcegitcommit: 3de7d92ac955272fd140ec47b3a0a7b1e287ca14
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/12/2020
ms.locfileid: "84723758"
---
# <a name="use-cases-for-built-in-analytics-with-azure-cosmos-db"></a>将内置分析与 Azure Cosmos DB 配合使用的用例

将 Azure Cosmos DB 中的内置分析与 Apache Spark 配合使用可以实现以下用例：

## <a name="htap-scenarios"></a>HTAP 方案

Azure Cosmos DB 中的内置分析可用于：

* 在事务处理期间检测欺诈行为。
* 在购物者浏览电子商务店铺时向他们提供建议。
* 提醒操作员关键的生产设备部件即将发生故障。
* 通过直接在操作数据中嵌入实时分析来创建快速可操作的见解。

Azure Cosmos DB 使用本机内置的 Apache Spark 来支持混合事务/分析处理 (HTAP) 方案。 Azure Cosmos DB 消除了传统系统存在的操作与分析隔离。

## <a name="multiple-regionally-distributed-data-lake-without-requiring-any-etl"></a>多区域分布式 Data Lake，无需任何 ETL 操作

Azure Cosmos DB 使用本机内置的 Apache Spark 提供快速、简单且可缩放的方式来构建多区域分布式 Data Lake，以提供单一系统映像。 多区域分布式多模型数据消除了投资昂贵 ETL 管道的需要，并可按需缩放，革新了数据团队分析其数据集的方式。

## <a name="time-series-analytics-over-historic-data"></a>基于历史数据的时序分析

在某些情况下，你可能需要根据过去完成的事件中特定时间点的数据来解答问题。 例如，要获取特定日期的 CRM 活动状态计数。 如果你在一周前运行了报告，则状态计数是根据该时间点的每个活动的状态计算的。 如果今天运行相同的报告，将会得到处于今天的状态的活动计数，而这些状态可能与过去一周的状态不同，因为这些活动经历了从开始到结束的生命周期。 因此，需要根据案例生命周期的每个阶段的快照提供报告。

使用 Azure Cosmos DB，用户可以实现“按时间顺序查看”的概念，可以追溯的方式针对数据查询和运行分析，并询问在特定的历史时间点查看数据的方式。 这意味着，用户可以轻松查看数据的当前视图和历史视图，并对其运行分析。

## <a name="multiple-regionally-distributed-machine-learning-and-ai"></a>多区域分布式机器学习和 AI

随着中国企业竞争的加剧、数据量呈 PB 量级的快速增大以及数据类型与格式日益增多，获取更深入、更准确的见解几乎是不可能的任务。 借助本机内置的 Apache Spark，Azure Cosmos DB 可提供多区域分布式分析平台，其中包含丰富的机器学习算法。 

<!--Not Available on You can use interactive Jupyter notebooks to build and train models, and cluster management capabilities. These capabilities enable you to provision highly tuned and auto-elastic Spark clusters on-demand.-->

## <a name="deep-learning-on-multi-model-multiple-regionally-distributed-data"></a>基于多模型多区域分布式数据的深度学习

随着数据量和复杂性的不断增大，深度学习是提供大数据预测分析解决方案的理想方式。 使用深度学习，企业可以运用强大的非结构化和半结构化数据来实现利用 AI、自然语言处理等技术的用例。

## <a name="reporting-integrating-with-power-apps-power-bi"></a>报告（与 Power Apps、Power BI 集成）

Power BI 提供交互式可视化和自助式商业智能功能，使最终用户能够自行创建报告和仪表板。 使用内置的 Spark 连接器可将 Power BI Desktop 连接到 Azure Cosmos DB 中的 Apache Spark 群集。 此连接器可让你使用直接查询将处理负载分散到 Azure Cosmos DB 中的 Apache Spark，当你不想要将大量的数据载入 Power BI，或者想要执行近实时分析时，此功能非常有用。

## <a name="iot-analytics-at-multiple-region-scale"></a>多区域规模的 IoT 分析

增加网络传感器后生成的数据可为以往不透明的系统和流程带来前所未有的可见性。 关键在于，无论 IoT 设备分布在中国的哪个区域，都要在此信息洪流中查找可操作的见解。 位于中国任何位置的 IOT 公司都可以使用 Azure Cosmos DB 实时分析高速传感器和时序数据。 它可以让你充分利用互联世界的真正价值，提供更好的客户体验、运营效率和新的收入机会。

## <a name="stream-processing-and-event-analytics"></a>流处理和事件分析 

从日志文件到传感器数据，企业越来越需要处理数据流的能力。 这些数据以稳定流的形式传入，并且往往同时来自多个源。 尽管可以将这些数据流存储在磁盘上并以追溯方式对其进行分析，但有时，最好或者必须在数据抵达时对其进行处理或采取措施。 例如，可以实时处理与金融交易相关的数据流，以识别并拒绝可能的欺诈性交易。

## <a name="interactive-analytics"></a>交互式分析

除了运行预定义的查询来创建有关销售、生产力或股票价格的静态仪表板以外，还可以交互方式浏览数据。 交互式分析允许提出问题、查看结果、根据响应更改初始问题，或更深入地钻取结果。 Azure Cosmos DB 中的 Apache Spark 通过快速响应和做出调整来支持交互式查询。

<!--Not Available on ## Data exploration using Jupyter notebooks-->

<!--Not Available on When you have a new dataset, before you dive into running models and tests, you need to inspect your data. In other words, you need to perform exploratory data analysis. Data exploration can inform several decisions. For example, you can find details such as the methods that are appropriate to use on your data, whether the data meets certain modeling assumptions, whether the data should be cleaned, restructured etc. By using the Azure Cosmos DB's natively built-in Jupyter notebooks and Apache Spark, you can do quick and effective exploratory data analysis on transactional and analytical data.-->

<!--Not Available on ## Next steps-->

<!--Not Available on To get started with these use cases on go to the following articles:-->

<!--Not Available on * [Built-in Jupyter notebooks in Azure Cosmos DB](cosmosdb-jupyter-notebooks.md)-->
<!--Not Available on * [How to enable notebooks for Azure Cosmos accounts](enable-notebooks.md)-->
<!--Not Available on * [Create a notebook to analyze and visualize data](create-notebook-visualize-data.md)-->

<!-- Update_Description: update meta properties, wording update, update link -->