---
author: WenJason
ms.service: storage
ms.topic: include
origin.date: 11/08/2019
ms.date: 01/06/2020
ms.author: v-jay
ms.openlocfilehash: f209e5f9dfefbcc04c6e1872ca8099a7358f2785
ms.sourcegitcommit: 6a8bf63f55c925e0e735e830d67029743d2c7c0a
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/03/2020
ms.locfileid: "75624096"
---
| 资源 | 目标        |
|----------|---------------|
| 单个 Blob 容器的最大大小 | 与最大存储帐户容量相同 |
| 块 Blob 或附加 Blob 中的块数上限 | 50,000 块 |
| 块 Blob 中块的最大大小 | 100 MiB |
| 块 Blob 的最大大小 | 50,000 X 100 MiB（大约 4.75 TiB） |
| 附加 Blob 中块的最大大小 | 4 MiB |
| 附加 Blob 的最大大小 | 50,000 x 4 MiB（大约 195 GiB） |
| 页 Blob 的最大大小 | 8 TiB |
| 每个 Blob 容器存储的访问策略的最大数目 | 5 |
|单个 blob 的目标请求速率 | 每秒最多 500 个请求 |
|单个页 blob 的目标吞吐量 | 最高每秒 60 MiB |
|单个块 blob 的目标吞吐量 |上限为存储帐户的传入/传出限制<sup>1</sup> |

<sup>1</sup> 单个 blob 的吞吐量取决于多个因素，包括但不限于：并发性、请求大小、性能层、源上传速度和目标下载速度。 具体地说，对于标准存储帐户，请使用大于 4 MiB 的 Blob 或块大小调用 [Put Blob](https://docs.microsoft.com/rest/api/storageservices/put-blob) 或 [Put Block](https://docs.microsoft.com/rest/api/storageservices/put-block) 操作。
