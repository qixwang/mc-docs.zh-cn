---
title: Azure VM 大小 - 常规用途 | Azure
description: 列出了 Azure 中虚拟机可用的不同常规用途大小。 列出了有关此系列中各大小的 vCPU 数、数据磁盘数和 NIC 数以及存储吞吐量和网络带宽的信息。
services: virtual-machines
documentationcenter: ''
author: rockboyfor
manager: digimobile
editor: ''
tags: azure-resource-manager,azure-service-management
ms.assetid: ''
ms.service: virtual-machines
ms.devlang: na
ms.topic: article
ms.workload: infrastructure-services
origin.date: 02/20/2020
ms.date: 03/09/2020
ms.author: v-yeche
ms.openlocfilehash: a048c3092a3de752744c41b5e81d027790cdb6af
ms.sourcegitcommit: c1ba5a62f30ac0a3acb337fb77431de6493e6096
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2020
ms.locfileid: "79084515"
---
<!--Verified successfully-->
<!--Partical Content from verified-->
# <a name="general-purpose-virtual-machine-sizes"></a>常规用途虚拟机大小

常规用途 VM 大小提供均衡的 CPU 与内存之比。 适用于测试和开发、小到中型数据库和低到中等流量 Web 服务器。 本文提供了有关常规用途计算产品/服务的信息。

- [Av2 系列](av2-series.md) VM 可以部署在各种不同的硬件类型和处理器上。 A 系列 VM 的 CPU 性能和内存配置非常适合部署和测试等入门级工作负荷。 根据硬件限制大小，为运行中的实例提供一致的处理器性能，不论硬件部署的位置。 若要判断此大小部署所在的物理硬件，请从虚拟机中查询虚拟硬件。 示例用例包括开发和测试服务器、低流量 Web 服务器、中小型数据库、概念证明和代码存储库。

- [B 系列可突增](sizes-b-series-burstable.md) VM 非常适合于并非持续需要 CPU 完全性能的工作负荷，例如 Web 服务器、小型数据库以及开发和测试环境。 这些工作负荷通常具有可突增的性能要求。 B 系列使这些客户能够购买具有高性价比基线性能的 VM 大小，允许 VM 实例在 VM 使用的性能小于其基线性能时积累积分。 如果 VM 已累积了积分，则 VM 可以在应用程序需要更高的 CPU 性能时突增到 VM 的基线之上，使用最多达到 100% 的 vCPU。

    <!--Not Available on [Dav4 and Dasv4-series](dav4-dasv4-series.md)-->
    <!--Not Available on [DCv2-series](dcv2-series.md)-->

- [Dv2 和 Dsv2 系列](dv2-dsv2-series.md) VM 是原 D 系列的后续产品，具有更强大的 CPU 和最优 CPU 到内存配置，使其适合于大多数生产工作负荷。 Dv2 系列比 D 系列快大约 35%。 Dv2 系列在 Intel® Xeon® 8171M 2.1GHz (Skylake)、Intel® Xeon® E5-2673 v4 2.3 GHz (Broadwell) 或 Intel® Xeon® E5-2673 v3 2.4 GHz (Haswell) 处理器上运行，并采用 Intel Turbo Boost Technology 2.0。 Dv2 系列的内存和磁盘配置与 D 系列相同。

- [Dv3 和 Dsv3 系列](dv3-dsv3-series.md) VM 在采用超线程配置的 Intel® Xeon® 8171M 2.1GHz (Skylake)、Intel® Xeon® E5-2673 v4 2.3 GHz (Broadwell) 或 Intel® Xeon® E5-2673 v3 2.4 GHz (Haswell) 处理器上运行，为大多数常规用途工作负荷提供更好的价值定位。 在磁盘和网络限制已基于核心进行了调整以适应超线程技术的同时，内存已扩展（从 ~3.5 GiB/vCPU 到 4 GiB/vCPU）。 Dv3 系列不再具有 D/Dv2 系列的高内存 VM 大小，那些已移至内存优化的 [Ev3 和 Esv3 系列](ev3-esv3-series.md)。

示例 D 系列用例包括企业级应用程序、关系数据库、内存中缓存和分析。

## <a name="other-sizes"></a>其他大小

- [计算优化](sizes-compute.md)
- [内存优化](sizes-memory.md)
    
    <!--Not Available on - [Storage optimized](sizes-storage.md)-->
    
- [GPU 优化](sizes-gpu.md)

    <!--Not Available on - [High performance compute](sizes-hpc.md)-->
    
- [前几代](sizes-previous-gen.md)

## <a name="next-steps"></a>后续步骤

了解有关 [Azure 计算单元 (ACU)](acu.md) 如何帮助你跨 Azure SKU 比较计算性能的详细信息。

<!-- Update_Description: new article about sizes general -->
<!--NEW.date: 03/09/2020-->