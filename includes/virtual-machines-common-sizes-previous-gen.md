---
title: include 文件
description: include 文件
services: virtual-machines-windows, virtual-machines-linux
author: rockboyfor
ms.service: multiple
ms.topic: include
origin.date: 05/16/2019
ms.date: 10/14/2019
ms.author: v-yeche
ms.custom: include file
ms.openlocfilehash: 963578116ae60bd6d8278baa59ce78774559cddf
ms.sourcegitcommit: c1ba5a62f30ac0a3acb337fb77431de6493e6096
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2020
ms.locfileid: "72272178"
---
本部分提供了有关先前几代虚拟机大小的信息。 这些大小仍可使用，但有新的大小可供使用。 

## <a name="f-series"></a>F 系列

F 系列基于 2.4 GHz Intel Xeon® E5-2673 v3 (Haswell) 处理器，该处理器使用 Intel Turbo Boost 技术 2.0，可实现高达 3.1 GHz 的时钟速度。 此 CPU 性能与 Dv2 系列的 VM 相同。  

对于需要更快的 CPU，但是每个 vCPU 不需要太多内存或临时存储的工作负荷来说，F 系列 VM 是一个极佳选择。  诸如分析、游戏服务器、Web 服务器和批处理等工作负荷从 F 系列的优势中获益。

ACU：210 - 250

高级存储：不支持

高级存储缓存：不支持

<!--MOONCAKE: CORRECT ON Max NICs / Expected network bandwidth (Mbps)-->

| 大小         | vCPU | 内存:GiB | 临时存储 (SSD) GiB | 最大临时存储吞吐量：IOPS/读取 MBps/写入 MBps | 最大的数据磁盘/吞吐量：IOPS | 最大 NIC 数/预期网络带宽 (Mbps) |
|--------------|-----------|-------------|----------------|----------------------------------------------------------|-----------------------------------|------------------------------|
| Standard_F1  | 1         | 2           | 16             | 3000/46/23                                           | 4/4x500                         | 2 / 750                 |
| Standard_F2  | 2         | 4           | 32             | 6000/93/46                                           | 8/8x500                         | 2 / 1500                     |
| Standard_F4  | 4         | 8           | 64             | 12000/187/93                                         | 16/16x500                         | 4 / 3000                     |
| Standard_F8  | 8         | 16          | 128            | 24000/375/187                                        | 32/32x500                       | 8 / 6000                     |
| Standard_F16 | 16        | 32          | 256            | 48000/750/375                                        | 64/64x500                       | 8 / 12000           |

## <a name="fs-series-sup1sup"></a>Fs 系列 <sup>1</sup>

Fs 系列具有 F 系列的所有优势（在高级存储的基础上）。

ACU：210 - 250

高级存储：支持

高级存储缓存：支持

<!--MOONCAKE: CORRECT ON Max NICs / Expected network bandwidth (Mbps)-->

| 大小 | vCPU | 内存:GiB | 临时存储 (SSD) GiB | 最大数据磁盘数 | 最大缓存吞吐量和临时存储吞吐量：IOPS/Mbps（以 GiB 为单位的缓存大小） | 最大非缓存磁盘吞吐量：IOPS/Mbps | 最大 NIC 数/预期网络带宽 (Mbps) |
| --- | --- | --- | --- | --- | --- | --- | --- |
| Standard_F1s |1 |2 |4 |4 |4000 / 32 (12) |3200 / 48 |2 / 750 |
| Standard_F2s |2 |4 |8 |8 |8000 / 64 (24) |6400 / 96 |2 / 1500 |
| Standard_F4s |4 |8 |16 |16 |16000 / 128 (48) |12800 / 192 |4 / 3000 |
| Standard_F8s |8 |16 |32 |32 |32000 / 256 (96) |25600 / 384 |8 / 6000 |
| Standard_F16s |16 |32 |64 |64 |64000 / 512 (192) |51200 / 768 |8 / 12000 |

Mbps = 每秒 10^6 字节，GiB = 1024^3 字节。

<sup>1</sup> Fs 系列 VM 可能的最大磁盘吞吐量（IOPS 或 Mbps）可能受限于附加磁盘的数量、大小和条带化。  有关详细信息，请参阅[为实现高性能而设计](../articles/virtual-machines/windows/premium-storage-performance.md)。  

<!--Not Available on ## NVv2-series (Preview)-->
<!-- Update_Description: wording update -->