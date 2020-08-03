---
title: .drop ingestion mapping - Azure 数据资源管理器 | Microsoft Docs
description: 本文介绍 Azure 数据资源管理器中的 .drop ingestion mapping。
services: data-explorer
author: orspod
ms.author: v-tawe
ms.reviewer: rkarlin
ms.service: data-explorer
ms.topic: reference
origin.date: 02/04/2020
ms.date: 07/01/2020
ms.openlocfilehash: 2c0e13b05f5fb546579bc7fcb38e6795604891f3
ms.sourcegitcommit: c17e965d4ffd82fd7cd86b2648fcb0053a65df00
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/20/2020
ms.locfileid: "86470387"
---
# <a name="drop-ingestion-mapping"></a>.drop ingestion mapping

从数据库中删除引入映射。

`.drop` `table` _TableName_ `ingestion` _MappingKind_ `mapping` _MappingName_

**示例**

```kusto
.drop table MyTable ingestion csv mapping "Mapping1"

.drop table MyTable ingestion json mapping "Mapping1"
```
