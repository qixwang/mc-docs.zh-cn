---
title: include 文件
description: include 文件
services: virtual-machines
author: rockboyfor
ms.service: virtual-machines
ms.topic: include
origin.date: 10/17/2019
ms.date: 11/11/2019
ms.author: v-yeche
ms.custom: include file
ms.openlocfilehash: f8e615895494a5447b9c6bcba2de2a6c72aa36f0
ms.sourcegitcommit: c1ba5a62f30ac0a3acb337fb77431de6493e6096
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2020
ms.locfileid: "73831466"
---
<!--CORRECT ON Max NICs / Expected network bandwidth (Mbps)-->
内存优化 VM 大小提供适用于关系数据库服务器、中到大型规模的缓存和内存中分析的高内存 CPU 比率。 本文介绍了此分组中各个大小的 vCPU 数、数据磁盘数、NIC 数、存储吞吐量及网络带宽的相关信息。

* Ev3 系列在超线程配置中采用 Intel® Xeon® 8171M 2.1 GHz (Skylake) 或 Intel® Xeon® E5-2673 v4 2.3 GHz (Broadwell) 处理器，针对最常规用途的工作负荷提供了更好的价值主张，因此 Ev3 适用于大多数其他云的常规用途 VM。  在磁盘和网络限制已基于核心进行了调整以适应超线程技术的同时，内存也得到了扩展（从 7 GiB/vCPU 到 8 GiB/vCPU）。  Ev3 是 D/Dv2 系列的高内存 VM 大小产品的后继产品。

    <!--Not Available on Eav3-->
    <!--Not Available on Mv2-Series -->
    
* M 系列提供高 vCPU 计数（最多 128 个vCPU）和大量内存（最高 3.8 TiB）。 它也非常适用于极大型数据库或受益于高 vCPU 计数和大量内存的其他应用程序。

* Dv2 系列和对应的 DSv2 系列是要求更快速 vCPU、更佳临时存储性能或具有更高内存需求的应用程序的最佳选择。  它们为许多企业级应用程序提供强大的组合。
    
    <!-- Not Available on G, GS series -->
    
* Dv2 系列是原 D 系列的后续系列，其特点是 CPU 功能更强大。 Dv2 系列比 D 系列快大约 35%。 它在 Intel® Xeon® 8171M 2.1 GHz (Skylake) 或 Intel® Xeon® E5-2673 v4 2.3 GHz (Broadwell) 或 Intel® Xeon® E5-2673 v3 2.4 GHz (Haswell) 处理器上运行，并采用 Intel Turbo Boost Technology 2.0。 Dv2 系列的内存和磁盘配置与 D 系列相同。


