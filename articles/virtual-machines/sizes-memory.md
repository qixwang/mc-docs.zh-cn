---
title: Azure VM 大小 - 内存 | Azure
description: 列出了 Azure 中虚拟机可用的不同内存优化大小。 针对此系列中的大小列出了 vCPU、数据磁盘和 NIC 的数量，以及存储吞吐量和网络带宽。
author: rockboyfor
keywords: VM 隔离,隔离的 VM,隔离,隔离的
ms.service: virtual-machines
ms.subservice: sizes
ms.topic: article
ms.workload: infrastructure-services
origin.date: 02/03/2020
ms.date: 07/06/2020
ms.author: v-yeche
ms.openlocfilehash: fd97293a68fe842f6c2affacdba511703292cbf7
ms.sourcegitcommit: 89118b7c897e2d731b87e25641dc0c1bf32acbde
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/03/2020
ms.locfileid: "85946040"
---
# <a name="memory-optimized-virtual-machine-sizes"></a>内存优化虚拟机大小

内存优化 VM 大小提供适用于关系数据库服务器、中到大型规模的缓存和内存中分析的高内存 CPU 比率。 本文介绍了此分组中各个大小的 vCPU 数、数据磁盘数、NIC 数、存储吞吐量及网络带宽的相关信息。

- [Dv2 和 DSv2 系列](dv2-dsv2-series-memory.md)是原 D 系列的后续产品，具有更强大的 CPU。 Dv2 系列比 D 系列快大约 35%。 它在第 2 代英特尔® 至强® 铂金 8272CL (Cascade Lake) 或英特尔&reg; 至强&reg; 8171M 2.1 GHz (Skylake) 或 英特尔&reg; 至强&reg; E5-2673 v4 2.3 GHz (Broadwell) 或 英特尔&reg; 至强&reg; E5-2673 v3 2.4 GHz (Haswell) 处理器上运行，并采用英特尔睿频加速技术 2.0。 Dv2 系列的内存和磁盘配置与 D 系列相同。

    Dv2 和 DSv2 系列非常适合于需要更快 vCPU、更佳临时存储性能或更高内存的应用程序。 它们为许多企业级应用程序提供强大的组合。

    <!--Not Available on [Eav4 and Easv4-series](eav4-easv4-series.md)-->
    
- [Ev3 和 Esv3 系列](ev3-esv3-series.md)在具有采用超线程配置的第 2 代英特尔® 至强® 铂金 8272CL (Cascade Lake) 或英特尔&reg; 至强&reg; 8171M 2.1 GHz (Skylake) 或英特尔&reg; 至强&reg; E5-2673 v4 2.3 GHz (Broadwell) 处理器上运行，为大多数常规用途工作负载提供更好的价值定位，并使 Ev3 满足大多数其他云的常规用途 VM 的要求。 内存已扩展（从 7 GiB/vCPU 扩展到 8 GiB/vCPU），而磁盘和网络限制已基于每个核心进行了调整，以适应向超线程的转变。 Ev3 是 D/Dv2 系列的高内存 VM 大小产品的后继产品。

    <!--Pending on  [Edv4 and Edsv4-series](edv4-edsv4-series.md)-->

- [M 系列](m-series.md)提供高 vCPU 计数（最多 128 个vCPU）和大量内存（最高 3.8 TiB）。 它也非常适用于极大型数据库或受益于高 vCPU 计数和大量内存的其他应用程序。

    <!--Not Available on [Mv2-series](mv2-series.md)-->
    
Azure 计算提供独立于特定硬件类型并专用于单个客户的虚拟机大小。 这些虚拟机大小非常适合于与其他客户的工作负载（涉及符合性和法规要求等元素）高度隔离的工作负载。 客户还可以选择利用[对嵌套虚拟机的 Azure 支持](https://azure.microsoft.com/blog/nested-virtualization-in-azure/)，对这些独立的虚拟机资源进一步细分。 请参阅下面的虚拟机系列页面，了解独立 VM 选项。

## <a name="other-sizes"></a>其他大小

- [常规用途](sizes-general.md)
- [计算优化](sizes-compute.md)
    
    <!--Not Available on - [Storage optimized](sizes-storage.md)-->
    
- [GPU 优化](sizes-gpu.md)
    
    <!--Not Available on - [High performance compute](sizes-hpc.md)-->
    
- [前几代](sizes-previous-gen.md)

## <a name="next-steps"></a>后续步骤

了解有关 [Azure 计算单元 (ACU)](acu.md) 如何帮助跨 Azure SKU 比较计算性能的详细信息。

<!-- Update_Description: update meta properties, wording update, update link -->