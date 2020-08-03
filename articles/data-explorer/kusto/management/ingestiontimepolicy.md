---
title: IngestionTime 策略 - Azure 数据资源管理器
description: 本文介绍 Azure 数据资源管理器中的 IngestionTime 策略。
services: data-explorer
author: orspod
ms.author: v-tawe
ms.reviewer: rkarlin
ms.service: data-explorer
ms.topic: reference
origin.date: 02/19/2020
ms.date: 07/01/2020
ms.openlocfilehash: 6cecd286f74ed542ad8e20f923cdc137c609ad23
ms.sourcegitcommit: c17e965d4ffd82fd7cd86b2648fcb0053a65df00
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/20/2020
ms.locfileid: "86470481"
---
# <a name="ingestiontime-policy"></a>IngestionTime 策略

IngestionTime 策略是可以对表设置（启用）的可选策略。

启用后，Kusto 会将隐藏的 `datetime` 列添加到名为 `$IngestionTime` 的表。
现在，每当引入新数据时，引入时间都会记录在此隐藏列中。
该时间是由 Kusto 群集在数据提交之前测量的。

> [!NOTE]
> 每条记录都有其自己的 `$IngestionTime` 值。

由于引入时间列处于隐藏状态，因此不能直接查询其值，
而只能使用名为 [ingestion_time()](../query/ingestiontimefunction.md) 的特殊函数来检索该值。 如果表中没有 `datetime` 列，或者在引入记录时未启用 IngestionTime 策略，则返回 NULL 值。

IngestionTime 策略设计用于两种主要方案：

- 允许用户估算引入数据时的延迟。
  包含日志数据的多个表具有时间戳列。 时间戳值由源填充，可指示生成记录的时间。 通过将该列的值与引入时间列进行比较，可以估算引入数据的延迟时间。

  > [!NOTE]
  > 计算得出的值只是一个估计值，因为源和 Kusto 不一定进行时钟同步。

- 为了支持允许用户发出连续查询的[数据库游标](../management/databasecursor.md)，查询仅限于自上次查询以来引入的数据。

获取详细信息。 请参阅[用于管理 IngestionTime 策略的控制命令](../management/ingestiontime-policy.md)。
