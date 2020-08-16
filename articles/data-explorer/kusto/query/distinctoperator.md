---
title: distinct 运算符 - Azure 数据资源管理器 | Microsoft Docs
description: 本文介绍 Azure 数据资源管理器中的 distinct 运算符。
services: data-explorer
author: orspod
ms.author: v-tawe
ms.reviewer: rkarlin
ms.service: data-explorer
ms.topic: reference
origin.date: 02/13/2020
ms.date: 08/06/2020
ms.openlocfilehash: c2a70be054cbf2f1c35a2eeab34b12935a679187
ms.sourcegitcommit: 7ceeca89c0f0057610d998b64c000a2bb0a57285
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/06/2020
ms.locfileid: "87841681"
---
# <a name="distinct-operator"></a>distinct 运算符

生成一个表，其中包含输入表中所提供列的不同组合。 

```kusto
T | distinct Column1, Column2
```

生成一个表，其中包含输入表中所有列的不同组合。

```kusto
T | distinct *
```

**示例**

显示水果与价格的不同组合。

```kusto
Table | distinct fruit, price
```

:::image type="content" source="images/distinctoperator/distinct.PNG" alt-text="Distinct":::

**备注**

* 与 `summarize by ...` 不同，`distinct` 运算符支持提供星号 (`*`) 作为组键，使其更易于用于宽表。
* 如果分组依据键的基数很高，则可将 `summarize by ...` 与[随机策略](shufflequery.md)一起使用。