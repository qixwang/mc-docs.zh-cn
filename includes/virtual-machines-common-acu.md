---
title: include 文件
description: include 文件
services: virtual-machines
author: rockboyfor
ms.service: virtual-machines
ms.topic: include
origin.date: 12/21/2018
ms.date: 07/01/2019
ms.author: v-yeche
ms.custom: include file
ms.openlocfilehash: 9f5c3a7b760c7b645f17cf4ba0ccf197e709d925
ms.sourcegitcommit: c1ba5a62f30ac0a3acb337fb77431de6493e6096
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2020
ms.locfileid: "67570339"
---
Azure 计算单位 (ACU) 这一概念提供一种比较 Azure SKU 的计算 (CPU) 性能的方法。 这有助于轻松确定最有可能满足性能需求的 SKU。  ACU 目前在小型 (Standard_A1) VM 上标准为 100，而所有其他 SKU 表示 SKU 在运行标准基准测试时大约可以有多快。 

> [!IMPORTANT]
> ACU 只是一种规则。  工作负荷的结果可能会有所不同。 
> 
> 

<br />

| SKU 系列 | ACU \ vCPU | vCPU：核心 |
| --- | --- |---|
| [A0](../articles/virtual-machines/windows/sizes-general.md) |50 | 1:1 |
| [A1 - A4](../articles/virtual-machines/windows/sizes-general.md) |100 | 1:1 |
| [A5 - A7](../articles/virtual-machines/windows/sizes-general.md) |100 | 1:1 |
| [A1_v2 - A8_v2](../articles/virtual-machines/windows/sizes-general.md) |100 | 1:1 |
| [A2m_v2 - A8m_v2](../articles/virtual-machines/windows/sizes-general.md) |100 | 1:1 |
| [D1 - D14](../articles/virtual-machines/windows/sizes-general.md) |160 - 250 | 1:1 |
| [D1_v2 - D15_v2](../articles/virtual-machines/windows/sizes-general.md) |210 - 250* | 1:1 |
| [DS1 - DS14](../articles/virtual-machines/virtual-machines-windows-sizes-memory.md) |160 - 250 | 1:1 |
| [DS1_v2 - DS15_v2](../articles/virtual-machines/virtual-machines-windows-sizes-memory.md) |210 - 250* | 1:1 |
| [D_v3](../articles/virtual-machines/virtual-machines-windows-sizes-general.md) |160 - 190* | 2:1\*\*\* |
| [Ds_v3](../articles/virtual-machines/virtual-machines-windows-sizes-general.md) |160 - 190* | 2:1\*\*\* |
| [E_v3](../articles/virtual-machines/virtual-machines-windows-sizes-memory.md) |160 - 190* | 2:1\*\*\*|
| [Es_v3](../articles/virtual-machines/virtual-machines-windows-sizes-memory.md) |160 - 190* | 2:1\*\*\* |
| [F2s_v2 - F72s_v2](../articles/virtual-machines/windows/sizes-compute.md) |195 - 210* | 2:1\*\*\* |
| [F1 - F16](../articles/virtual-machines/windows/sizes-compute.md) |210 - 250* | 1:1 |
| [F1s - F16s](../articles/virtual-machines/windows/sizes-compute.md) |210 - 250* | 1:1 |
| [M](../articles/virtual-machines/virtual-machines-windows-sizes-memory.md) | 160 - 180 | 2:1\*\*\* |


<!-- Not Available  [A8-A11]  -->
<!-- Not Available  [G1-G5]  -->
<!-- Not Available  [GS1-GS5]  -->
<!-- Not Available  [H] -->
<!-- Not Available  [HB] -->
<!-- Not Available  [HC] -->
<!-- Not Available  [L4s - L32s]  -->
<!-- Not Available  [L8s_v2 - L80s_v2]  -->

*ACU 使用 Intel® Turbo 技术来增加 CPU 频率和提升性能。  性能提升程度可能因 VM 大小、工作负荷和同一主机上运行的其他工作负荷而有所不同。

<!--Not Available on HB/L8s_v2-L80s_v2 serial on **ACUs use AMD® Boost technology to increase CPU frequency and provide a performance increase.  The amount of the performance increase can vary based on the VM size, workload, and other workloads running on the same host.-->

***超线程，能够运行嵌套虚拟化

<!--Update_Description: update meta properties -->