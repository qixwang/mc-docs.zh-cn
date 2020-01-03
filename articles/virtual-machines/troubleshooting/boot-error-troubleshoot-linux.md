---
title: 对 Azure Linux 虚拟机启动错误进行故障排除 | Azure
description: 本文提供的链接指向关于对 Azure Linux 虚拟机启动错误进行故障排除的文章。
services: virtual-machines-linux
documentationCenter: ''
author: rockboyfor
manager: digimobile
editor: ''
ms.service: virtual-machines-linux
ms.topic: troubleshooting
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
origin.date: 11/20/2019
ms.date: 12/16/2019
ms.author: v-yeche
ms.openlocfilehash: 63e95e408254741144f2e511f6b10d1b24bcec3e
ms.sourcegitcommit: 4a09701b1cbc1d9ccee46d282e592aec26998bff
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/25/2019
ms.locfileid: "75348471"
---
# <a name="troubleshoot-azure-linux-virtual-machines-boot-errors"></a>对 Azure Linux 虚拟机启动错误进行故障排除

本文列出在 Azure 中启动 Linux 虚拟机 (VM) 时可能遇到的常见启动错误。 有关这些错误的详细信息，请参阅“启动错误和解决方案”部分中的文章  。

## <a name="boot-errors-and-solutions"></a>启动错误和解决方案

* [GRUB 救援](troubleshoot-vm-boot-error.md)

## <a name="next-steps"></a>后续步骤

<!--Not Available on - [VM Serial Console](serial-console-linux.md)-->

通过使用 Azure 将 OS 磁盘附加到恢复 VM 来对 Linux VM 进行故障排除：

- [Azure VM 修复](repair-linux-vm-using-azure-virtual-machine-repair-commands.md)

 磁盘交换 - 可以使用以下任一方法自动实现：
- [Power Shell 恢复脚本](https://github.com/Azure/azure-support-scripts/tree/master/VMRecovery/ResourceManager)
- [bash 恢复脚本](https://github.com/sribs/azure-support-scripts)

- [CLI](troubleshoot-recovery-disks-linux.md)
- [Azure 门户](troubleshoot-recovery-disks-portal-linux.md)

<!--Not Available on ## Disk Swap Video:-->
<!--Not Available on [this](https://youtu.be/m5t0GZ5oGAc)-->
## <a name="unofficial-solution"></a>非正式解决方案

也可尝试使用不受支持的 BETA 脚本 [ALAR](https://github.com/malachma/azure-auto-recover) 来恢复 VM

<!-- Update_Description: new article about boot error troubleshoot linux -->
<!--NEW.date: 12/16/2019-->