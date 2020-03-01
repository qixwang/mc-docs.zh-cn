---
title: include 文件
description: include 文件
services: virtual-machines
author: rockboyfor
ms.service: virtual-machines
ms.topic: include
origin.date: 11/04/2019
ms.date: 02/10/2020
ms.author: v-yeche
ms.custom: include file
ms.openlocfilehash: a2460b32e0dec931b5117c56df1873236f64b85f
ms.sourcegitcommit: ada94ca4685855f58616e4bf1dd5ca757878dfdc
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/18/2020
ms.locfileid: "77428376"
---
<!-- F-series, Fs-series* -->

计算优化 VM 大小具有较高的 CPU 与内存之比。 这些大小适用于中等流量的 Web 服务器、网络设备、批处理和应用程序服务器。 本文提供了有关 vCPU、数据磁盘和 NIC 的数量的信息。 它还介绍了此分组中每个大小的存储吞吐量和网络带宽。

Fsv2 系列基于 Intel® Xeon® Platinum 8168 处理器。 它具有稳定的 3.4 GHz 的全核 Turbo 时钟速度和最大为 3.7 GHz 的单核 Turbo 频率。 Intel 可扩展处理器上提供了全新的 Intel® AVX-512 指令。 对于单精度和双精度浮点运算，这些指令可为向量处理工作负荷提供高达 2 倍的性能提升。 换而言之，对于任何计算工作负荷，它们的处理速度相当快。

凭借较低的每小时定价，Fsv2 系列在基于每个 vCPU 的 Azure 计算单位 (ACU) 的 Azure 产品组合中具有最高性价比。

## <a name="fsv2-series-sup1sup"></a>Fsv2 系列 <sup>1</sup>

ACU：195 - 210

高级存储：支持

高级存储缓存：支持

| 大小             | vCPU | 内存:GiB | 临时存储 (SSD) GiB | 最大数据磁盘数 | 最大缓存吞吐量和临时存储吞吐量：IOPS/Mbps（以 GiB 为单位的缓存大小） | 最大非缓存磁盘吞吐量：IOPS/Mbps | 最大 NIC 数/预期网络带宽 (Mbps) |
|------------------|--------|-------------|----------------|----------------|--------------------------|--------------------------|-------------------------|
| Standard_F2s_v2  | 2      | 4           | 16             | 4              | 4000 / 31 (32)           | 3200 / 47                | 2 / 875                 |
| Standard_F4s_v2  | 4      | 8           | 32             | 8              | 8000 / 63 (64)           | 6400 / 95                | 2 / 1750               |
| Standard_F8s_v2  | 8      | 16          | 64             | 16             | 16000 / 127 (128)        | 12800 / 190              | 4 / 3500               |
| Standard_F16s_v2 | 16     | 32          | 128            | 32             | 32000 / 255 (256)        | 25600 / 380              | 4 / 7000               |
| Standard_F32s_v2 | 32     | 64          | 256            | 32             | 64000 / 512 (512)        | 51200 / 750              | 8 / 14000              |
| Standard_F48s_v2 | 48     | 96          | 384            | 32             | 96000/768 (768)        | 76800/1100             | 8/21000              |
| Standard_F64s_v2 | 64     | 128         | 512            | 32             | 128000 / 1024 (1024)     | 80000 / 1100             | 8 / 28000              |
| Standard_F72s_v2<sup>2，&nbsp;3</sup> | 72 | 144 | 576         | 32             | 144000 / 1152 (1520)     | 80000 / 1100             | 8 / 30000              |

<sup>1</sup> Fsv2 系列 VM 采用了 Intel® 超线程技术。

<sup>2</sup> 使用超过 64 个 vCPU 需要以下受支持的来宾操作系统之一：
- Windows Server 2016 或更高版本
- Ubuntu 16.04 LTS 或更高版本，带 Azure 优化内核（4.15 内核或更高版本）
- SLES 12 SP2 或更高版本
- CentOS 版本 6.7 到 6.10，安装了 Microsoft 提供的 LIS 程序包 4.3.1（或更高版本）
- CentOS 版本 7.3，安装了 Microsoft 提供的 LIS 程序包 4.2.1（或更高版本）
- CentOS 版本 7.6 或更高版本
    
    <!--Not Avaialble on - Oracle Linux with UEK4 or later-->
    
- Debian 9，带有向后移植内核 Debian 10 或更高版本
- 带有 4.14 内核或更高版本的 CoreOS

<sup>3</sup> 实例与专用于单个客户的硬件隔离。

<!-- Update_Description: update meta properties, wording update, update link -->