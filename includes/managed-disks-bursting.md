---
title: include 文件
description: include 文件
services: virtual-machines
author: rockboyfor
ms.service: virtual-machines
ms.topic: include
origin.date: 03/29/2020
ms.date: 04/27/2020
ms.author: v-yeche
ms.custom: include file
ms.openlocfilehash: a9f1eb162b8097b5caf829b11dddd9a611915c84
ms.sourcegitcommit: 564739de7e63e19a172122856ebf1f2f7fb4bd2e
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/23/2020
ms.locfileid: "82093554"
---
<!--CONFIRM THE DEOPLOMENT REGIONS BEFORE RELEASEMENT-->
高级 SSD 支持磁盘突发。 <= 512 GiB（P20 或以下）的任何高级 SSD 磁盘大小都支持突发。 这些磁盘大小按照“尽力而为”的原则支持突发，并使用一种积分系统来管理突发。 每当磁盘流量低于根据其磁盘大小预配的性能目标时，积分就会在突发桶中累积；当发生流量突发并且流量超过该目标时，就会消耗积分。 磁盘流量是针对预配目标中的 IOPS 和带宽跟踪的。 磁盘突发不会绕过针对 IOPS 或吞吐量的虚拟机 (VM) 大小限制。

在支持磁盘突发的磁盘大小的新部署上，默认已启用磁盘突发。 支持磁盘突发的现有磁盘大小可以通过以下任一方法启用突发：

- 分离磁盘，然后再重新附加磁盘。
- 停止 VM，然后再启动 VM。

## <a name="burst-states"></a>突发状态

将磁盘附加到虚拟机后，所有支持突发的磁盘大小最初会获得一个完整的突发积分桶。 突发的最长持续时间由突发积分桶的大小确定。 只能累积未使用的积分，且积分不能超过积分桶的大小。 在任意时间点，磁盘突发积分桶可能处于以下三种状态之一： 

- 正在增加：此时，磁盘流量小于预配的性能目标。 如果磁盘流量超出 IOPS 和/或带宽目标，仍可以累积积分。 消耗了整个磁盘带宽时，仍可以累积 IO 积分，反之亦然。  

- 正在减少：此时，磁盘流量大于预配的性能目标。 突发流量将独立消耗 IOPS 或带宽的积分。 

- 保持稳定：此时，磁盘流量与预配的性能目标完全相同。 

下表汇总了提供突发支持的磁盘大小以及突发规范。

## <a name="regional-availability"></a>区域可用性

<!--CORRECT ON Azure China Cloud-->

磁盘突发在 Azure 中国云的所有区域中可用。

## <a name="disk-sizes"></a>磁盘大小

[!INCLUDE [disk-storage-premium-ssd-sizes](disk-storage-premium-ssd-sizes.md)]

## <a name="example-scenarios"></a>示例方案

为帮助你更好地了解磁盘突发的工作原理，下面提供了几个示例场景：

- 可以受益于磁盘突发的一个常见场景是，VM 的启动速度更快，以及 OS 磁盘上启动应用程序的速度更快。 以一个包含 8 GiB OS 映像的 Linux VM 为例。 如果我们使用 P2 磁盘作为 OS 磁盘，则预配的目标为 120 IOPS 和 25 MiB。 当 VM 启动时，加载启动文件的 OS 磁盘会出现读取高峰。 引入突发后，能够以最大突发速度 3500 IOPS 和 170 MiB 进行读取，从而可将加载速度至少提高 6 倍。 VM 启动后，OS 磁盘上的流量级别通常较低，因为应用程序的大部分数据操作是针对附加的数据磁盘执行的。 如果流量低于预配的目标，则会累积积分。

- 如果你正在托管一个远程虚拟桌面环境，每当活动的用户启动诸如 AutoCAD 之类的应用程序时，OS 磁盘的读取流量会大幅增加。 在这种情况下，突发流量会消耗累积的积分，使你能够超出预配的目标，以快得多的速度启动该应用程序。

- P1 磁盘的预配目标为 120 IOPS 和 25 MiB。 如果磁盘上的实际流量在过去 1 秒的时间间隔内为 100 IOPS 和 20 MiB，则未使用的 20 IO 和 5 MB 的积分将计入到磁盘的突发桶中。 以后，当流量超出预配的目标（但不超过最大突发限制）时，可以使用突发桶中的积分。 最大突发限制定义了磁盘流量的上限，即使有可供使用的突发积分，也不能超过此上限。 在本例中，即使积分桶中有 10000 IO，P1 磁盘发出的流量也不能超过最大突发量，即每秒 3500 IO。

<!-- Update_Description: new article about managed disks bursting -->
<!--NEW.date: 04/27/2020-->