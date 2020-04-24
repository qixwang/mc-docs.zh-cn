---
title: include 文件
description: include 文件
services: batch
author: dlepow
ms.service: batch
ms.topic: include
origin.date: 04/06/2018
ms.date: 05/16/2018
ms.author: v-junlch
ms.custom: include file
ms.openlocfilehash: 1c7ade83d9a9eda7d5fd273a088b3d131893728a
ms.sourcegitcommit: c1ba5a62f30ac0a3acb337fb77431de6493e6096
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2020
ms.locfileid: "68912296"
---
在 Azure Batch 中运行的任务可能在其运行时产生输出数据。 通常需要存储任务输出数据，以便作业中的其他任务和/或执行该作业的客户端应用程序进行检索。 任务可向 Batch 计算节点的文件系统写入输出数据，但当重置节点映像或节点离开池时，节点上的所有数据都会丢失。 任务可能还具有文件保留期，超过此保留期后，系统将删除该任务创建的文件。 出于这些原因，请务必将以后需要的任务输出保留到数据存储，如 [Azure 存储](/storage/)。

有关 Batch 中的存储帐户选项，请参阅 [Batch 功能概述](../articles/batch/batch-api-basics.md#azure-storage-account)。

