---
author: rockboyfor
ms.service: virtual-machines
ms.topic: include
origin.date: 03/18/2019
ms.date: 07/27/2020
ms.author: v-yeche
ms.openlocfilehash: cbf1034a96740c57365b16b2805c8efeeabc4123
ms.sourcegitcommit: 2b78a930265d5f0335a55f5d857643d265a0f3ba
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/28/2020
ms.locfileid: "87260200"
---
可以将多个数据磁盘附加到 Azure 虚拟机。 根据 VM 数据磁盘的可伸缩性和性能目标，可以确定满足性能和容量要求所需的磁盘数量和类型。

> [!IMPORTANT]
> 为了获得最佳性能，需要限制附加到虚拟机的、重度使用的磁盘数，以避免可能的性能限制。 如果附加的所有磁盘未在同时重度使用，则虚拟机可以支持更多的磁盘。

**对于 Azure 托管磁盘：**

下表说明了每个订阅的每个区域的资源数的默认限制和最大限制。 对每个资源组的托管磁盘、快照和映像的数量没有限制。  

> | 资源 | 限制 |
> | --- | --- |
> | 标准托管磁盘 | 50,000 |
> | 标准 SSD 托管磁盘 | 50,000 |
> | 高级托管磁盘 | 50,000 |
> | 标准 LRS 快照 | 50,000 |
> | 托管映像 | 50,000 |

<!--Not Available on > | Standard_ZRS snapshots | 50,000 |-->

* **对于标准存储帐户：** 标准存储帐户的总请求率上限为 20,000 IOPS。 在标准存储帐户中，所有虚拟机磁盘的 IOPS 总数不应超过此限制。
  
    可以根据请求率的限制，大致计算单个标准存储帐户可支持的重度使用磁盘数。 例如，对于基本层 VM，高使用率的磁盘数上限约为 66，即每个磁盘 20,000/300 IOPS。 对于标准层 VM，高使用率的磁盘数上限约为 40，即每个磁盘 20,000/500 IOPS。 

* **对于高级存储帐户：** 高级存储帐户的总吞吐量速率上限为 50 Gbps。 所有 VM 磁盘的总吞吐量不应超过此限制。

<!-- Update_Description: update meta properties, wording update, update link -->