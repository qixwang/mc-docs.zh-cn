---
title: 不支持的标量数据类型 - Azure 数据资源管理器 | Microsoft Docs
description: 本文介绍 Azure 数据资源管理器中不支持的标量数据类型。
services: data-explorer
author: orspod
ms.author: v-tawe
ms.reviewer: rkarlin
ms.service: data-explorer
ms.topic: reference
origin.date: 01/27/2020
ms.date: 07/31/2020
ms.openlocfilehash: 0e054af25a22253d40c59653516ed940c6b40332
ms.sourcegitcommit: 4e1bc2e9b2a12dbcc05c52db5dbd1ae290aeb18d
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/01/2020
ms.locfileid: "87509133"
---
# <a name="unsupported-scalar-data-types"></a>不支持的标量数据类型

在 Kusto 中，所有未记录的标量数据类型均被视为不受支持。

下面是不支持的类型。 某些类型以前受支持：

| 类型       | 其他名称   | 等效 .NET 类型              | 存储类型（内部名称）|
| ---------- | -------------------- | --------------------------------- | ----------------------------|
| `float`    |                      | `System.Single`                   | `R32`                       |
| `int16`    |                      | `System.Int16`                    | `I16`                       |
| `uint16`   |                      | `System.UInt16`                   | `UI16`                      |
| `uint32`   | `uint`               | `System.UInt32`                   | `UI32`                      |
| `uint64`   | `ulong`              | `System.UInt64`                   | `UI64`                      |
| `uint8`    | `byte`               | `System.Byte`                     | `UI8`                       |