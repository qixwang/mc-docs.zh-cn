---
title: 关于使用 Site Recovery 进行 Hyper-V VM（使用 VMM）灾难恢复到 Azure 的网络映射
description: 介绍如何使用 Azure Site Recovery 为 Hyper-V VM（在 VMM 云中托管）到 Azure 的灾难恢复设置网络映射。
author: rockboyfor
manager: digimobile
ms.service: site-recovery
ms.topic: conceptual
origin.date: 09/09/2019
ms.date: 09/30/2019
ms.author: v-yeche
ms.openlocfilehash: 710099d90ff6d49f2e7912bee65ec230d7dc9c09
ms.sourcegitcommit: c1ba5a62f30ac0a3acb337fb77431de6493e6096
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2020
ms.locfileid: "71340719"
---
# <a name="prepare-network-mapping-for-hyper-v-vm-disaster-recovery-to-azure"></a>为 Hyper-V VM 灾难恢复到 Azure 准备网络映射

本文有助于理解及准备使用 [Azure Site Recovery](site-recovery-overview.md) 服务将 System Center Virtual Machine Manager (VMM) 云中的 Hyper-V VM 复制到 Azure 或辅助站点时的网络映射。

## <a name="prepare-network-mapping-for-replication-to-azure"></a>为复制到 Azure 准备网络映射

复制到 Azure 时，网络映射在源 VMM 服务器上的 VM 网络与目标 Azure 虚拟网络之间进行映射。 映射可执行以下操作：
- **网络连接**：确保复制的 Azure VM 连接到映射的网络。 在同一网络上进行故障转移的所有计算机都可互相连接，即使这些计算机在不同的恢复计划中进行故障转移。
- **网络网关**：如果在目标 Azure 网络上设置了网络网关，VM 可连接到其他本地虚拟机。

网络映射的工作原理如下所述：

- 源 VMM VM 网络会映射到 Azure 虚拟网络。
- 故障转移后，源网络中的 Azure VM 会连接到映射的目标虚拟网络。
- 执行复制时，添加到源 VM 网络的新 VM 会连接到映射的 Azure 网络。
- 如果目标网络具有多个子网，并且其中一个子网与源虚拟机所在的子网同名，则在故障转移后副本虚拟机将连接到该目标子网。
- 如果没有具有匹配名称的目标子网，则虚拟机连接到网络中的第一个子网。

## <a name="prepare-network-mapping-for-replication-to-a-secondary-site"></a>为复制到辅助站点准备网络映射

复制到辅助站点时，网络映射在源 VMM 服务器上的 VM 网络与目标 VMM 服务器上的 VM 网络之间进行映射。 映射可执行以下操作：

- **网络连接**：在故障转移后将 VM 连接到适当的网络。 副本 VM 会连接到已映射到源网络的目标网络。
- **最佳 VM 放置**：以最佳方式在 Hyper-V 主机服务器上放置副本 VM。 副本 VM 放在可访问映射的 VM 网络的主机上。
- 无网络映射  - 如果未配置网络映射，则在故障转移后副本 VM 将不会连接到任何 VM 网络。

网络映射的工作原理如下所述：

- 可以在两个 VMM 服务器上的 VM 网络之间配置网络映射；如果两个站点由同一个服务器管理，则在可以在单个 VMM 服务器上的两个对应 VM 网络之间配置网络映射。
- 在适当配置映射且启用复制后，主位置上的 VM 将连接到网络，目标位置上的副本将连接到其映射网络。
- 在 Site Recovery 中进行网络映射期间选择目标 VM 网络时，会显示使用源 VM 网络的 VMM 源云以及用于保护的目标云上的可用目标 VM 网络。
- 如果目标网络具有多个子网，并且其中一个子网与源虚拟机所在的子网同名，则在故障转移后副本 VM 将连接到该目标子网。 如果没有具有匹配名称的目标子网，则 VM 会连接到网络中的第一个子网。

## <a name="example"></a>示例

以下示例演示了此机制。 我们来考察一个具有两个位置（北京和上海）的组织。

<!--MOONCAKE: Insert | characters if the front columns container empty characters-->

|**位置** | **VMM 服务器** | **VM 网络** | **映射到**|
|---|---|---|---|
|北京 | VMM-Beijing| VMNetwork1-Beijing | 映射到 VMNetwork1-Shanghai|
| |  | VMNetwork2-Beijing | 未映射|
|上海 | VMM-Shanghai| VMNetwork1-Shanghai | 映射到 VMNetwork1-Beijing|
| | | VMNetwork2-Shanghai | 未映射|

