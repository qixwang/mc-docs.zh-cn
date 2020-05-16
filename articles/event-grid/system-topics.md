---
title: Azure 事件网格中的系统主题
description: 介绍 Azure 事件网格中的系统主题。
services: event-grid
author: Johnnytechn
ms.service: event-grid
ms.topic: conceptual
ms.date: 05/06/2020
ms.author: v-johya
ms.openlocfilehash: 5ecd80979f75e26d785afb153b10832db31c0f2f
ms.sourcegitcommit: 81241aa44adbcac0764e2b5eb865b96ae56da6b7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/09/2020
ms.locfileid: "83002293"
---
# <a name="system-topics-in-azure-event-grid"></a>Azure 事件网格中的系统主题
当你为 Azure 事件源创建第一个事件订阅时，Azure 事件网格服务会创建系统主题。 目前，事件网格不会为 2020 年 3 月 15 日之前创建的主题源创建系统主题。 对于在此日期创建的或之后创建的所有主题源，事件网格会自动创建系统主题。 本文介绍 Azure 事件网格中的系统主题  。

## <a name="overview"></a>概述
为 Azure 事件源（如 Azure 存储帐户）创建第一个事件订阅时，该订阅的预配过程会创建类型为“Microsoft.EventGrid/systemTopics”  的附加资源。 删除 Azure 事件源的最后一个事件订阅后，系统主题会被自动删除。

系统主题不适用于自定义主题方案（即事件网格主题和事件网格域）。 

## <a name="location"></a>位置
对于位于特定区域/位置的 Azure 事件源，将在与 Azure 事件源相同的位置创建系统主题。 例如，如果为中国北部的 Azure Blob 存储创建事件订阅，则会在中国北部创建系统主题。 

## <a name="resource-group"></a>资源组 
通常，系统主题在 Azure 事件源所在的资源组中创建。 对于在 Azure 订阅范围内创建的事件订阅，系统主题在资源组“Default-EventGrid”  下创建。 如果该资源组不存在，则 Azure 事件网格会在创建系统主题之前创建该资源组。 

尝试使用存储帐户删除资源组时，你会在受影响的资源列表中看到系统主题。  

![删除资源组](./media/system-topics/delete-resource-group.png)

## <a name="next-steps"></a>后续步骤
请参阅以下文章： 

- [自定义主题](custom-topics.md)
- [域](event-domains.md)

