---
title: Azure Batch 池删除开始事件
description: 批处理池删除开始事件参考。 当池删除操作开始时，会发出此事件。
ms.topic: article
origin.date: 04/20/2017
ms.date: 06/20/2019
ms.author: v-lingwu
ms.openlocfilehash: 2e994c3bbdf8dbe7fe11290b860d749bc1bda6fa
ms.sourcegitcommit: cbaa1aef101f67bd094f6ad0b4be274bbc2d2537
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/28/2020
ms.locfileid: "84126572"
---
# <a name="pool-delete-start-event"></a>池删除开始事件

 当池删除操作开始时，会发出此事件。 由于池删除是异步事件，预计在删除操作完成后会发出池删除完成事件。

 以下示例显示了池删除开始事件的正文。

```
{
    "id": "myPool1"
}
```

|元素|类型|说明|
|-------------|----------|-----------|
|`id`|String|池的 ID。|

<!-- Update_Description: update metedata properties -->