* Azure 计算提供独立于特定硬件类型并专用于单个客户的虚拟机大小。  这些虚拟机大小非常适合于与其他客户的工作负载（涉及符合性和法规要求等元素）高度隔离的工作负载。  客户还可以选择利用[对嵌套虚拟机的 Azure 支持](https://azure.microsoft.com/blog/nested-virtualization-in-azure/)，对这些独立的虚拟机资源进一步细分。  请参阅下面的虚拟机系列表，了解独立 VM 选项。

## <a name="esv3-series"></a>Esv3 系列

ACU：160-190 <sup>1</sup>

高级存储：支持

高级存储缓存：支持

ESv3 系列实例采用 Intel® Xeon® 8171M 2.1 GHz (Skylake) 或 Intel® Xeon® E5-2673 v4 2.3 GHz (Broadwell) 处理器，使用 Intel Turbo Boost Technology 2.0 可达到 3.5GHz 并使用高级存储。 Ev3 系列实例适用于内存密集型企业应用程序。

| 大小             | vCPU | 内存:GiB | 临时存储 (SSD) GiB | 最大数据磁盘数 | 最大缓存吞吐量和临时存储吞吐量：IOPS/Mbps（以 GiB 为单位的缓存大小） | 最大非缓存磁盘吞吐量：IOPS/Mbps | 最大 NIC 数/预期网络带宽 (Mbps) |
|------------------|--------|-------------|----------------|----------------|-----------------------------------------------------------------------|-------------------------------------------|------------------------------------------------|
| Standard_E2s_v3 | 2      | 16          | 32             | 4              | 4000 / 32 (50)                                                       | 3200 / 48                                | 2 / 1000                                   |
| Standard_E4s_v3&nbsp;<sup>2</sup> | 4      | 32          | 64             | 8              | 8000 / 64 (100)                                                      | 6400 / 96                                | 2 / 2000                                   |
| Standard_E8s_v3&nbsp;<sup>2</sup> | 8      | 64          | 128            | 16             | 16000 / 128 (200)                                                    | 12800 / 192                              | 4 / 4000                                       |
| Standard_E16s_v3&nbsp;<sup>2</sup> | 16     | 128         | 256            | 32             | 32000 / 256 (400)                                                    | 25600 / 384                              | 8 / 8000                                       |
| Standard_E20s_v3                   | 20 个     | 160         | 320            | 32             | 40000 / 320 (400)                                                    | 32000 / 480                              | 8/10000                                       |
| Standard_E32s_v3&nbsp;<sup>2</sup> | 32     | 256         | 512            | 32             | 64000 / 512 (800)                                                    | 51200 / 768                              | 8 / 16000                             |
| Standard_E48s_v3&nbsp;<sup>2</sup> | 48     | 384         | 768            | 32             | 96000/768 (1200)                                                   | 76800/1152                             | 8/24000                             |
| Standard_E64s_v3&nbsp;<sup>2</sup> | 64     | 432         | 864            | 32             | 128000 / 1024 (1600)                                                   | 80000 / 1200                             | 8 / 30000                             |
| Standard_E64is_v3&nbsp;<sup>3</sup> | 64     | 432         | 864            | 32             | 128000 / 1024 (1600)                                                   | 80000 / 1200                             | 8 / 30000                             |

<sup>1</sup> Esv3 系列 VM 的 Intel® 超线程技术功能。

<sup>2</sup> 受约束的可用核心大小。

<sup>3</sup> 实例与专用于单个客户的硬件隔离。

<!--Not Avaialble on ## Easv3-series (Preview)-->
## <a name="ev3-series"></a>Ev3 系列 

ACU：160 - 190 <sup>1</sup>

高级存储：不支持

高级存储缓存：不支持

Ev3 系列实例采用 Intel® Xeon® 8171M 2.1 GHz (Skylake) 或 Intel® Xeon® E5-2673 v4 2.3 GHz (Broadwell) 处理器，使用 Intel Turbo Boost Technology 2.0 可达到 3.5GHz。 Ev3 系列实例适用于内存密集型企业应用程序。

数据磁盘存储与虚拟机分开计费。 若要使用高级存储磁盘，请使用 ESv3 大小。 ESv3 系列大小的定价和计费标准与 Ev3 系列相同。 

| 大小            | vCPU | 内存:GiB | 临时存储 (SSD) GiB | 最大数据磁盘数 | 最大临时存储吞吐量：IOPS/读取 MBps/写入 MBps | 最大网卡数/网络带宽等级 |
|-----------------|-----------|-------------|----------------|----------------|----------------------------------------------------------|------------------------------|
| Standard_E2_v3  | 2         | 16          | 50             | 4              | 3000/46/23                                               | 2 / 1000                 |
| Standard_E4_v3  | 4         | 32          | 100            | 8              | 6000/93/46                                               | 2 / 2000                 |
| Standard_E8_v3  | 8         | 64          | 200            | 16             | 12000/187/93                                             | 4 / 4000                     |
| Standard_E16_v3 | 16        | 128         | 400            | 32             | 24000/375/187                                            | 8 / 8000                     |
| Standard_E20_v3 | 20 个        | 160         | 500            | 32             | 30000/469/234                                            | 8/10000                     |
| Standard_E32_v3 | 32        | 256         | 800            | 32             | 48000/750/375                                            | 8 / 16000                 |
| Standard_E48_v3 | 48        | 384         | 1200            | 32             | 96000/1000/500                                            | 8/24000                 |
| Standard_E64_v3 | 64        | 432         | 1600           | 32             | 96000/1000/500                                           | 8 / 30000           |
| Standard_E64i_v3&nbsp;<sup>2、&nbsp;3</sup> | 64        | 432         | 1600           | 32             | 96000/1000/500                                           | 8 / 30000           |

<sup>1</sup> Ev3 系列 VM 的 Intel® 超线程技术功能。

<sup>2</sup> 受约束的可用核心大小。

<sup>3</sup> 实例与专用于单个客户的硬件隔离。

<!--Not Available on ## Eav3-series (Preview)-->
<!--Not Available on ## Mv2-series-->
<!--Not Available on #### Find a SUSE image-->
<!--Not Available on #### Select a SUSE image via Azure CLI-->

## <a name="m-series"></a>M 系列 

ACU：160-180 <sup>1</sup>

高级存储：支持

高级存储缓存：支持

M 系列大小基于 Intel(R) Xeon(R) CPU E7-8890 v3 @ 2.50GHz   

写入加速器：[支持](/virtual-machines/windows/how-to-enable-write-accelerator)

<!-- NOTICE: 最大 NIC 数/预期网络带宽 (Mbps) SHOULD BE (Mbps) -->

| 大小            | vCPU | 内存:GiB | 临时存储 (SSD) GiB | 最大数据磁盘数 | 最大缓存吞吐量和临时存储吞吐量：IOPS/Mbps（以 GiB 为单位的缓存大小） | 最大非缓存磁盘吞吐量：IOPS/Mbps | 最大 NIC 数/预期网络带宽 (Mbps) |
|-----------------|------|-------------|----------------|----------------|-----------------------------------------------------------------------|-------------------------------------------|------------------------------|
| Standard_M8ms&nbsp;<sup>3</sup>    | 8  | 218.75 | 256  | 8  | 10000 / 100 (793)  | 5000 / 125 | 4 / 2000 |
| Standard_M16ms&nbsp;<sup>3</sup>   | 16 | 437.5  | 512  | 16 | 20000 / 200 (1587) | 10000 / 250 | 8 / 4000 |
| Standard_M32ts | 32 | 192    | 1024 | 32 | 40000 / 400 (3174) | 20000 / 500 | 8 / 8000 |
| Standard_M32ls | 32 | 256    | 1024 | 32 | 40000 / 400 (3174) | 20000 / 500 | 8 / 8000 |
| Standard_M32ms&nbsp;<sup>3</sup>   | 32 | 875    | 1024 | 32 | 40000 / 400 (3174) | 20000 / 500 | 8 / 8000 |
| Standard_M64s  | 64 | 1024   | 2048 | 64 | 80000 / 800 (6348)| 40000 / 1000 | 8 / 16000          |
| Standard_M64ls  | 64 | 512    | 2048 | 64 | 80000 / 800 (6348) | 40000 / 1000 | 8 / 16000 |
| Standard_M64ms&nbsp;<sup>3</sup>  | 64   | 1792 | 2048 | 64 | 80000 / 800 (6348)| 40000 / 1000 | 8 / 16000          |
| Standard_M128s&nbsp;<sup>2</sup> | 128  | 2048        | 4096  | 64 | 160000 / 1600 (12696) | 80000 / 2000                            | 8 / 30000          |
| Standard_M128ms&nbsp;<sup>2,&nbsp;3,&nbsp;4</sup> | 128  | 3892  | 4096 | 64 | 160000 / 1600 (12696) | 80000 / 2000                            | 8 / 30000          |
| Standard_M64   | 64  | 1024 | 7168  | 64 | 80000  / 800  (1228) | 40000 / 1000 | 8 / 16000 |
| Standard_M64m  | 64  | 1792 | 7168  | 64 | 80000  / 800  (1228) | 40000 / 1000 | 8 / 16000 |
| Standard_M128&nbsp;<sup>2  | 128 | 2048 | 14336 | 64 | 250000 / 1600 (2456) | 80000 / 2000 | 8 / 32000 |
| Standard_M128m&nbsp;<sup>2 | 128 | 3892 | 14336 | 64 | 250000 / 1600 (2456) | 80000 / 2000 | 8 / 32000 |

<sup>1</sup> M 系列 VM 的 Intel® 超线程技术功能

<sup>2</sup> 超过 64 vCPU 的 VM 需要以下受支持的来宾 OS 之一：Windows Server 2016、Ubuntu 16.04 LTS、SLES 12 SP2 和 Red Hat Enterprise Linux、CentOS 7.3 或带 LIS 4.2.1 的 Oracle Linux 7.3。

<sup>3</sup> 受约束的可用核心大小。

<sup>4</sup> 实例与专用于单个客户的硬件隔离。
<br />

<a name="dsv2-series-11-15"></a>
## <a name="dsv2-series-11-15"></a>DSv2 系列 11-15

<!-- NOTICE: 最大 NIC 数/预期网络带宽 (Mbps) SHOULD BE (Mbps) -->

ACU：210 - 250 <sup>1</sup>

高级存储：支持

高级存储缓存：支持

DSv2 系列大小在 Intel® Xeon® 8171M 2.1 GHz (Skylake) 或 Intel® Xeon® E5-2673 v4 2.3 GHz (Broadwell) 或 Intel® Xeon® E5-2673 v3 2.4 GHz (Haswell) 处理器上运行。

| 大小 | vCPU | 内存:GiB | 临时存储 (SSD) GiB | 最大数据磁盘数 | 最大缓存吞吐量和临时存储吞吐量：IOPS/Mbps（以 GiB 为单位的缓存大小） | 最大非缓存磁盘吞吐量：IOPS/Mbps | 最大 NIC 数/预期网络带宽 (Mbps) |
| --- | --- | --- | --- | --- | --- | --- | --- |
| Standard_DS11_v2&nbsp;<sup>3</sup> |2 |14 |28 |8 |8000 / 64 (72) |6400 / 96 |2 / 1500 |
| Standard_DS12_v2&nbsp;<sup>3</sup> |4 |28 |56 |16 |16000 / 128 (144) |12800 / 192 |4 / 3000 |
| Standard_DS13_v2&nbsp;<sup>3</sup> |8 |56 |112 |32 |32000 / 256 (288) |25600 / 384 |8 / 6000 |
| Standard_DS14_v2&nbsp;<sup>3</sup>|16 |112 |224 |64 |64000 / 512 (576) |51200 / 768 |8 / 12000 |
| Standard_DS15_v2&nbsp;<sup>2</sup> |20 个 |140 |280 |64 |80000 / 640 (720) |64000 / 960 |8 / 25000&nbsp;<sup>4</sup>

<!-- Please acknowledge that DSv2 Max Disk Count are 8,16,32,64,64 -->

<sup>1</sup> DSv2 系列 VM 可能的最大磁盘吞吐量（IOPS 或 Mbps）可能受限于附加磁盘的数量、大小和条带化。  有关详细信息，请参阅[为实现高性能而设计](../articles/virtual-machines/windows/premium-storage-performance.md)。  
<sup>2</sup> 实例对于专用于单个客户的硬件独立。  
<sup>3</sup> 受约束的可用核心大小。  
<sup>4</sup> 25000 Mbps，具有加速网络。 

<br />

## <a name="dv2-series-11-15"></a>Dv2 系列 11-15

ACU：210 - 250

高级存储：不支持

高级存储缓存：不支持

| 大小              | vCPU | 内存:GiB | 临时存储 (SSD) GiB | 最大临时存储吞吐量：IOPS/读取 MBps/写入 MBps | 最大的数据磁盘/吞吐量：IOPS | 最大 NIC 数/预期网络带宽 (Mbps) |
|-------------------|-----------|-------------|----------------|----------------------------------------------------------|-----------------------------------|------------------------------|
| Standard_D11_v2   | 2         | 14          | 100            | 6000/93/46                                           | 8/8x500                         | 2 / 1500                     |
| Standard_D12_v2   | 4         | 28          | 200            | 12000/187/93                                         | 16/16x500                         | 4 / 3000                     |
| Standard_D13_v2   | 8         | 56          | 400            | 24000/375/187                                        | 32/32x500                       | 8 / 6000                     |
| Standard_D14_v2   | 16        | 112         | 800            | 48000/750/375                                        | 64/64x500                       | 8 / 12000          |
| Standard_D15_v2&nbsp;<sup>1</sup> | 20 个        | 140         | 1000          | 60000/937/468                                        | 64/64x500                       | 8 / 25000&nbsp;<sup>2</sup> |

<!-- Please acknowledge that Dv2 Max Disk Count are 8,16,32,64,64 -->

<sup>1</sup> 实例对于专用于单个客户的硬件独立。  
<sup>2</sup> 25000 Mbps，具有加速网络。

<!-- Update_Description: update meta properties, wording update, update link -->

