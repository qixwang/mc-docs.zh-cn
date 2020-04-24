---
title: include 文件
description: include 文件
services: virtual-machines-windows, virtual-machines-linux
author: rockboyfor
ms.service: multiple
ms.topic: include
origin.date: 06/11/2019
ms.date: 02/10/2020
ms.author: v-yeche
ms.custom: include file
ms.openlocfilehash: 8604eda0c0cc5731495e68ad703a144662b70c7f
ms.sourcegitcommit: c1ba5a62f30ac0a3acb337fb77431de6493e6096
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2020
ms.locfileid: "77428374"
---
GPU 优化 VM 大小是具有单个或多个 NVIDIA GPU 的专用虚拟机。 这些大小是针对计算密集型、图形密集型和可视化工作负荷设计的。 本文介绍有关 GPU、vCPU、数据磁盘和 NIC 的数量和类型的信息。 此分组中的每个大小还包括存储吞吐量及网络带宽。

* **NCv3** 大小针对计算密集型和网络密集型应用程序和算法进行了优化。 一些示例包括基于 CUDA 和 OpenCL 的应用程序和模拟、AI 和深度学习。 NCv3 系列专用于采用 NVIDIA Tesla V100 GPU 的高性能计算工作负载。 NCv3 系列 VM 使用 Intel Xeon E5-2690 v4 (Broadwell) 处理器。

<!-- Not Available on NC, NCv2   -->
<!-- Not Available on ND, and NDv2   -->
<!-- Not Available on NV and NVv3   -->
<!-- Not Available on NVv4   -->
<!-- Not Available on ## NC-series-->
<!-- Not Available on ## NCv2-series-->

## <a name="ncv3-series"></a>NCv3 系列

高级存储：支持

高级存储缓存：支持

NCv3 系列 VM 采用 [NVIDIA Tesla V100](https://www.nvidia.com/data-center/tesla-v100/) GPU。 这些 GPU 可提供 NCv2 系列的 1.5 倍计算性能。 客户可将这些更新的 GPU 用于传统的 HPC 工作负荷，例如油藏模拟、DNA 测序、蛋白质分析、Monte Carlo 模拟和其他工作负荷。 NC24rs v3 配置提供了针对紧密耦合的并行计算工作负荷优化的低延迟、高吞吐量网络接口。 除了 GPU 之外，NCv3 系列 VM 还采用 Intel Xeon E5-2690 v4 (Broadwell) CPU。

> [!IMPORTANT]
> 对于此大小系列，订阅中的 vCPU（核心）配额在每个区域中最初都设置为 0。 可以请求在某个[可用区域](https://status.azure.com/status/)中[提高此系列的 vCPU 配额](https://support.azure.cn/support/support-azure)。
>

<!--Notice: URL redirect ../articles/azure-supportability/resource-manager-core-quotas-request.md to https://support.azure.cn/support/support-azure -->

| 大小 | vCPU | 内存:GiB | 临时存储 (SSD)：GiB | GPU | GPU 内存：GiB | 最大数据磁盘数 | 最大非缓存磁盘吞吐量：IOPS/Mbps | 最大 NIC 数 |
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
| Standard_NC6s_v3 | 6 |112 | 736 | 1 | 16 | 12 | 20000 / 200 | 4 |
| Standard_NC12s_v3 | 12 |224 | 1474 | 2 | 32 | 24 | 40000 / 400 | 8 |
| Standard_NC24s_v3 | 24 |448 | 2948 | 4 | 64 | 32 | 80000 / 800 | 8 | 
| Standard_NC24rs_v3* |24 |448 | 2948 | 4 | 64 | 32 | 80000 / 800 | 8 |

1 GPU = 一个 V100 卡。

*支持 RDMA

<!-- Not Available on ## Updated NDv2-series (Preview)-->
<!-- Not Available on ## ND-series-->
<!-- Not Avaiable on ## NV-series-->
<!-- Not Avaiable on ## NVv3-series-->
<!-- Not Avaiable on ## NVv4-series-->
<!-- Update_Description: update meta properties, wording update -->