---
author: rockboyfor
ms.service: virtual-network
ms.topic: include
origin.date: 02/14/2020
ms.date: 07/06/2020
ms.author: v-yeche
ms.openlocfilehash: fc28b99cbcedaba111ae19d1cc268d37d7fda093
ms.sourcegitcommit: af71b9199d47fb81e85d70da0cfb265cc814a644
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/06/2020
ms.locfileid: "85969026"
---
系统会从指定的范围自动为虚拟机 (VM) 分配专用 IP 地址，具体取决于 VM 部署时所在的子网。 VM 会保留地址，直到该 VM 被删除。 Azure 从你创建 VM 时所在的子网动态分配下一个可用的专用 IP 地址。 如果想要已分配给 VM 的子网中的特定 IP 地址，请分配静态 IP 地址。
