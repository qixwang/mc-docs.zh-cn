---
title: 事件中心 - 使用 Azure 门户捕获流式处理事件
description: 本文介绍如何使用 Azure 门户捕获通过 Azure 事件中心流式处理的事件。
services: event-hubs
documentationcenter: ''
author: ShubhaVijayasarathy
manager: timlt
editor: ''
ms.assetid: ''
ms.service: event-hubs
ms.workload: na
ms.tgt_pltfrm: na
ms.custom: seodec18
ms.devlang: na
ms.topic: conceptual
origin.date: 12/20/2019
ms.date: 02/17/2020
ms.author: v-tawe
ms.openlocfilehash: 3154f991a1410a535bc1042036db7f66e9792d91
ms.sourcegitcommit: 7c80405a6b48380814b4b414e9f8a5756c007880
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/07/2020
ms.locfileid: "77067742"
---
# <a name="enable-capturing-of-events-streaming-through-azure-event-hubs"></a>启用捕获通过 Azure 事件中心流式处理的事件

Azure [事件中心捕获][capture-overview]用于自动将事件中心中的流数据传送到所选 [Azure Blob 存储](https://www.azure.cn/home/features/storage/)帐户。

<!-- Not Available [Azure Data Lake Store](https://www.azure.cn/home/features/data-lake-store/) -->

可以使用 [Azure 门户](https://portal.azure.cn)在创建事件中心时配置捕获。 可以将数据捕获到 Azure [Blob 存储](https://www.azure.cn/home/features/storage/)容器。

<!-- Not Available [Azure Data Lake Store](https://www.azure.cn/home/features/data-lake-store/) account.-->

有关详细信息，请参阅[事件中心捕获概述][capture-overview]。

## <a name="capture-data-to-an-azure-storage-account"></a>将数据捕获到 Azure 存储帐户  

创建事件中心以后，即可在“创建事件中心”门户屏幕中单击“启用”按钮，以便启用捕获。   然后在“捕获提供程序”框中单击“Azure 存储”，指定存储帐户和容器。   由于事件中心捕获对存储使用服务到服务身份验证，因此无需指定存储连接字符串。 资源选取器自动为存储帐户选择资源 URI。 如果使用 Azure Resource Manager，必须以字符串形式显式提供此 URI。

默认时间窗口为 5 分钟。 最小值为 1，最大值为 15。 **大小** 窗口的范围为 10-500 MB。

![捕获的时间范围][1]

> [!NOTE]
> 可以允许或禁止当捕获时间段内未发生任何事件时发出空文件。 

<!-- Not Available ## Capture data to an Azure Data Lake Store account-->

## <a name="add-or-configure-capture-on-an-existing-event-hub"></a>在现有的事件中心添加或配置捕获

可以在事件中心命名空间中的现有事件中心配置捕获。 若要对现有的事件中心启用“捕获”功能，或者要更改“捕获”设置，请单击命名空间以加载概览屏幕，然后单击要启用或更改“捕获”设置的事件中心。 最后，单击已打开页面左侧的“捕获”  选项，然后编辑设置，如以下各图所示：

### <a name="azure-blob-storage"></a>Azure Blob 存储

![配置 Azure Blob 存储][2]


<!-- Not Available ### Azure Data Lake Store-->

[1]: ./media/event-hubs-capture-enable-through-portal/event-hubs-capture1.png
[2]: ./media/event-hubs-capture-enable-through-portal/event-hubs-capture2.png
[3]: ./media/event-hubs-capture-enable-through-portal/event-hubs-capture3.png
[4]: ./media/event-hubs-capture-enable-through-portal/event-hubs-capture4.png

## <a name="next-steps"></a>后续步骤

- 阅读[事件中心捕获概述][capture-overview]，详细了解事件中心捕获。
- 还可以通过 Azure Resource Manager 模板配置事件中心捕获。 有关详细信息，请参阅[通过 Azure 资源管理器模板启用捕获](event-hubs-resource-manager-namespace-event-hub-enable-capture.md)。
- [了解如何创建使用事件中心命名空间作为源的 Azure 事件网格订阅](store-captured-data-data-warehouse.md)

<!--Not available - [Get started with Azure Data Lake Store using the Azure portal](../data-lake-store/data-lake-store-get-started-portal.md)-->

[capture-overview]: event-hubs-capture-overview.md

<!--Update_Description: update meta properties, wording update-->