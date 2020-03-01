---
title: 优化 VM 网络吞吐量 | Azure
description: 了解如何优化 Azure 虚拟机的网络吞吐量。
services: virtual-network
documentationcenter: na
author: rockboyfor
manager: digimobile
editor: ''
ms.assetid: ''
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
origin.date: 11/15/2017
ms.date: 02/24/2020
ms.author: v-yeche
ms.openlocfilehash: e95688a5de645194b227e46ba95a1c7abef82af7
ms.sourcegitcommit: f06e1486873cc993c111056283d04e25d05e324f
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/27/2020
ms.locfileid: "77653260"
---
# <a name="optimize-network-throughput-for-azure-virtual-machines"></a>优化 Azure 虚拟机的网络吞吐量

Azure 虚拟机 (VM) 的默认网络设置可以进一步针对网络吞吐量进行优化。 本文介绍如何优化 Azure Windows 和 Linux VM（包括 Ubuntu 和 CentOS 等主要发行版）的网络吞吐量。

<!-- Not Avaiable on Red Hat -->

## <a name="windows-vm"></a>Windows VM

如果 Windows VM 支持[加速网络](create-vm-accelerated-networking-powershell.md)，则启用该功能会是吞吐量的最佳配置。 对于所有其他 Windows VM，与不使用 RSS 的 VM 相比，使用接收方缩放 (RSS) 可达到更高的最大吞吐量。 RSS 在 Windows VM 中默认已禁用。 完成以下步骤以确定是否启用了 RSS 并在处于禁用状态时启用：

1. 使用 `Get-NetAdapterRss` PowerShell 命令查看是否为网络适配器启用了 RSS。 从 `Get-NetAdapterRss`返回的以下示例输出中可以看出，RSS 未启用。

    ```powershell
    Name                    : Ethernet
    InterfaceDescription    : Microsoft Hyper-V Network Adapter
    Enabled                 : False
    ```
    
    <!--CORRECT ON Microsoft Hyper-V Network Adapter-->
    
2. 若要启用 RSS，请输入以下命令：

    ```powershell
    Get-NetAdapter | % {Enable-NetAdapterRss -Name $_.Name}
    ```
    前一个命令没有输出。 该命令更改了 NIC 设置，导致出现暂时性连接断开大约一分钟。 连接断开期间会显示“重新连接”对话框。 通常在第三次尝试后，连接会恢复。
3. 再次输入 `Get-NetAdapterRss` 命令，确认 RSS 是否在 VM 中启用。 如果成功，则返回以下示例输出：

    ```powershell
    Name                    : Ethernet
    InterfaceDescription    : Microsoft Hyper-V Network Adapter
    Enabled                  : True
    ```
    
    <!--CORRECT ON Microsoft Hyper-V Network Adapter-->

## <a name="linux-vm"></a>Linux VM

默认情况下，RSS 在 Azure Linux VM 中始终启用。 自 2017 年 12 月以后发布的 Linux 内核均包含新的网络优化选项，可使 Linux VM 实现更高的网络吞吐量。

### <a name="ubuntu-for-new-deployments"></a>用于新部署的 Ubuntu

Ubuntu Azure 内核在 Azure 上提供最佳网络性能，并且自 2017 年 9 月 21 日起已成为默认内核。 若要获得此内核，请首先安装 16.04-LTS 的最新支持版本，如下所述：

```json
"Publisher": "Canonical",
"Offer": "UbuntuServer",
"Sku": "16.04-LTS",
"Version": "latest"
```

创建完成后，输入以下命令获取最新更新。 这些步骤也适用于当前正在运行 Ubuntu Azure 内核的 VM。

```bash
#run as root or preface with sudo
apt-get -y update
apt-get -y upgrade
apt-get -y dist-upgrade
```

对于已经有了 Azure 内核但由于出错而无法进一步更新的现有 Ubuntu 部署，以下可选命令集可能会非常有用。

```bash
#optional steps may be helpful in existing deployments with the Azure kernel
#run as root or preface with sudo
apt-get -f install
apt-get --fix-missing install
apt-get clean
apt-get -y update
apt-get -y upgrade
apt-get -y dist-upgrade
```

#### <a name="ubuntu-azure-kernel-upgrade-for-existing-vms"></a>现有 VM 的 Ubuntu Azure 内核升级

重要的吞吐量性能可通过升级到 Azure Linux 内核来实现。 若要验证是否具有此内核，请检查你的内核版本。

```bash
#Azure kernel name ends with "-azure"
uname -r

#sample output on Azure kernel:
#4.13.0-1007-azure
```

如果 VM 没有 Azure 内核，版本号将通常以“4.4”开头。 如果 VM 没有 Azure 内核，请以 root 身份运行以下命令：

```bash
#run as root or preface with sudo
apt-get update
apt-get upgrade -y
apt-get dist-upgrade -y
apt-get install "linux-azure"
reboot
```

### <a name="centos"></a>CentOS

若要获得最新优化，最好通过指定以下参数，创建具有最新支持版本的 VM：

```json
"Publisher": "OpenLogic",
"Offer": "CentOS",
"Sku": "7.4",
"Version": "latest"
```

新的和现有的 VM 可受益于安装最新 Linux Integration Services (LIS)。 从 4.2.2-2 开始，吞吐量优化位于 LIS 中，尽管更高版本包含更多改进。 输入以下命令以安装最新 LIS：

```bash
sudo yum update
sudo reboot
sudo yum install microsoft-hyper-v
```

<!-- Not Avaiable on ### Red Hat -->

## <a name="next-steps"></a>后续步骤
* 请参阅[带宽/吞吐量测试 Azure VM](virtual-network-bandwidth-testing.md)，查阅方案的优化结果。
* 阅读有关如何[为虚拟机分配带宽](virtual-machine-network-throughput.md)的信息
* 通过 [Azure 虚拟网络常见问题解答 (FAQ)](virtual-networks-faq.md) 了解详细信息

<!--Update_Description: update meta properties, wording update，update link  -->