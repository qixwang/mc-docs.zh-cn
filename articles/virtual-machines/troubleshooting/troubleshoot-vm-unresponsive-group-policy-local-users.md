---
title: 应用“组策略本地用户和组”策略时虚拟机无响应
description: 本文提供的步骤用于解决在 Azure 虚拟机 (VM) 中启动时加载屏幕停滞在“正在应用策略”状态的问题。
services: virtual-machines-windows
documentationcenter: ''
author: rockboyfor
manager: digimobile
editor: ''
tags: azure-resource-manager
ms.assetid: ff113268-f5bf-4e6a-986e-63b9b0ceff20
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: na
ms.topic: troubleshooting
origin.date: 04/02/2020
ms.date: 05/06/2020
ms.author: v-yeche
ms.openlocfilehash: 9110ab0d0a097dafc15a53cb0e81ad7985375e4e
ms.sourcegitcommit: 81241aa44adbcac0764e2b5eb865b96ae56da6b7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/09/2020
ms.locfileid: "83001905"
---
<!--Verified successfully-->
# <a name="virtual-machine-is-unresponsive-while-applying-group-policy-local-users--groups-policy"></a>应用“组策略本地用户和组”策略时虚拟机无响应

本文提供的步骤用于解决在 Azure 虚拟机 (VM) 中启动时加载屏幕停滞在“正在应用策略”状态的问题。

## <a name="symptom"></a>症状

使用[启动诊断](/virtual-machines/troubleshooting/boot-diagnostics)查看 VM 的屏幕截图时，看到屏幕停滞在“正在加载”状态并显示以下消息：“正在应用‘组策略本地用户和组’策略”  。

![替代文本：显示“正在应用‘组策略本地用户和组’策略”加载状态的屏幕 (Windows Server 2012)。](media/troubleshoot-vm-unresponsive-group-policy-local-users/1.png)

Windows Server 2012

![替代文本：显示“正在应用‘组策略本地用户和组’策略”加载状态的屏幕 (Windows Server 2012 R2)。](media/troubleshoot-vm-unresponsive-group-policy-local-users/2.png)

Windows Server 2012 R2

## <a name="cause"></a>原因

这种冻结状态的症状是 Windows 配置文件服务动态链接库 (profsvc.dll) 中的代码缺陷造成的  。

> [!NOTE]
> 此缺陷只存在于 Windows Server 2012 和 Windows Server 2012 R2 上。

### <a name="the-policy-in-question"></a>有问题的策略

正在应用的无法完成其进程的策略为：

* “计算机配置”\“策略”\“管理模板”\“系统/用户配置文件”\“在系统重启时，删除超过指定天数的用户配置文件” 

仅当符合以下六个条件时，此策略才会挂起：

* 已启用“在系统重启时，删除超过指定天数的用户配置文件”策略  。
* 你的配置文件达到期限要求，因此需要清理。
* 你的组件已注册配置文件删除通知。
* 组件发出的调用（直接或间接）需要从 Windows 服务控制管理器 (SCM) 组件获取数据，例如关于某项服务的启动、停止或查询信息。
* 你的某个服务已配置为自动启动  。
* 此服务设置为在域帐户的上下文中运行（而不是使用内置帐户，例如本地系统）。

### <a name="the-code-defect"></a>代码缺陷

代码缺陷的原因是服务控制管理器 (SCM) 和配置文件服务尝试同时相互应用锁。 锁的作用是防止多个服务同时对相同的数据进行更改，避免损坏。 一般情况下，多个锁请求不会导致问题。 但是，这种情况发生在启动过程中，因此这两个服务都不能完成其进程，因为它们都处于停滞状态，互相等待对方完成其操作。

### <a name="os-bug-5880648---service-control-manager-deadlocks-with-the-delete-user-profiles-on-restart-policy"></a>OS Bug 5880648 - 服务控制管理器在处理“重启时删除用户配置文件”策略时死锁

存在两个重叠的操作，其中：

* 操作 1 获取了配置文件锁，但尚未获得 SCM 锁。

    **AND**

* 操作 2 获取了 SCM 锁，但尚未获得配置文件锁。

发生这种死锁后，尝试获取第二个所需的锁会挂起该操作。

### <a name="action-1---old-profile-deletion-notification-has-profile-lock-needs-scm-lock"></a>操作 1 - 旧配置文件删除通知（已获取配置文件锁，需要 SCM 锁）  

1. 首先，设置为删除旧配置文件的策略会获取内部配置文件服务锁。

    * 此锁用于防止在删除操作正在进行时有两个线程与配置文件交互  。

2. 该策略会查找因太旧而需要删除的配置文件。
3. 在删除配置文件的过程中，已注册配置文件删除通知的组件会尝试启动某个服务  。
4. 在启动该服务之前，服务控制管理器 (SCM) 需要获取操作 2 中的线程所持有的内部 SCM 锁   。

### <a name="action-2---profile-loadcreation-for-user-specific-data-has-scm-lock-needs-profile-lock"></a>操作 2 - 为用户特定的数据加载/创建配置文件（已获取 SCM 锁，需要配置文件锁）  

1. 在启动时，SCM 需要按组为所有自动启动服务以及这些服务所依赖的任何服务排序  。

2. 当它为服务排序时，会使用“SCM 获取内部 SCM 锁”来控制启动、停止或配置服务的访问权限  。

3. 服务排序以后，SCM 就会循环访问每个服务并启动它。

