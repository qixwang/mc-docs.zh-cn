---
title: include 文件
description: include 文件
services: virtual-machines-linux
author: rockboyfor
ms.service: virtual-machines-linux
ms.topic: include
origin.date: 02/11/2019
ms.date: 05/18/2020
ms.author: v-yeche
ms.custom: include file
ms.openlocfilehash: f933a891fe9eebe3fb0c48f553094a30b726b007
ms.sourcegitcommit: 8d56bc6baeb42d675695ecef1909d76f5c4a6ae3
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/14/2020
ms.locfileid: "83406193"
---
## <a name="supported-distributions-and-drivers"></a>支持的分发和驱动程序

### <a name="nvidia-cuda-drivers"></a>NVIDIA CUDA 驱动程序

仅下表列出的 Linux 发行版支持适用于 NCv3 系列 VM 的 NVIDIA CUDA 驱动程序。 本文发布时，CUDA 驱动程序信息为最新版本。 有关最新 CUDA 驱动程序，请访问 [NVIDIA](https://developer.nvidia.com/cuda-zone) 网站。 确保安装或升级到最新 CUDA 驱动程序分发软件包。 

<!-- Not Available on NC, NCv2, and ND-series-->
<!-- Not Available on (optional for NV-series)-->

> [!TIP]
> 作为一种在 Linux VM 上手动安装 CUDA 驱动程序的替代方法，可以部署 Azure [数据科学虚拟机](../articles/machine-learning/data-science-virtual-machine/overview.md)映像。 用于 Ubuntu 16.04 LTS 或 CentOS 7.4 的 DSVM 版本预安装 NVIDIA CUDA 驱动程序、CUDA 深度神经网络库和其他工具。

| 分发 | 驱动程序 |
| --- | -- | 
| Ubuntu 16.04 LTS、18.04 LTS<br/><br/> 基于 CentOS 的 7.3、7.4、7.5、7.6 | NVIDIA CUDA 10.1，驱动程序分支为 R418 |

<!-- Not Available on Red Hat Enterprise Linux 7.3 or 7.4-->
<!--Not Available on  CentOS-based 7.4 HPC-->

<!-- Not Available on ### NVIDIA GRID drivers-->

<!-- Not Available on NV-series-->
<!-- Not Available on [Red Hat Knowledgebase article](https://access.redhat.com/articles/1067)-->
<!-- Update_Description: update meta properties, wording update, update link -->