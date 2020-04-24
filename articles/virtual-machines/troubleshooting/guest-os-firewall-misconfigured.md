---
title: Azure VM 来宾 OS 防火墙配置不正确 | Azure
description: ''
services: virtual-machines-windows
documentationcenter: ''
author: rockboyfor
manager: digimobile
editor: ''
tags: ''
ms.service: virtual-machines
ms.topic: troubleshooting
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: azurecli
origin.date: 11/22/2018
ms.date: 11/11/2019
ms.author: v-yeche
ms.openlocfilehash: 8e3c7967cfdeacfd6d6d8426a6b98e9e9a2c53c6
ms.sourcegitcommit: c1ba5a62f30ac0a3acb337fb77431de6493e6096
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2020
ms.locfileid: "74116899"
---
# <a name="azure-vm-guest-os-firewall-is-misconfigured"></a>Azure VM 来宾 OS 防火墙配置不正确

本文介绍如何在 Azure VM 上修复配置不正确的来宾操作系统防火墙。

## <a name="symptoms"></a>症状

1. 虚拟机 (VM) 欢迎屏幕显示 VM 已完全加载。

2. 根据来宾操作系统的配置方式，可能有一些网络流量到达 VM，也可能没有。

## <a name="cause"></a>原因

来宾系统防火墙配置不正确可能会阻止部分或所有类型的网络流量发往 VM。

## <a name="solution"></a>解决方案

在执行这些步骤之前，请创建受影响 VM 的系统磁盘快照作为备份。 有关详细信息，请参阅[拍摄磁盘快照](../windows/snapshot-copy-managed-disk.md)。

若要排查此问题，可通过将 VM 的系统磁盘附加到恢复 VM 来[修复 VM 脱机](troubleshoot-rdp-internal-error.md#repair-the-vm-offline)。

<!-- Not Available on use the Serial Console -->
<!--Not Available on ## Online mitigations-->

### <a name="offline-mitigations"></a>脱机缓解措施

1. 若要启用或禁用防火墙规则，请参阅[在 Azure VM 来宾 OS 上启用或禁用防火墙规则](enable-disable-firewall-rule-guest-os.md)。

2. 检查自己是否在实施[来宾 OS 防火墙阻止入站流量方案](guest-os-firewall-blocking-inbound-traffic.md)。

3. 如果仍怀疑防火墙是否正在阻止你的访问，请参阅[在 Azure VM 中禁用来宾 OS 防火墙](disable-guest-os-firewall-windows.md)，然后通过使用正确的规则来重新启用来宾系统防火墙。

<!-- Update_Description: wording update -->
