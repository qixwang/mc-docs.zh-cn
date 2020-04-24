---
title: include 文件
description: include 文件
services: virtual-machines
author: rockboyfor
ms.service: virtual-machines
ms.topic: include
origin.date: 05/28/2019
ms.date: 02/10/2020
ms.author: v-yeche
ms.custom: include file
ms.openlocfilehash: 781be9a7a3fc0be13af352d875d92f968aca350e
ms.sourcegitcommit: c1ba5a62f30ac0a3acb337fb77431de6493e6096
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2020
ms.locfileid: "77428429"
---
请务必了解 Azure 中虚拟机 (VM) 运行的方式和位置，以及最大化性能、可用性和冗余的选项。 本文提供了 Azure 的可用性和冗余功能的概述。

## <a name="what-are-azure-regions"></a>什么是 Azure 区域？
Azure 在世界各地的多个数据中心运行。 这些数据中心分组到地理区域，让用户可灵活选择构建应用程序的位置。 

可以在规定的地理区域（例如“中国东部”、“中国北部”或“中国东部 2”）中创建 Azure 资源。 可查看[区域及其位置的列表](https://status.azure.com/status/)。 为了提供冗余和可用性，每个区域都设有多个数据中心。 这样，便可灵活设计应用程序，创建距离用户最近的 VM，满足任何法律、符合性或税务要求。

<!--Not Available on ## Special Azure regions-->

## <a name="region-pairs"></a>区域对
每个 Azure 区域都与同一地域内的另一个区域配对。 此方法适用于跨地域复制资源（例如 VM 存储），降低因自然灾害、社会动乱、电力中断或物理网络中断而同时影响两个区域的可能性。 区域对的其他优点包括：

<!--Not Available on (such as US, Europe, or Asia)-->

* 出现范围较广的 Azure 区域中断时，每个区域对中有一个区域优先级更高，这样可以缩短应用程序的还原时间。 
* 将逐一对配对的区域进行计划内 Azure 更新，尽量减少停机时间并降低应用程序中断风险。
* 出于税务和执法管辖权方面的考虑，数据仍以配对的形式驻留在同一地域内。

    <!--Not Available on （巴西南部除外）-->
    <!--Not Available on (except for Brazil South)-->
    
区域对的示例包括：

| 主 | 辅助副本 |
|:--- |:--- |
| 中国北部  | 中国东部 |
| 中国北部 2 | 中国东部 2 |

<!--Not Avaiable on  [list of regional pairs here](../articles/best-practices-availability-paired-regions.md)-->
<!--Not Avaiable on #what-are-paired-regions-->

## <a name="feature-availability"></a>功能可用性
某些服务或 VM 功能（例如特定的 VM 大小或存储类型）仅在特定区域提供。 也有一些全球性 Azure 服务不需要用户选择特定的区域，例如 [Azure Active Directory](../articles/active-directory/fundamentals/active-directory-whatis.md)、[流量管理器](../articles/traffic-manager/traffic-manager-overview.md)或 [Azure DNS](../articles/dns/dns-overview.md)。 若要更好地设计应用程序环境，可查看 [Azure 服务在每个区域的可用性](https://status.azure.com/status/)。 此外，还可以[以编程方式查询受支持的 VM 大小和每个区域中的限制](../articles/azure-resource-manager/templates/error-sku-not-available.md)。

## <a name="storage-availability"></a>存储可用性
在考虑可用的存储复制选项时，了解 Azure 区域和地理位置变得相当重要。 根据存储类型，可以使用不同的复制选项。

**Azure 托管磁盘**
* 本地冗余存储 (LRS)
  * 在创建存储帐户时所在的区域复制数据三次。

**基于存储帐户的磁盘**
* 本地冗余存储 (LRS)
    * 在创建存储帐户时所在的区域复制数据三次。

    <!--Not Available on * Zone redundant storage (ZRS)-->
  
* 异地冗余存储 (GRS)
    * 将数据复制到距主要区域数百英里以外的次要区域。
* 读取访问异地冗余存储 (RA-GRS)
    * 与 GRS 一样，可将数据复制到次要区域，但此外还提供对次要位置中数据的只读访问权限。

下表提供存储复制类型之间差异的简要概述：

<!--Not Available on Zone redundant storage (ZRS)-->

| 复制策略 | LRS | GRS | RA-GRS |
|:--- |:--- |:--- |:--- |
| 数据在多个设施之间进行复制。 |否 |是 |是 |
| 可以从辅助位置和主位置读取数据。 |否 |否 |是 |
| 在单独的节点上维护的数据副本数。 |3 |6 |6 |

<!--Not Available on Zone redundant storage (ZRS)-->

可以在[此处](../articles/storage/common/storage-redundancy.md)详细了解 Azure 存储复制选项。 有关托管磁盘的详细信息，请参阅 [Azure 托管磁盘概述](../articles/virtual-machines/windows/managed-disks-overview.md)。

### <a name="storage-costs"></a>存储成本
价格根据所选存储类型和可用性的不同而异。

**Azure 托管磁盘**
* 高级托管磁盘受固态硬盘 (SSD) 支持，标准托管磁盘受常规旋转磁盘支持。 高级和标准托管磁盘均按预配的磁盘容量计费。

**非托管磁盘**
* 高级存储以固态硬盘 (SSD) 为基础，收费方式根据磁盘容量而定。
* 标准存储以普通的旋转式磁盘为基础，收费方式根据所用容量和所需的存储可用性而定。
    * 对于 RA-GRS，有一个额外的异地复制数据传输费用，它是将该数据复制到另一个 Azure 区域收取的带宽费用。

有关不同存储类型和可用性选项的定价信息，请参阅 [Azure 存储器定价](https://www.azure.cn/pricing/details/storage/)。

<!-- Update_Description: update meta properties, wording update, update link -->