---
title: 适用于虚拟机的 Azure 专用主机概述
description: 详细了解如何使用 Azure 专用主机部署虚拟机。
services: virtual-machines-windows
author: rockboyfor
manager: digimobile
ms.service: virtual-machines-windows
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
origin.date: 01/09/2020
ms.date: 04/27/2020
ms.author: v-yeche
ms.openlocfilehash: ea1b8100ec5233ffc8ea948b2556a8697735530f
ms.sourcegitcommit: 275203d0f144aa809792f2c570fd64797f405c80
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/24/2020
ms.locfileid: "82125798"
---
<!--Verified successfully-->
# <a name="azure-dedicated-hosts"></a>Azure 专用主机

<!--Not Avaialble on availability zone-->

Azure 专用主机是一种提供物理服务器（能够托管一个或多个虚拟机）的服务，专用于一个 Azure 订阅。 专用主机是数据中心中使用的相同物理服务器，作为资源提供。 你可以在区域和容错域中预配专用主机。 然后，可以按照最能满足你的需要的配置将 VM 直接放入预配的主机中。

[!INCLUDE [virtual-machines-common-dedicated-hosts](../../../includes/virtual-machines-common-dedicated-hosts.md)]

## <a name="next-steps"></a>后续步骤

- 你可以使用 [Azure PowerShell](dedicated-hosts-powershell.md) 和 [Azure CLI](../linux/dedicated-hosts-cli.md) 部署专用主机。

    <!--Not Available on [portal](dedicated-hosts-portal.md)-->

- [此处](https://github.com/Azure/azure-quickstart-templates/blob/master/201-vm-dedicated-hosts/README.md)有一个示例模板，该模板使用区域和容错域来最大限度地提高某个地区的复原能力。

<!--Not Available on [Reserved Instance of Azure Dedicated Hosts](../prepay-dedicated-hosts-reserved-instances.md)-->

<!-- Update_Description: new article about dedicated hosts -->
<!--NEW.date: 04/27/2020-->