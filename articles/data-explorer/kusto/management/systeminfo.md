---
title: 系统信息 - Azure 数据资源管理器 | Microsoft Docs
description: 本文介绍 Azure 数据资源管理器中的系统信息。
services: data-explorer
author: orspod
ms.author: v-tawe
ms.reviewer: rkarlin
ms.service: data-explorer
ms.topic: reference
origin.date: 06/07/2019
ms.date: 07/01/2020
ms.openlocfilehash: 0a36e5a90cc6b115eb88be3931bc2790f3a0c69a
ms.sourcegitcommit: c17e965d4ffd82fd7cd86b2648fcb0053a65df00
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/20/2020
ms.locfileid: "86470440"
---
# <a name="system-information"></a>系统信息

本部分总结了可供 `Database Admins` 和 `Database Monitors` 角色使用的命令，这些命令可用于探索用法、跟踪操作和调查引入失败。

- [.show queries](queries.md) - 显示有关已完成的和正在运行的查询的信息。
- [.show commands](commands.md) - 显示有关已完成的命令及其资源利用率的信息。
- [.show commands-and-queries](commands-and-queries.md) - 显示有关已完成的命令和查询及其资源利用率的信息。
- [.show journal](journal.md) - 显示元数据操作的历史记录。
- [.show operations](operations.md) - 显示自上次选择管理节点以来正在运行的和已完成的管理操作。
- [.show failed ingestions](ingestionfailures.md) - 显示在将数据引入到群集期间遇到的故障的相关信息。
