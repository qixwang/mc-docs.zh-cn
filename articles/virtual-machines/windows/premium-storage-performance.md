---
title: Azure 高级存储 - 高性能设计
description: 使用 Azure 高级 SSD 托管磁盘设计高性能应用程序。 高级存储为 Azure 虚拟机上运行的 I/O 密集型工作负载提供高性能、低延迟的磁盘支持。
author: rockboyfor
ms.service: virtual-machines-windows
ms.topic: conceptual
origin.date: 06/27/2017
ms.date: 08/10/2020
ms.testscope: no
ms.testdate: 07/06/2020
ms.author: v-yeche
ms.subservice: disks
ms.openlocfilehash: 7e06da0d9a2d3207d5468e39601c95c6f91ec7da
ms.sourcegitcommit: ac70b12de243a9949bf86b81b2576e595e55b2a6
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/07/2020
ms.locfileid: "87917184"
---
# <a name="azure-premium-storage-design-for-high-performance"></a>Azure 高级存储：高性能设计

[!INCLUDE [virtual-machines-common-premium-storage-introduction](../../../includes/virtual-machines-common-premium-storage-introduction.md)]

> [!NOTE]
> 有时，显示为磁盘性能问题的原因实际上是网络瓶颈。 在这些情况下，应优化[网络性能](../../virtual-network/virtual-network-optimize-network-bandwidth.md)。
>
> 如果你希望对磁盘进行基准测试，请参阅我们的关于[磁盘基准测试](disks-benchmarks.md)的文章。
>
> 如果 VM 支持加速网络，则应确保它已启用。 如果未启用，则可以在 [Windows](../../virtual-network/create-vm-accelerated-networking-powershell.md#enable-accelerated-networking-on-existing-vms) 和 [Linux](../../virtual-network/create-vm-accelerated-networking-cli.md#enable-accelerated-networking-on-existing-vms) 上已部署的 VM 上启用它。

如果不熟悉高级存储，请在开始之前先阅读[为 IaaS VM 选择 Azure 磁盘类型](disks-types.md)和[高级页 blob 存储帐户的可伸缩性目标](../../storage/blobs/scalability-targets-premium-page-blobs.md)。

[!INCLUDE [virtual-machines-common-premium-storage-performance.md](../../../includes/virtual-machines-common-premium-storage-performance.md)]

如果你希望对磁盘进行基准测试，请参阅我们的关于[磁盘基准测试](disks-benchmarks.md)的文章。

了解有关可用磁盘类型的详细信息：[选择磁盘类型](disks-types.md)  

SQL Server 用户请阅读有关 SQL Server 性能最佳实践的文章：

* [Azure 虚拟机中 SQL Server 的性能最佳做法](../../azure-sql/virtual-machines/windows/performance-guidelines-best-practices.md)
* [Azure 高级存储为 Azure VM 中的 SQL Server 提供最高性能](https://cloudblogs.microsoft.com/sqlserver/2015/04/23/azure-premium-storage-provides-highest-performance-for-sql-server-in-azure-vm/)

<!-- Update_Description: update meta properties, wording update, update link -->