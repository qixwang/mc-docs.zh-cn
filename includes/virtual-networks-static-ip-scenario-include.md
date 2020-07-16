---
author: rockboyfor
ms.service: virtual-network
ms.topic: include
origin.date: 02/14/2020
ms.date: 07/06/2020
ms.author: v-yeche
ms.openlocfilehash: 0e864de64d3944d4bc579bd328e1ed99075e2ea7
ms.sourcegitcommit: af71b9199d47fb81e85d70da0cfb265cc814a644
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/06/2020
ms.locfileid: "85969027"
---
## <a name="scenario"></a>方案

为了更好地说明如何为 VM 配置静态 IP 地址，本文档使用这一方案：

![虚拟网络方案：前端和后端子网，前端子网具有静态 IP 地址](./media/virtual-networks-static-ip-scenario-include/static-ip-scenario.png)

在此方案中，在 FrontEnd 子网中创建一个名为 DNS01 的 VM，然后将它设置为使用静态 IP 地址 192.168.1.101  。
