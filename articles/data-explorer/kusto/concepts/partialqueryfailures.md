---
title: 部分查询失败 - Azure 数据资源管理器 | Microsoft Docs
description: 本文介绍 Azure 数据资源管理器中的部分查询失败。
services: data-explorer
author: orspod
ms.author: v-tawe
ms.reviewer: rkarlin
ms.service: data-explorer
ms.topic: reference
origin.date: 10/23/2018
ms.date: 07/01/2020
ms.openlocfilehash: 29c99900d65c7efe65b2628d03b960434ba70420
ms.sourcegitcommit: 9bc3e55f01e0999f05e7b4ebaea95f3ac91d32eb
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/10/2020
ms.locfileid: "86226313"
---
# <a name="partial-query-failures"></a>部分查询失败

“部分查询失败”是运行查询失败，只有在查询已经开始实际执行阶段之后才会被检测到。 到那时，Kusto 已经将 HTTP 状态行 `200 OK` 返回给客户端，并且无法“收回”，因此必须在将查询结果返回给客户端的结果流中指示失败。 （事实上，它可能已将一些结果数据返回给调用方。）

存在几种类型的部分查询失败：
* [失控查询](runawayqueries.md)：占用过多资源的查询。
* [结果截断](resulttruncation.md)：由于其结果集超出某种限制而被截断的查询。
* [溢出](overflow.md)：触发溢出错误的查询。

可通过以下两种方式之一将部分查询失败报告回客户端：

* 作为结果数据的一部分（一个特别类型的记录会指示这不是数据本身）。 这是默认方式。
* 作为结果流中“QueryStatus”表的一部分。 这是通过使用请求的 `properties` 槽 (`Kusto.Data.Common.ClientRequestProperties.OptionDeferPartialQueryFailures`) 中的 `deferpartialqueryfailures` 选项完成的。
  执行此操作的客户端负责使用服务的整个结果流查找 `QueryStatus` 结果，并确保该结果中没有任何记录的 `Severity` 为 `2` 或更小。 