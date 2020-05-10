---
author: WenJason
ms.service: databox
ms.subservice: heavy
ms.topic: include
origin.date: 06/18/2019
ms.date: 05/11/2019
ms.author: v-jay
ms.openlocfilehash: 041554108b2cd01b889554c955c73c5f896f57ad
ms.sourcegitcommit: 95efd248f5ee3701f671dbd5cfe0aec9c9959a24
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/29/2020
ms.locfileid: "82516006"
---
下面是对复制到存储帐户的数据的大小限制。 请确保上传的数据符合这些限制。 有关这些限制的最新信息，请参阅 [Blob 存储的可伸缩性和性能目标](../articles/storage/blobs/scalability-targets.md)和 [Azure 文件存储的可伸缩性和性能目标](../articles/storage/files/storage-files-scale-targets.md)。

| 复制到 Azure 存储帐户的数据的大小                      | 默认限制          |
|---------------------------------------------------------------------|------------------------|
| 块 Blob 和页 blob                                            | 每个存储帐户 500 TiB。 <br> 这包括来自所有源（包括 Data Box）的数据。|
| Azure 文件                                                          | 每个共享 5 TB。<br> StorageAccount_AzureFiles 下的所有文件夹都须遵循此限制  。       |
