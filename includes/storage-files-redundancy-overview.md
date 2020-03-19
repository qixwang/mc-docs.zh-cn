---
title: include 文件
description: include 文件
services: storage
author: WenJason
ms.service: storage
ms.topic: include
origin.date: 12/27/2019
ms.date: 03/09/2020
ms.author: v-jay
ms.custom: include file
ms.openlocfilehash: bc2eda800ba102e642f39e179e6b109503d02e37
ms.sourcegitcommit: fbc7584f403417d3af7bd6bbbaed7c13a78c57b9
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/06/2020
ms.locfileid: "78411252"
---
为了在 Azure 文件共享中保护数据以防数据丢失或损坏，所有 Azure 文件共享都在写入每个文件时存储了该文件的多个副本。 可以根据工作负载的要求选择额外的冗余度。 Azure 文件存储目前支持以下数据冗余选项：

- **本地冗余**：本地冗余存储（通常称为 LRS）表示每个文件在 Azure 存储群集中存储三次。 这可以防止由于硬件故障（例如磁盘驱动器损坏）而导致数据丢失。
- **异地冗余**：异地冗余存储（通常称为 GRS）类似于本地冗余存储，因为文件在主要区域的 Azure 存储群集内存储三次。 然后，所有写入都将异步复制到 Azure 定义的次要区域。 异地冗余存储提供 6 个数据副本，这些副本分布在两个 Azure 区域。 如果发生重大灾难（例如，由于自然灾害或其他类似事件而导致 Azure 区域永久性丢失），Azure 会执行故障转移，使次要区域实际上成为主要区域，为所有操作提供服务。 由于主要区域和次要区域之间的复制是异步的，因此，在发生重大灾难时，尚未复制到次要区域的数据会丢失。 还可以对异地冗余存储帐户执行手动故障转移。

标准 Azure 文件共享支持这两种冗余类型。

常规用途版本 2 (GPv2) 存储帐户提供 Azure 文件存储不支持的额外冗余选项：读取访问异地冗余存储，通常称为“RA-GRS”。 可以在设置了这些选项的存储帐户中预配 Azure 文件共享，但 Azure 文件存储不支持从次要区域读取。 部署到读取访问异地冗余存储帐户中的 Azure 文件共享会按异地冗余存储计费。