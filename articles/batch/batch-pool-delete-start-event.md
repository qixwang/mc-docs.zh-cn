---
title: Azure Batch 池删除开始事件
description: Batch 池删除开始事件参考。 当池删除操作开始时，会发出此事件。
services: batch
author: lingliw
manager: digimobile
ms.assetid: ''
ms.service: batch
ms.topic: article
ms.tgt_pltfrm: ''
ms.workload: big-compute
origin.date: 04/20/2017
ms.date: 06/20/2019
ms.author: v-lingwu
ms.openlocfilehash: d6dbffae44e2c0741e056aa1b9a559bb316d0e22
ms.sourcegitcommit: 27eaabd82b12ad6a6840f30763034a6360977186
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/20/2020
ms.locfileid: "77497385"
---
# <a name="pool-delete-start-event"></a>池删除开始事件

 当池删除操作开始时，会发出此事件。 由于池删除是异步事件，预计在删除操作完成后会发出池删除完成事件。

 以下示例显示了池删除开始事件的正文。

```
{
    "id": "myPool1"
}
```

|元素|类型|注释|
|-------------|----------|-----------|
|`id`|String|池的 ID。|

<!-- Update_Description: update metedata properties -->