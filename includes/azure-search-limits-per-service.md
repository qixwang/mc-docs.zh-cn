---
title: include 文件
description: include 文件
services: search
author: HeidiSteen
ms.service: search
ms.topic: include
origin.date: 05/06/2019
ms.date: 03/16/2020
ms.author: v-tawe
ms.custom: include file
ms.openlocfilehash: e237dd345ca03c4e39837db7bb06fc0c04ba68f4
ms.sourcegitcommit: 1d3d8dfdaf6281f06640cbee7124a1e8bf102c50
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/25/2020
ms.locfileid: "80272903"
---
存储受磁盘空间限制，或者受索引、文档或其他高级资源的*最大数目*的硬性限制，具体取决于哪一个限制先实施。 下表描述了存储限制。 有关索引、文档和其他对象的最大限制，请参阅[按资源限制](../articles/search/search-limits-quotas-capacity.md#index-limits)。

| 资源 | 免费 | 基本<sup>1</sup> | S1 | S2 | S3 | S3&nbsp;HD<sup>2</sup> | L1 | L2 |
| -------- | --- | --- | --- | --- | --- | --- | --- | --- |
| 服务级别协议 (SLA)<sup>3</sup>  |否 |是 |是 |是 |是 |是 |是 |是 |
| 每个分区的存储空间 |50 MB |2 GB |25 GB |100 GB |200 GB |200 GB |1 TB |2 TB |
| 每个服务的分区数 |不适用 |1 |12 |12 |12 |3 |12 |12 |
| 分区大小 |不适用 |2 GB |25 GB |100 GB |200 GB |200 GB |1 TB |2 TB |
| 副本 |不适用 |3 |12 |12 |12 |12 |12 |12 |

<sup>1</sup> 基本层有一个固定分区。 在这一层，附加的搜索单元用于为增加的查询工作负荷分配更多副本。

<sup>2</sup> S3 HD 的硬性限制为三个分区，低于 S3 的分区限制。 采用的分区限制较低是因为 S3 HD 的索引计数要高得多。 由于计算资源（存储和处理）和内容（索引和文档）都存在服务限制，因此会先达到内容限制。

<sup>3</sup> 为可计费服务提供了针对专用资源的服务级别协议。 免费服务和预览功能没有 SLA。 对于可计费服务，SLA 将在用户为服务预配足够冗余时生效。 查询（读取）SLA 需要两个或更多副本。 查询和索引（读-写）SLA 需要三个或更多副本。 分区数不属于 SLA 相关考虑因素。 