---
author: WenJason
ms.service: storage
ms.topic: include
origin.date: 06/20/2019
ms.date: 09/30/2019
ms.author: v-jay
ms.openlocfilehash: 4bb02123d501c76b2af9e749cde6d3c045ce8241
ms.sourcegitcommit: c1ba5a62f30ac0a3acb337fb77431de6493e6096
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2020
ms.locfileid: "71310553"
---
| 资源 | 目标 |
|----------|---------------|
| 单个表的最大大小 | 500 TiB |
| 表实体的最大大小 | 1 MiB |
| 表实体中属性的最大数目 | 255，包含三个系统属性：PartitionKey、RowKey 和 Timestamp |
| 实体中属性值的最大总大小 | 1 MiB |
| 实体中单个属性的最大总大小 | 因属性类型而异。 有关详细信息，请参阅[了解表服务数据模型](https://docs.microsoft.com/rest/api/storageservices/understanding-the-table-service-data-model)中的**属性类型**。 |
| 每个表存储的访问策略的最大数目 | 5 |
| 每个存储帐户的最大请求速率 | 20,000 事务/秒，假定实体大小为 1-KiB |
| 单个表分区的目标吞吐量（1 KiB 实体） | 每秒最多 2,000 个实体 |
