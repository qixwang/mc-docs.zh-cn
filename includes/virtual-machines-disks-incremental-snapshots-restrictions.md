---
title: include 文件
description: include 文件
services: virtual-machines
author: rockboyfor
ms.service: virtual-machines
ms.topic: include
origin.date: 03/05/2020
ms.date: 06/15/2020
ms.author: v-yeche
ms.custom: include file
ms.openlocfilehash: e7483647023db673d7a412e43e0aa7f7be7449f0
ms.sourcegitcommit: c4fc01b7451951ef7a9616fca494e1baf29db714
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/09/2020
ms.locfileid: "84574615"
---
<!--Verified successfully by PG team-->
- 增量快照当前无法在订阅之间转移。
- 当前，在任何给定时间，只能为某个特定系列的最多 5 个快照生成 SAS URI。
- 不能在磁盘的订阅之外为该磁盘创建增量快照。
- 每五分钟最多可为每个磁盘创建 7 个增量快照。
- 总共可以为单个磁盘创建 200 个增量快照。
- 如果父磁盘的大小调整超过 4 TB 限额，则无法获取分别在磁盘大小调整之前和之后拍摄的快照之间的差异。 需要再次下载调整大小后创建的快照的完整副本。 在那之后，可以获取大小调整超过 4 TB 后创建的快照之间的差异。

<!-- Update_Description: new article about virtual machines disks incremental snapshots restrictions -->
<!--NEW.date: 06/15/2020-->