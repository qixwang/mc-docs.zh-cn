---
title: include 文件
description: include 文件
services: storage
author: WenJason
ms.service: storage
ms.topic: include
origin.date: 12/27/2019
ms.date: 06/15/2020
ms.author: v-jay
ms.custom: include file
ms.openlocfilehash: 9c86ef1a232cbf01819ac4113363ddc48f204d9c
ms.sourcegitcommit: 3de7d92ac955272fd140ec47b3a0a7b1e287ca14
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/12/2020
ms.locfileid: "84723571"
---
默认情况下，虽然可将共享限制增加到 100 TiB，但标准文件共享只能跨越最多 5 TiB。 为此，必须在存储帐户级别启用大文件共享** 功能。 

只能在本地冗余的标准存储帐户上启用大文件共享。 启用大文件共享功能标志后，不能将冗余级别更改为异地冗余存储。

若要在现有存储帐户上启用大文件共享，请导航到存储帐户的目录中的“配置”视图，将大文件共享摇杆开关切换到“启用”：****

![在 Azure 门户中启用大文件共享摇杆开关的屏幕截图](media/storage-files-tiers-enable-large-shares/enable-lfs-0.png)

还可以通过 [`Set-AzStorageAccount`](https://docs.microsoft.com/powershell/module/az.storage/set-azstorageaccount) PowerShell cmdlet 和 [`az storage account update`](/cli/storage/account#az-storage-account-update) Azure CLI 命令启用 100 TiB 文件共享。 有关启用大型文件共享的详细说明，请参阅[启用和创建大型文件共享](../articles/storage/files/storage-files-how-to-create-large-file-share.md)。

要详细了解如何在新存储帐户上创建大型文件共享，请参阅[创建 Azure 文件共享](../articles/storage/files/storage-how-to-create-file-share.md)。