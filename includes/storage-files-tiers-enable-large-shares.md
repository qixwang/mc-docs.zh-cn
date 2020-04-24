---
title: include 文件
description: include 文件
services: storage
author: WenJason
ms.service: storage
ms.topic: include
origin.date: 12/27/2019
ms.date: 03/09/2020
ms.author: v-jay
ms.custom: include file
ms.openlocfilehash: b9cf84846751aecddf60d140015e1c4f6518860b
ms.sourcegitcommit: c1ba5a62f30ac0a3acb337fb77431de6493e6096
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2020
ms.locfileid: "78411256"
---
默认情况下，虽然可将共享限制增加到 100 TiB，但标准文件共享只能跨越最多 5 TiB。 为此，必须在存储帐户级别启用大文件共享  功能。 高级存储帐户没有大文件共享功能标志，因为所有高级文件共享都可以最高可预配到完整的 100 TiB 容量。

只能在本地冗余的标准存储帐户上启用大文件共享。 启用大文件共享功能标志后，不能将冗余级别更改为异地冗余存储。

若要在现有存储帐户上启用大文件共享，请导航到存储帐户的目录中的“配置”视图，将大文件共享摇杆开关切换到“启用”： 

![在 Azure 门户中启用大文件共享摇杆开关的屏幕截图](media/storage-files-tiers-enable-large-shares/enable-lfs-0.png)

还可以通过 [`Set-AzStorageAccount`](https://docs.microsoft.com/powershell/module/az.storage/set-azstorageaccount) PowerShell cmdlet 和 [`az storage account update`](/cli/storage/account#az-storage-account-update) Azure CLI 命令启用 100 TiB 文件共享。

若要详细了解如何在新存储帐户上启用大文件共享，请参阅[创建 Azure 文件共享](../articles/storage/files/storage-how-to-create-file-share.md)。