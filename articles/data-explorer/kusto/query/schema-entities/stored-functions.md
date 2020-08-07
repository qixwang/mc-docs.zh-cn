---
title: 存储函数 - Azure 数据资源管理器 | Microsoft Docs
description: 本文介绍 Azure 数据资源管理器中的存储函数。
services: data-explorer
author: orspod
ms.author: v-tawe
ms.reviewer: rkarlin
ms.service: data-explorer
ms.topic: reference
origin.date: 10/23/2018
ms.date: 07/31/2020
ms.openlocfilehash: 2f6adac8f6ec4f189369d8f44d5a30cbea01aedc
ms.sourcegitcommit: 4e1bc2e9b2a12dbcc05c52db5dbd1ae290aeb18d
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/01/2020
ms.locfileid: "87509234"
---
# <a name="stored-functions"></a>存储函数

**函数**是可重用的查询或查询部件。 可将函数存储为数据库实体（与表等类似），这些实体称为“存储函数”。 还可以使用 [let 语句](../letstatement.md)，通过名为 lambda 表达式的机制在查询内以临时方式创建函数。

有关函数调用语法和规则的更多详细信息，请参阅[用户定义的函数](../functions/user-defined-functions.md)。
有关如何创建和管理存储函数的详细信息，请参阅[管理函数](../../management/functions.md)。