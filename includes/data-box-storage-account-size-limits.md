---
author: WenJason
ms.service: databox
ms.subservice: heavy
ms.topic: include
origin.date: 06/18/2019
ms.date: 09/09/2019
ms.author: v-jay
ms.openlocfilehash: b7561f862364714ec1ca0da992f0e352c1c22cd7
ms.sourcegitcommit: c1ba5a62f30ac0a3acb337fb77431de6493e6096
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2020
ms.locfileid: "70737485"
---
下面是对复制到存储帐户的数据的大小限制。 请确保上传的数据符合这些限制。 有关这些限制的最新信息，请转到 [Azure blob 存储规模目标](/storage/common/storage-scalability-targets#azure-blob-storage-scale-targets)和 [Azure 文件规模目标](/storage/common/storage-scalability-targets#azure-files-scale-targets)。

| 复制到 Azure 存储帐户的数据的大小                      | 默认限制          |
|---------------------------------------------------------------------|------------------------|
| 块 Blob 和页 blob                                            | 每个存储帐户 500 TiB。 <br> 这包括来自所有源（包括 Data Box）的数据。|
| Azure 文件                                                          | 每个共享 5 TB。<br> StorageAccount_AzureFiles 下的所有文件夹都须遵循此限制  。       |
