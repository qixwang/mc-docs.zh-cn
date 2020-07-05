---
title: 并置 Windows Azure VM
description: 了解并置 Azure VM 资源如何改善延迟。
author: rockboyfor
ms.service: virtual-machines
ms.topic: article
ms.workload: infrastructure-services
origin.date: 10/30/2019
ms.date: 07/06/2020
ms.author: v-yeche
ms.openlocfilehash: c00039acc7afc0a3134850551167e025a143e643
ms.sourcegitcommit: 89118b7c897e2d731b87e25641dc0c1bf32acbde
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/03/2020
ms.locfileid: "85946058"
---
<!--Verified successfully-->
# <a name="co-locate-resource-for-improved-latency"></a>并置资源以改善延迟

在 Azure 中部署应用程序时，跨区域或可用性区域分布实例会造成网络延迟，这可能会影响应用程序的总体性能。 

## <a name="proximity-placement-groups"></a>邻近放置组 

[!INCLUDE [virtual-machines-common-ppg-overview](../../../includes/virtual-machines-common-ppg-overview.md)]

## <a name="next-steps"></a>后续步骤

使用 Azure PowerShell 将 VM 部署到[邻近放置组](proximity-placement-groups.md)。

了解如何[测试网络延迟](https://docs.azure.cn/virtual-network/virtual-network-test-latency?toc=%2fvirtual-machines%2fwindows%2ftoc.json)。

了解如何[优化网络吞吐量](/virtual-network/virtual-network-optimize-network-bandwidth?toc=%2fvirtual-machines%2fwindows%2ftoc.json)。  

<!--Not Available on [use proximity placement groups with SAP applications](/virtual-machines/workloads/sap/sap-proximity-placement-scenarios?toc=%2fvirtual-machines%2fwindows%2ftoc.json)-->

<!-- Update_Description: update meta properties, wording update, update link -->