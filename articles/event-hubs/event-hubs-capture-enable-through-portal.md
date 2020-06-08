---
title: 事件中心 - 使用 Azure 门户捕获流式处理事件
description: 本文介绍如何使用 Azure 门户捕获通过 Azure 事件中心流式处理的事件。
services: event-hubs
documentationcenter: ''
author: spelluru
editor: ''
ms.assetid: ''
ms.service: event-hubs
ms.workload: na
ms.tgt_pltfrm: na
ms.custom: seodec18
ms.devlang: na
ms.topic: conceptual
origin.date: 02/12/2020
ms.date: 05/29/2020
ms.author: v-tawe
ms.openlocfilehash: 607928f7d2738991f090d4b1b2f822ec556bda14
ms.sourcegitcommit: be0a8e909fbce6b1b09699a721268f2fc7eb89de
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/29/2020
ms.locfileid: "84199433"
---
# <a name="enable-capturing-of-events-streaming-through-azure-event-hubs"></a>启用捕获通过 Azure 事件中心流式处理的事件

Azure [事件中心捕获][capture-overview]用于自动将事件中心中的流数据传送到所选 [Azure Blob 存储](https://www.azure.cn/home/features/storage/)帐户。

可以使用 [Azure 门户](https://portal.azure.cn)在创建事件中心时配置捕获。 可以将数据捕获到 Azure [Blob 存储](https://azure.microsoft.com/services/storage/blobs/)容器或 [Azure Data Lake Store Gen 2](https://azure.microsoft.com/services/data-lake-store/) 帐户。

有关详细信息，请参阅[事件中心捕获概述][capture-overview]。

## <a name="capture-data-to-azure-storage"></a>将数据捕获到 Azure 存储

创建事件中心以后，即可在“创建事件中心”门户屏幕中单击“启用”按钮，以便启用捕获。  然后在“捕获提供程序”框中单击“Azure 存储”，指定存储帐户和容器。  由于事件中心捕获对存储使用服务到服务身份验证，因此无需指定存储连接字符串。 资源选取器自动为存储帐户选择资源 URI。 如果使用 Azure Resource Manager，必须以字符串形式显式提供此 URI。

默认时间窗口为 5 分钟。 最小值为 1，最大值为 15。 **大小** 窗口的范围为 10-500 MB。

![捕获的时间范围][1]

> [!NOTE]
> 可以允许或禁止当捕获时间段内未发生任何事件时发出空文件。 

<!-- Not Available ## Capture data to an Azure Data Lake Store account-->

1. 请按照[创建存储帐户](../storage/common/storage-account-create.md?tabs=azure-portal#create-a-storage-account)一文创建 Azure 存储帐户。 请在“高级”选项卡上将“分层命名空间”设置为“启用”，使其成为 Azure Data Lake Storage Gen 2 帐户  。
2. 创建事件中心时，请执行以下步骤： 

    1. 将“捕获”设置为“打开” 。 
    2. 选择“Azure 存储”作为捕获提供程序。 “捕获提供程序”对应的 Azure Data Lake Store 选项为 Azure Data Lake Storage Gen 1 。 要使用 Azure Data Lake Storage Gen 2，请选择“Azure 存储”。
    2. 选择“选择容器”按钮。 

        ![启用捕获到 Data Lake Storage Gen 2](./media/event-hubs-capture-enable-through-portal/data-lake-storage-gen2.png)
3. 从列表中选择 Azure Data Lake Storage Gen 2 帐户。 

    ![选择 Data Lake Storage Gen2](./media/event-hubs-capture-enable-through-portal/select-data-lake-storage-gen2.png)
4. 选择“容器”（Data Lake Storage Gen 2 中的文件系统）。

    ![在存储中选择文件系统](./media/event-hubs-capture-enable-through-portal/select-file-system-data-lake-storage.png)
5. 在“创建事件中心”页面上，选择“创建” 。 

    ![选择“创建”按钮](./media/event-hubs-capture-enable-through-portal/create-event-hub-data-lake-storage.png)

    > [!NOTE]
    > 你使用此用户界面 (UI) 在 Azure Data Lake Storage Gen 2 中创建的容器将显示在“存储资源管理器”中的“文件系统”下 。 同样，你在 Data Lake Storage Gen 2 帐户下创建的文件系统在此 UI 中将显示为容器。 

<!-- 
## Capture data to Azure Data Lake Storage Gen 1 

To capture data to Azure Data Lake Storage Gen 1, you create a Data Lake Storage Gen 1 account, and an event hub:

### Create an Azure Data Lake Storage Gen 1 account and folders

1. Create a Data Lake Storage account, following the instructions in [Get started with Azure Data Lake Storage Gen 1 using the Azure portal](../data-lake-store/data-lake-store-get-started-portal.md).
2. Follow the instructions in the [Assign permissions to Event Hubs](../data-lake-store/data-lake-store-archive-eventhub-capture.md#assign-permissions-to-event-hubs) section to create a folder within the Data Lake Storage Gen 1 account in which you want to capture the data from Event Hubs, and assign permissions to Event Hubs so that it can write data into your Data Lake Storage Gen 1 account.  

### Create an event hub

1. The event hub must be in the same Azure subscription as the Azure Data Lake Storage Gen 1 account you created. Create the event hub, clicking the **On** button under **Capture** in the **Create Event Hub** portal page. 
2. In the **Create Event Hub** portal page, select **Azure Data Lake Store** from the **Capture Provider** box.
3. In **Select Store** next to the **Data Lake Store** drop-down list, specify the Data Lake Storage Gen 1 account you created previously, and in the **Data Lake Path** field, enter the path to the data folder you created.

    ![Select Data Lake Storage account][3]
-->

## <a name="add-or-configure-capture-on-an-existing-event-hub"></a>在现有的事件中心添加或配置捕获

可以在事件中心命名空间中的现有事件中心配置捕获。 若要对现有的事件中心启用“捕获”功能，或者要更改“捕获”设置，请单击命名空间以加载概览屏幕，然后单击要启用或更改“捕获”设置的事件中心。 最后，单击已打开页面左侧的“捕获”选项，然后编辑设置，如以下各图所示：

### <a name="azure-blob-storage"></a>Azure Blob 存储

![配置 Azure Blob 存储][2]

### <a name="azure-data-lake-storage-gen-2"></a>Azure Data Lake Storage Gen 2

![配置 Azure Data Lake Storage Gen 2](./media/event-hubs-capture-enable-through-portal/configure-data-lake-storage-gen2.png)

<!-- 
### Azure Data Lake Storage Gen 1 

![Configure Azure Data Lake Storage][4]

[1]: ./media/event-hubs-capture-enable-through-portal/event-hubs-capture1.png
[2]: ./media/event-hubs-capture-enable-through-portal/event-hubs-capture2.png
[3]: ./media/event-hubs-capture-enable-through-portal/event-hubs-capture3.png
[4]: ./media/event-hubs-capture-enable-through-portal/event-hubs-capture4.png
-->

## <a name="next-steps"></a>后续步骤

- 阅读[事件中心捕获概述][capture-overview]，详细了解事件中心捕获。
- 还可以通过 Azure Resource Manager 模板配置事件中心捕获。 有关详细信息，请参阅[通过 Azure 资源管理器模板启用捕获](event-hubs-resource-manager-namespace-event-hub-enable-capture.md)。
- [了解如何创建使用事件中心命名空间作为源的 Azure 事件网格订阅](store-captured-data-data-warehouse.md)

<!--Not available - [Get started with Azure Data Lake Store using the Azure portal](../data-lake-store/data-lake-store-get-started-portal.md)-->

[capture-overview]: event-hubs-capture-overview.md
