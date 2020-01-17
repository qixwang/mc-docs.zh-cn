---
title: include 文件
description: include 文件
services: storage
author: WenJason
ms.service: storage
ms.topic: include
origin.date: 06/28/2019
ms.date: 01/06/2020
ms.author: v-jay
ms.custom: include file
ms.openlocfilehash: 0cacdd5c1ff8466cacbf2e5ab289fc29216ae2cf
ms.sourcegitcommit: 6a8bf63f55c925e0e735e830d67029743d2c7c0a
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/03/2020
ms.locfileid: "75624147"
---
存储帐户的复制选项包括：

* [本地冗余存储 (LRS)](../articles/storage/common/storage-redundancy-lrs.md)：一种简单、低成本的复制策略。 数据将在主要区域中同步复制三次。
* [异地冗余存储 (GRS)](../articles/storage/common/storage-redundancy-grs.md)：为了防范区域性服务中断而提供的跨区域复制。 数据在主要区域中以同步方式复制三次，然后以异步方式复制到次要区域。 若要对次要区域中的数据进行读取访问，请启用读取访问异地冗余存储 (RA-GRS)。
