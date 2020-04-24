---
author: rockboyfor
ms.service: virtual-network
ms.topic: include
origin.date: 11/09/2018
ms.date: 01/21/2019
ms.author: v-yeche
ms.openlocfilehash: 2aa29babfa71e8f401a66753a4402f13f3db8986
ms.sourcegitcommit: c1ba5a62f30ac0a3acb337fb77431de6493e6096
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2020
ms.locfileid: "63846459"
---
## <a name="scenario"></a>场景
为了更好地说明如何为 VM 配置静态 IP 地址，本文档将使用以下方案。

![VNet 方案](./media/virtual-networks-static-ip-scenario-include/static-ip-scenario.png)

在此方案中，会在 **FrontEnd** 子网中创建一个名为 **DNS01** 的 VM，并将它设置为使用静态 IP 地址 **192.168.1.101**。