---
title: Azure 中 Linux 虚拟机的大小
description: 列出 Azure 中 Linux 虚拟机的不同可用大小。
services: virtual-machines-linux
documentationcenter: ''
author: rockboyfor
manager: digimobile
editor: ''
tags: azure-resource-manager,azure-service-management
ms.assetid: da681171-f045-4c80-a5a9-d8bd47964673
ms.service: virtual-machines-linux
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
origin.date: 11/06/2019
ms.date: 02/10/2020
ms.author: v-yeche
ms.openlocfilehash: a49f52729aa6fdb108042b77abea3ff0c5e996ef
ms.sourcegitcommit: c1ba5a62f30ac0a3acb337fb77431de6493e6096
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2020
ms.locfileid: "77428356"
---
# <a name="sizes-for-linux-virtual-machines-in-azure"></a>Azure 中 Linux 虚拟机的大小
本文介绍可用于运行 Linux 应用和工作负荷的 Azure 虚拟机的可用大小与选项。 此外，还提供了你在计划使用这些资源时要考虑的部署注意事项。 本文也适用于 [Windows 虚拟机](../windows/sizes.md?toc=%2fvirtual-machines%2fwindows%2ftoc.json)。

| 类型                     | 大小           |    说明       |
|--------------------------|-------------------|------------------------------------------------------------------------------------------------------------------------------------|
| [常规用途](sizes-general.md)          | B、Dsv3、Dv3、DSv2、Dv2、Av2  | CPU 与内存之比平衡。 适用于测试和开发、小到中型数据库和低到中等流量 Web 服务器。 |
| [计算优化](sizes-compute.md)        | Fsv2           | 高 CPU 与内存之比。 适用于中等流量的 Web 服务器、网络设备、批处理和应用程序服务器。        |
| [内存优化](sizes-memory.md)         | Esv3、Ev3、M、DSv2、Dv2  | 高内存与 CPU 之比。 适用于关系数据库服务器、中到大型规模的缓存和内存中分析。                 |
| [GPU](sizes-gpu.md)            |     NCv3           | 针对大量图形绘制和视频编辑的专用虚拟机，以及带有深度学习功能的模型定型和推断 (ND)。 可选择单个或多个 GPU。       |

<!--Not Available [General purpose](sizes-general.md) Dasv4, Dav4, DC-->
<!--Not Available [Memory optimized](sizes-memory.md) Easv4, Eav4, Mv2-->
<!--Not Available [Storage optimized](sizes-storage.md) Lsv2-->
<!--Not Available NV, NVv2, NC, NCv2, ND, NDv2 (Preview)-->
<!--Not Available [High performance compute](sizes-hpc.md) HB, HC, H -->

<br />

- 有关不同大小的定价信息，请参阅[虚拟机定价](https://www.azure.cn/pricing/details/virtual-machines/)。 
- 有关各 Azure 区域中推出的 VM 大小，请查看[可用产品(按区域)](https://status.azure.com/status/)。
- 若要查看 Azure VM 的一般限制，请参阅 [Azure 订阅和服务限制、配额与约束](../../azure-resource-manager/management/azure-subscription-service-limits.md)。
- 了解有关 [Azure 计算单元 (ACU)](acu.md) 如何帮助你跨 Azure SKU 比较计算性能的详细信息。

## <a name="rest-api"></a>REST API

若要了解如何使用 REST API 查询 VM 大小，请参阅以下文章：

- [List available virtual machine sizes for resizing](https://docs.microsoft.com/rest/api/compute/virtualmachines/listavailablesizes)（列出可用虚拟机大小以重设大小）
- [List available virtual machine sizes for a subscription](https://docs.microsoft.com/rest/api/compute/resourceskus/list)（列出某个订阅的可用虚拟机大小）
- [List available virtual machine sizes in an availability set](https://docs.microsoft.com/rest/api/compute/availabilitysets/listavailablesizes)（列出可用性集中的可用虚拟机大小）

## <a name="acu"></a>ACU

了解有关 [Azure 计算单元 (ACU)](acu.md) 如何帮助你跨 Azure SKU 比较计算性能的详细信息。

## <a name="benchmark-scores"></a>基准评分

使用 [CoreMark 基准测试分数](compute-benchmark-scores.md)，详细了解 Linux VM 的计算性能。

## <a name="next-steps"></a>后续步骤

了解关于可用的各种 VM 大小的详细信息：
- [常规用途](sizes-general.md)
- [计算优化](sizes-compute.md)
- [内存优化](sizes-memory.md)

    <!--Not Available- [Storage optimized](sizes-storage.md) -->
    
- [GPU](sizes-gpu.md)

    <!--Not Available- [High performance compute](sizes-hpc.md)-->
    
- 查看[上一代](sizes-previous-gen.md)页，了解 A Basic 和 Standard、D/DSv1（D1-4 和 D11-14 v1）以及 F/Fsv1 系列。

<!--Not Available on A8-A11, G/GS, Lsv1, and NVv2 series-->
<!--Update_Description: update meta properties, wording update, update link -->
