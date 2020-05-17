---
title: Azure 中 Windows 虚拟机的大小
description: 列出 Azure 中 Windows 虚拟机的不同可用大小。
services: virtual-machines-windows
documentationcenter: ''
author: rockboyfor
manager: digimobile
editor: ''
tags: azure-resource-manager,azure-service-management
ms.assetid: aabf0d30-04eb-4d34-b44a-69f8bfb84f22
ms.service: virtual-machines-windows
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
origin.date: 02/03/2020
ms.date: 04/27/2020
ms.author: v-yeche
ms.openlocfilehash: cb654752878268da2f4dd9103deb3695061c0f53
ms.sourcegitcommit: 2d8950c6c255361eb6c66406988e25c69cf4e0f5
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/14/2020
ms.locfileid: "83392424"
---
# <a name="sizes-for-windows-virtual-machines-in-azure"></a>Azure 中 Windows 虚拟机的大小

本文介绍可用于运行 Windows 应用和工作负荷的 Azure 虚拟机的可用大小和选项。 此外，还提供在计划使用这些资源时要考虑的部署注意事项。  本文也适用于 [Linux 虚拟机](../linux/sizes.md)。

| 类型 | 大小 | 说明 |
|------|-------|-------------|
| [常规用途](../sizes-general.md) | B、Dsv3、Dv3、DSv2、Dv2、Av2 | CPU 与内存之比平衡。 适用于测试和开发、小到中型数据库和低到中等流量 Web 服务器。 |
| [计算优化](../sizes-compute.md) | Fsv2 | 高 CPU 与内存之比。 适用于中等流量的 Web 服务器、网络设备、批处理和应用程序服务器。 |
| [内存优化](../sizes-memory.md) | Esv3、Ev3、M、DSv2、Dv2  | 高内存与 CPU 之比。 适用于关系数据库服务器、中到大型规模的缓存和内存中分析。 |
| [GPU](../sizes-gpu.md) | NCv3           | 针对大量图形绘制和视频编辑的专用虚拟机，以及带有深度学习功能的模型定型和推断 (ND)。 可选择单个或多个 GPU。 |

<!-- Not Available  Dasv4, Dav4, DC series -->
<!-- Not Available  Easv4, Eav4, Mv2, -->
<!-- Not Available [Storage optimized](../virtual-machines-windows-sizes-storage.md)        | Lsv2 -->
<!-- Not Available [GPU](sizes-gpu.md)            | NC, NVv2, ND, NDv2 (Preview), NV, NVv3, NVv4 (Preview)   -->
<!-- Not Available [High performance compute](sizes-hpc.md) | HB, HC,  H-->


- 有关不同大小的定价信息，请参阅[虚拟机定价](https://www.azure.cn/pricing/details/virtual-machines/)。
- 若要查看 Azure VM 的一般限制，请参阅 [Azure 订阅和服务限制、配额与约束](../../azure-resource-manager/management/azure-subscription-service-limits.md)。
- 存储成本根据存储帐户中的已使用页数进行单独计算。 有关详细信息，请参阅 [Azure 存储定价](https://www.azure.cn/pricing/details/storage/)。
- 了解有关 [Azure 计算单元 (ACU)](../acu.md) 如何帮助跨 Azure SKU 比较计算性能的详细信息。

## <a name="rest-api"></a>REST API

有关使用 REST API 来查询 VM 大小的信息，请参阅以下文章：

- [List available virtual machine sizes for resizing](https://docs.microsoft.com/rest/api/compute/virtualmachines/listavailablesizes)（列出可用的虚拟机大小以便调整大小）
- [List available virtual machine sizes for a subscription](https://docs.microsoft.com/rest/api/compute/resourceskus/list)（列出订阅的可用虚拟机大小）
- [List available virtual machine sizes in an availability set](https://docs.microsoft.com/rest/api/compute/availabilitysets/listavailablesizes)（列出可用性集中的可用虚拟机大小）

## <a name="acu"></a>ACU

了解有关 [Azure 计算单元 (ACU)](../acu.md) 如何帮助跨 Azure SKU 比较计算性能的详细信息。

## <a name="benchmark-scores"></a>基准评分

使用 [CoreMark 基准测试分数](compute-benchmark-scores.md)，详细了解 Windows VM 的计算性能。

## <a name="next-steps"></a>后续步骤

了解关于可用的各种 VM 大小的详细信息：

- [常规用途](../sizes-general.md)
- [计算优化](../sizes-compute.md)
- [内存优化](../sizes-memory.md)
    
    <!--Not Avaialble on - [Storage optimized](../sizes-storage.md)-->
    
- [GPU 优化](../sizes-gpu.md)
    
    <!-- Not Available on - [High performance compute](../sizes-hpc.md)-->
    
- 查看[上一代](../sizes-previous-gen.md)页，了解 A Standard 和 Dv1（D1-4 和 D11-14 v1）系列

<!-- Not Available on A8-A11 series-->
<!-- Update_Description: update meta properties, wording update, update link -->