4. 如果服务在某个域帐户的上下文中运行，则需要为该域帐户加载或创建一个配置文件，以便可以存储用户特定的数据。

5. 此请求将发送到配置文件服务  。

6. 配置文件服务需要访问在操作 1 中获取的内部锁   。

## <a name="solution"></a>解决方案

### <a name="process-overview"></a>流程概述

1. 创建并访问修复 VM
2. 启用串行控制台和内存转储收集
3. 重建 VM
4. 收集内存转储文件

    > [!NOTE]
    > 遇到此启动错误时，来宾 OS 不会正常运行。 需要在脱机模式下进行故障排除才能解决此问题。

### <a name="create-and-access-a-repair-vm"></a>创建并访问修复 VM

1. 使用 [VM 修复命令的步骤 1-3](/virtual-machines/troubleshooting/repair-windows-vm-using-azure-virtual-machine-repair-commands#repair-process-example) 来准备一个修复 VM。
2. 使用远程桌面连接来连接到修复 VM。

### <a name="enable-serial-console-and-memory-dump-collection"></a>启用串行控制台和内存转储收集

若要启用内存转储收集和串行控制台，请运行以下脚本：

1. 打开提升的命令提示符会话（以管理员身份运行）。
2. 运行以下命令：

    * 启用串行控制台：

        `bcdedit /store <VOLUME LETTER WHERE THE BCD FOLDER IS>:\boot\bcd /ems {<BOOT LOADER IDENTIFIER>} ON`

        `bcdedit /store <VOLUME LETTER WHERE THE BCD FOLDER IS>:\boot\bcd /emssettings EMSPORT:1 EMSBAUDRATE:115200`

3. 验证 OS 磁盘上的可用空间是否与 VM 上的内存大小 (RAM) 相当。

    * 如果 OS 磁盘上没有足够的空间，则应更改内存转储文件的创建位置，将其指向任何已附加到 VM 且具有足够可用空间的数据磁盘。 若要更改位置，请在以下命令中将 `%SystemRoot%` 替换为数据磁盘的驱动器号（例如“F:”）。

#### <a name="suggested-configuration-to-enable-os-dump"></a>用于启用 OS 转储的建议配置

**加载损坏的 OS 磁盘：**

`REG LOAD HKLM\BROKENSYSTEM <VOLUME LETTER OF BROKEN OS DISK>:\windows\system32\config\SYSTEM`

**在 ControlSet001 上启用：**

`REG ADD "HKLM\BROKENSYSTEM\ControlSet001\Control\CrashControl" /v CrashDumpEnabled /t REG_DWORD /d 1 /f`

`REG ADD "HKLM\BROKENSYSTEM\ControlSet001\Control\CrashControl" /v DumpFile /t REG_EXPAND_SZ /d "%SystemRoot%\MEMORY.DMP" /f`

`REG ADD "HKLM\BROKENSYSTEM\ControlSet001\Control\CrashControl" /v NMICrashDump /t REG_DWORD /d 1 /f`

**在 ControlSet002 上启用：**

`REG ADD "HKLM\BROKENSYSTEM\ControlSet002\Control\CrashControl" /v CrashDumpEnabled /t REG_DWORD /d 1 /f`

`REG ADD "HKLM\BROKENSYSTEM\ControlSet002\Control\CrashControl" /v DumpFile /t REG_EXPAND_SZ /d "%SystemRoot%\MEMORY.DMP" /f`

`REG ADD "HKLM\BROKENSYSTEM\ControlSet002\Control\CrashControl" /v NMICrashDump /t REG_DWORD /d 1 /f`

### <a name="rebuild-the-vm"></a>重建 VM

使用 [VM 修复命令的步骤 5](/virtual-machines/troubleshooting/repair-windows-vm-using-azure-virtual-machine-repair-commands#repair-process-example) 重新装配 VM。

### <a name="collect-the-memory-dump-file"></a>收集内存转储文件

若要解决此问题，需先收集故障的内存转储文件，然后使用此内存转储文件联系支持部门。 若要收集转储文件，请执行以下步骤：

#### <a name="attach-the-os-disk-to-a-new-repair-vm"></a>将 OS 磁盘附加到新的修复 VM

1. 使用 [VM 修复命令的步骤 1-3](/virtual-machines/troubleshooting/repair-windows-vm-using-azure-virtual-machine-repair-commands#repair-process-example) 来准备一个新的修复 VM。

2. 使用远程桌面连接来连接到修复 VM。

#### <a name="locate-the-dump-file-and-submit-a-support-ticket"></a>找到转储文件并提交支持票证

1. 在修复 VM 上，转到附加的 OS 磁盘中的 Windows 文件夹。 如果分配给附加 OS 磁盘的驱动器号为 F，则需转到 F:\Windows。

2. 找到 memory.dmp 文件，然后使用该内存转储文件[提交支持票证](https://support.azure.cn/support/support-azure/)。

3. 如果在查找 memory.dmp 文件时遇到问题，可以按照指南使用 NMI 调用来[生成内核或完整故障转储](https://docs.microsoft.com/windows/client-management/generate-kernel-or-complete-crash-dump)文件。

    <!--Not Available on [non-maskable interrupt (NMI) calls in serial console](https://docs.microsoft.com/azure/virtual-machines/troubleshooting/serial-console-windows#use-the-serial-console-for-nmi-calls)-->

<!-- Update_Description: new article about troubleshoot vm unresponsive group policy local users -->
<!--NEW.date: 04/27/2020-->