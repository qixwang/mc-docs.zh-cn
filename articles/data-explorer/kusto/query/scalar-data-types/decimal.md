---
title: decimal 数据类型 - Azure 数据资源管理器 | Microsoft Docs
description: 本文介绍 Azure 数据资源管理器中的 decimal 数据类型。
services: data-explorer
author: orspod
ms.author: v-tawe
ms.reviewer: rkarlin
ms.service: data-explorer
ms.topic: reference
origin.date: 10/23/2018
ms.date: 07/31/2020
ms.openlocfilehash: 8e4ccef9d8d68e0db51d762ac8287d5e8fc2a8b4
ms.sourcegitcommit: 4e1bc2e9b2a12dbcc05c52db5dbd1ae290aeb18d
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/01/2020
ms.locfileid: "87509135"
---
# <a name="the-decimal-data-type"></a>decimal 数据类型

`decimal` 数据类型表示一个 128 位宽的十进制数字。

`decimal` 数据类型的文本与 .NET 的 `System.Data.SqlTypes.SqlDecimal` 具有相同的表示形式。

`decimal(1.0)`、`decimal(0.1)`、`decimal(1e5)` 均为 `decimal` 类型的文本。

有几种特殊文本形式：
* `decimal(null)`：这是 [NULL 值](null-values.md)。