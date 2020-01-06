---
title: 响应 Azure Blob 存储事件 | Microsoft Docs
description: 使用 Azure 事件网格订阅 Blob 存储事件。
author: WenJason
ms.author: v-jay
origin.date: 01/30/2018
ms.date: 01/06/2020
ms.topic: conceptual
ms.service: storage
ms.subservice: blobs
ms.reviewer: cbrooks
ms.openlocfilehash: dbcbbdc8b2016c993c8c21681546ed3a2dc34f2c
ms.sourcegitcommit: 6a8bf63f55c925e0e735e830d67029743d2c7c0a
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/03/2020
ms.locfileid: "75624273"
---
# <a name="reacting-to-blob-storage-events"></a>响应 Blob 存储事件

Azure 存储事件允许应用程序使用新式无服务器体系结构响应事件，例如 Blob 的创建和删除。 为此，它无需复杂的代码或高价低效的轮询服务。

相反，可以通过 [Azure 事件网格](/event-grid/)向订阅者（如 Azure Functions 或 Azure 逻辑应用，甚至是你的自定义 HTTP 侦听器）推送事件，且仅需为已使用的内容付费。

Blob 存储事件会可靠地发送到事件网格服务，该服务通过丰富的重试策略和死信传递向应用程序提供可靠的传递服务。

常见的 Blob 存储事件方案包括图像或视频处理、搜索索引或任何面向文件的工作流。 异步文件上传十分适合事件。 基于事件的体系结构对于鲜少更改，但要求立即响应的情况尤为有效。

如果现在就要进行此尝试，请参阅下述快速入门文章中的任一篇：

|若要使用此工具：    |请参阅此文： |
|--|-|
|Azure 门户    |[快速入门：利用 Azure 门户将 Blob 存储事件路由到 Web 终结点](/event-grid/blob-event-quickstart-portal?toc=%2fstorage%2fblobs%2ftoc.json)|
|PowerShell    |[快速入门：利用 PowerShell 将存储事件路由到 Web 终结点](/storage/blobs/storage-blob-event-quickstart-powershell?toc=%2fstorage%2fblobs%2ftoc.json)|
|Azure CLI    |[快速入门：使用 Azure CLI 将存储事件路由到 Web 终结点](/storage/blobs/storage-blob-event-quickstart?toc=%2fstorage%2fblobs%2ftoc.json)|

## <a name="the-event-model"></a>事件模型

事件网格使用[事件订阅](../../event-grid/concepts.md#event-subscriptions)将事件消息路由到订阅方。 此图展示了事件发布者、事件订阅和事件处理程序之间的关系。

![事件网格模型](./media/storage-blob-event-overview/event-grid-functional-model.png)

首先，将终结点订阅到事件。 然后，在触发某个事件时，事件网格服务会将有关该事件的数据发送到终结点。

请参阅 [Blob 存储事件架构](../../event-grid/event-schema-blob-storage.md?toc=%2fstorage%2fblobs%2ftoc.json)一文，以便查看：

> [!div class="checklist"]
> * Blob 存储事件的完整列表，以及如何触发每个事件。
> * 事件网格会针对每个此类事件发送的数据的示例。
> * 显示在数据中的每个键值对的用途。

## <a name="filtering-events"></a>筛选事件

可按事件类型以及已创建或已删除对象的容器名称和 blob 名称来筛选 blob 事件订阅。  可在[创建](/cli/azure/eventgrid/event-subscription?view=azure-cli-latest)事件订阅期间或[以后](/cli/azure/eventgrid/event-subscription?view=azure-cli-latest)将筛选器应用于事件订阅。 事件网格中的使用者筛选器基于“开始时间”和“结束时间”的匹配进行筛选，将含有匹配使用者的事件传送给订阅方。

若要详细了解如何应用筛选器，请参阅[筛选事件网格的事件](/event-grid/how-to-filter-events)。

Blob 存储事件使用者使用的格式：

```
/blobServices/default/containers/<containername>/blobs/<blobname>
```

要匹配存储帐户的所有事件，可将主题筛选器留空。

要匹配在一组共享前缀的容器中创建的 blob 的事件，请使用 `subjectBeginsWith` 筛选器，如下所示：

```
/blobServices/default/containers/containerprefix
```

要匹配在特定容器中创建的 blob 的事件，请使用 `subjectBeginsWith` 筛选器，如下所示：

```
/blobServices/default/containers/containername/
```

要匹配在共享 blob 名称前缀的特定容器中创建的 blob 的事件，请使用 `subjectBeginsWith` 筛选器，如下所示：

```
/blobServices/default/containers/containername/blobs/blobprefix
```

要匹配在共享 blob 后缀的特定容器中创建的 blob 事件，请使用 `subjectEndsWith` 筛选器，例如“.log”或“.jpg”。 有关详细信息，请参阅 [事件网格概念](../../event-grid/concepts.md#event-subscriptions)。

## <a name="practices-for-consuming-events"></a>使用事件的做法

处理 Blob 存储事件的应用程序应遵循以下建议的做法：
> [!div class="checklist"]
> * 由于可将多个订阅配置为将事件路由至相同的事件处理程序，因此请勿假定事件来自特定的源，而是应检查消息的主题，确保它来自所期望的存储帐户。
> * 同样，检查 eventType 是否为准备处理的项，并且不假定所接收的全部事件都是期望的类型。
> * 消息在一段延迟时间后会无序到达，请使用 etag 字段来了解对象的相关信息是否是最新的。  此外，还可使用 sequencer 字段来了解任何特定对象的事件顺序。
> * 使用 blobType 字段可了解 blob 中允许何种类型的操作，以及应当使用哪种客户端库类型来访问该 blob。 有效值为 `BlockBlob` 或 `PageBlob`。 
> * 将 URL 字段与 `CloudBlockBlob` 和 `CloudAppendBlob` 构造函数配合使用，以访问 blob。
> * 忽略不了解的字段。 此做法有助于适应将来可能添加的新功能。
> * 若要确保 **Microsoft.Storage.BlobCreated** 事件仅在块 Blob 完全提交后触发，请针对 `CopyBlob`、`PutBlob`、`PutBlockList` 或 `FlushWithClose` REST API 调用筛选此事件。 这些 API 调用仅在数据已完全提交到块 Blob 后才触发 **Microsoft.Storage.BlobCreated** 事件。 若要了解如何创建筛选器，请参阅[筛选事件网格的事件](/event-grid/how-to-filter-events)。


## <a name="next-steps"></a>后续步骤

了解关于事件网格的详细信息，尝试一下 Blob 存储事件：

- [关于事件网格](../../event-grid/overview.md)
- [将 Blob 存储事件路由到自定义 Web 终结点](storage-blob-event-quickstart.md)
