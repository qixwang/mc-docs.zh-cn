---
title: include 文件
description: include 文件
services: storage
author: WenJason
ms.author: v-jay
origin.date: 08/09/2019
ms.date: 02/10/2020
ms.service: storage
ms.subservice: blobs
ms.topic: include
ms.reviewer: hux
ms.custom: include file
ms.openlocfilehash: 743907a186755ceda82fe6f3ab144033e0c40a61
ms.sourcegitcommit: c1ba5a62f30ac0a3acb337fb77431de6493e6096
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2020
ms.locfileid: "77028953"
---
若要读取存档存储中的数据，必须先将 Blob 的层更改为热层或冷层。 此过程称为解冻，可能需要多个小时才能完成。 建议使用较大的 Blob 大小，以优化解冻性能。 同时解冻多个小型 Blob 可能导致该时间延长。 目前有两种解冻优先级：高（预览版功能）和“标准”，可以在[设置 Blob 层](https://docs.microsoft.com/rest/api/storageservices/set-blob-tier)或[复制 Blob](https://docs.microsoft.com/rest/api/storageservices/copy-blob) 操作中通过可选的 *x-ms-rehydrate-priority* 属性进行设置。

* **标准优先级**：解冻请求将按其接收顺序处理，最长可能需要 15 个小时。
* **高优先级（预览版功能）** ：解冻请求优先于标准请求，可在 1 小时内完成。 高优先级请求可能需要花费 1 小时以上，具体取决于 Blob 大小和当前需求。 保证高优先级请求优先于标准优先级请求。

> [!NOTE]
> 标准优先级是存档的默认解冻选项。 高优先级是更快的选项，其费用高于标准优先级解冻，通常保留用于紧急数据还原。

发起解冻请求后，将无法取消。 在解除冻结期间，可以通过查看“存档状态”Blob 属性来确认该层是否已更改。  状态显示为“rehydrate-pending-to-hot”或“rehydrate-pending-to-cool”，具体取决于目标层。 完成后，“存档状态”属性会被删除，“访问层”Blob 属性会反映出新层是热层还是冷层。 
