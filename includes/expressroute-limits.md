---
ms.openlocfilehash: cb1f6429c32ed57aee62b38ed180ff591fb6e272
ms.sourcegitcommit: c1ba5a62f30ac0a3acb337fb77431de6493e6096
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2020
ms.locfileid: "63829291"
---
#### <a name="expressroute-limits"></a>ExpressRoute 限制

下列限制适用于每个订阅的 ExpressRoute 资源。

| 资源 | 默认限制/最大限制 |
| --- | --- |
| 每个订阅的 ExpressRoute 线路数 |10 |
| 每个订阅每个区域的 ExpressRoute 线路数（Azure 资源管理器） |10 |
| 具有 ExpressRoute Standard 的 Azure 私用对等互连的最大路由数 |4,000 |
| 具有 ExpressRoute Premium 附加设备的 Azure 私用对等互连的最大路由数 |10,000 |
| 具有 ExpressRoute Standard 的 Azure Microsoft 对等互连的最大路由数 |200 |
| 具有 ExpressRoute Premium 附加设备的 Azure Microsoft 对等互连的最大路由数 |200 |
| 链接到不同对等互连位置中相同虚拟网络的最大 ExpressRoute 线路数 |4 |
| 每个 ExpressRoute 线路允许的虚拟网络链接数 | 请参阅下表 |

#### <a name="number-of-virtual-networks-per-expressroute-circuit"></a>每个 ExpressRoute 线路的虚拟网络数

| **线路大小** | **标准版 VNet 链接数** | **高级版附加设备的 VNet 链接数** |
|---|---|---|
| 50 Mbps | 10 | 20 |
| 100 Mbps | 10 | 25 |
| 200 Mbps | 10 | 25 |
| 500 Mbps | 10 | 40 |
| 1 Gbps | 10 | 50 |
| 2 Gbps | 10 | 60 |
| 5 Gbps | 10 | 75 |
| 10 Gbps | 10 | 100 |