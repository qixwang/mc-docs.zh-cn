---
title: include 文件
description: include 文件
services: storage
author: WenJason
ms.author: v-jay
origin.date: 04/08/2020
ms.date: 06/15/2020
ms.service: storage
ms.subservice: blobs
ms.topic: include
ms.reviewer: hux
ms.custom: include file
ms.openlocfilehash: c0940b08d47451bde0cdddb1a4768bfe4336b96e
ms.sourcegitcommit: 3de7d92ac955272fd140ec47b3a0a7b1e287ca14
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/12/2020
ms.locfileid: "84723698"
---
若要读取存档存储中的数据，必须先将 Blob 的层更改为热层或冷层。 此过程称为解冻，可能需要多个小时才能完成。 建议使用较大的 Blob 大小，以优化解冻性能。 同时解冻多个小型 Blob 可能导致该时间延长。 目前有两种解冻优先级：“高”和“标准”，可以在[设置 Blob 层](https://docs.microsoft.com/rest/api/storageservices/set-blob-tier)或[复制 Blob](https://docs.microsoft.com/rest/api/storageservices/copy-blob) 操作中通过可选的 x-ms-rehydrate-priority 属性进行设置**。

* **标准优先级**：解冻请求将按其接收顺序处理，最长可能需要 15 个小时。
* **高优先级**：解冻请求优先于标准请求，可在 1 小时内完成。 高优先级请求可能需要花费 1 小时以上，具体取决于 Blob 大小和当前需求。 保证高优先级请求优先于标准优先级请求。

> [!NOTE]
> 标准优先级是存档的默认解冻选项。 高优先级是更快的选项，其费用高于标准优先级解冻，通常保留用于紧急数据还原。

发起解冻请求后，将无法取消。 在解冻过程中，x-ms-access-tier blob 属性将继续显示为存档，直到解冻过程进展到联机层**。 要确认解冻状态和进度，可以调用[获取 Blob 属性](https://docs.microsoft.com/rest/api/storageservices/get-blob-properties)以查看 x-ms-archive-status 和 x-ms-rehydrate-priority blob 属性** **。 存档状态可显示为“rehydrate-pending-to-hot”或“rehydrate-pending-to-cool”，具体取决于解除冻结目标层。 解冻优先级将指示“高”或“标准”的速度。 解冻完成后，将删除存档状态和解冻优先级属性，并更新访问层 blob 属性以反映所选的热存储层或冷存储层。