<!--MOONCAKE: Insert | characters if the front columns container empty characters-->

在本示例中：

- 为连接到 VMNetwork1-Beijing 的任意 VM 创建的副本 VM 将连接到 VMNetwork1-Shanghai。
- 为 VMNetwork2-Beijing 或 VMNetwork2-Shanghai 创建的副本 VM 不会连接到任何网络。

这是本示例组织中 VMM 云的设置方式，以及逻辑网络与云关联的方式。

### <a name="cloud-protection-settings"></a>云保护设置

<!--MOONCAKE: Insert | characters if the front columns container empty characters-->

|**受保护的云** | **提供保护的云** | 逻辑网络（北京）   |
|---|---|---|
|GoldCloud1 | GoldCloud2 | |
|SilverCloud1| SilverCloud2 | |
|GoldCloud2 | <p>NA</p><p></p> | <p>LogicalNetwork1-Beijing</p><p>LogicalNetwork1-Shanghai</p>|
|SilverCloud2 | <p>NA</p><p></p> | <p>LogicalNetwork1-Beijing</p><p>LogicalNetwork1-Shanghai</p>|

<!--MOONCAKE: Insert | characters if the front columns container empty characters-->

### <a name="logical-and-vm-network-settings"></a>逻辑和 VM 网络设置

<!--MOONCAKE: Insert | characters if the front columns container empty characters-->

|**位置** | **逻辑网络** | **关联的 VM 网络**|
|---|---|---|
|北京 | LogicalNetwork1-Beijing | VMNetwork1-Beijing|
|上海 | LogicalNetwork1-Shanghai | VMNetwork1-Shanghai|
| | LogicalNetwork2Shanghai | VMNetwork2-Shanghai|

<!--MOONCAKE: Insert | characters if the front columns container empty characters-->

### <a name="target-network-settings"></a>目标网络设置

<!--MOONCAKE: Insert | characters if the front columns container empty characters-->

根据这些设置，选择目标 VM 网络时，下表显示将可用的选项。

|**Select** | **受保护的云** | **提供保护的云** | **可用目标网络**|
|---|---|---|---|
|VMNetwork1-Shanghai | SilverCloud1 | SilverCloud2 | 可用|
| | GoldCloud1 | GoldCloud2 | 可用|
|VMNetwork2-Shanghai | SilverCloud1 | SilverCloud2 | 不可用|
| | GoldCloud1 | GoldCloud2 | 可用|

<!--MOONCAKE: Insert | characters if the front columns container empty characters-->

如果目标网络具有多个子网，并且其中一个子网与源虚拟机所在的子网同名，则在故障转移后副本虚拟机将连接到该目标子网。 如果没有具有匹配名称的目标子网，虚拟机将连接到网络中的第一个子网。

### <a name="failback-behavior"></a>故障回复行为

若要了解在故障回复（反向复制）情况下会发生什么，让我们假设 VMNetwork1-Beijing 已映射到VMNetwork1-Shanghai，设置如下。

**VM** | **连接 VM 网络**
---|---
VM1 | VMNetwork1-Network
VM2（VM1 的副本） | VMNetwork1-Shanghai

在采用这些设置的条件下，我们看看在一些可能的方案中会发生什么情况。

**方案** | **结果**
---|---
在故障转移后，VM-2 的网络属性没有发生任何变化。 | VM-1 保持连接到源网络。
在故障转移后，VM-2 的网络属性发生变化，且连接断开。 | VM-1 断开连接。
VM-2 的网络属性在故障转移后发生更改并连接到 VMNetwork2-Shanghai。 | 如果未映射 VMNetwork2-Shanghai，则 VM-1 将断开连接。
VMNetwork1-Shanghai 的网络映射已更改。 | VM-1 将连接到现已映射到 VMNetwork1-Shanghai 的网络。

## <a name="next-steps"></a>后续步骤

- [了解](hyper-v-vmm-networking.md)在故障转移到辅助 VMM 站点后的 IP 寻址。
- [了解](concepts-on-premises-to-azure-networking.md)在故障转移到 Azure 后的 IP 寻址。

<!-- Update_Description: update meta properties  -->