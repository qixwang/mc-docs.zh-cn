---
title: include 文件
description: include 文件
services: storage
author: WenJason
ms.service: storage
ms.topic: include
origin.date: 01/14/2020
ms.date: 03/09/2020
ms.author: v-jay
ms.custom: include file
ms.openlocfilehash: 33ad79c87ed807dac779d052f445f6bfaacc9bfc
ms.sourcegitcommit: c1ba5a62f30ac0a3acb337fb77431de6493e6096
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2020
ms.locfileid: "78411260"
---
存储帐户的冗余选项包括：

* 本地冗余存储 (LRS)：简单的低成本冗余策略。 数据将在主要区域中同步复制三次。
* 异地冗余存储 (GRS)：为了防范区域性服务中断而提供的跨区域冗余。 数据在主要区域中以同步方式复制三次，然后以异步方式复制到次要区域。 若要对次要区域中的数据进行读取访问，请启用读取访问异地冗余存储 (RA-GRS)。

有关 Azure 存储中冗余选项的详细信息，请参阅 [Azure 存储冗余](../articles/storage/common/storage-redundancy.md)。
