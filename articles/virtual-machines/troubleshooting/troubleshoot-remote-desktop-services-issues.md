---
title: 远程桌面服务在 Azure VM 上不启动 | Azure
description: 了解如何排查使用远程桌面服务连接到虚拟机时出现的问题 | Azure
services: virtual-machines-windows
documentationCenter: ''
author: rockboyfor
manager: digimobile
editor: ''
ms.service: virtual-machines-windows
ms.topic: troubleshooting
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
origin.date: 10/23/2018
ms.date: 02/10/2020
ms.author: v-yeche
ms.openlocfilehash: 5acfdb428353f837646bca8c490e52b985738035
ms.sourcegitcommit: c1ba5a62f30ac0a3acb337fb77431de6493e6096
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2020
ms.locfileid: "77428842"
---
# <a name="remote-desktop-services-isnt-starting-on-an-azure-vm"></a>远程桌面服务在 Azure VM 上不启动

本文介绍如何排查连接到 Azure 虚拟机 (VM) 时远程桌面服务 (TermService) 不启动或无法启动的问题。

> [!NOTE]  
> Azure 提供两种不同的部署模型用于创建和处理资源：[Azure 资源管理器部署模型和经典部署模型](../../azure-resource-manager/management/deployment-models.md)。 本文介绍资源管理器部署模型的用法。 建议为新部署使用此模型，而不是使用经典部署模型。

## <a name="symptoms"></a>症状

尝试连接到 VM 时遇到以下情况：

- VM 屏幕截图显示操作系统已满载，并正在等待凭据。

    ![VM 状态的屏幕截图](./media/troubleshoot-remote-desktop-services-issues/login-page.png)

- 在使用事件查看器远程查看 VM 中的事件日志时， 你发现远程桌面服务 (TermService) 未启动或无法启动。 下面是日志示例：

    **日志名称**：    系统 <br />
    **源**：      服务控制管理器 <br />
    **日期**：        2017 年 12 月 16 日上午 11:19:36<br />
    **事件 ID**：    7022<br />
    **任务类别**：无<br />
    **级别**：       错误<br />
    **关键字**：    经典<br />
    **用户**：        不适用<br />
    **计算机**: vm.contoso.com<br />
    **说明**：远程桌面服务在启动时挂起。 

    <!--Not Available on Serial Access Console-->
    <!--Not Available on wevtutil qe system /c:1 /f:text /q:"Event[System[Provider[@Name='Service Control Manager'] and EventID=7022 and TimeCreated[timediff(@SystemTime) <= 86400000]]]" | more -->

## <a name="cause"></a>原因

之所以发生此问题，是因为远程桌面服务未在 VM 上运行。 未运行的原因取决于以下情况： 

- TermService 服务设置为“已禁用”。  
- TermService 服务崩溃或未响应。 
- 错误的配置导致 TermService 不启动。

## <a name="solution"></a>解决方案

若要排查此问题，可通过将 VM 的 OS 磁盘附加到恢复 VM 来[修复 VM 脱机](#repair-the-vm-offline)。

<!--Not Available on Serial Console-->
<!-- Not Available on ### Use Serial Console-->

### <a name="repair-the-vm-offline"></a>修复 VM 脱机

#### <a name="attach-the-os-disk-to-a-recovery-vm"></a>将 OS 磁盘附加到恢复 VM

1. [将 OS 磁盘附加到恢复 VM](../windows/troubleshoot-recovery-disks-portal.md)。
2. 开始与恢复 VM 建立远程桌面连接。 确保附加的磁盘在磁盘管理控制台中标记为“联机”。  请注意分配给附加的 OS 磁盘的驱动器号。
3. 打开权限提升的命令提示符实例（“以管理员身份运行”）。  然后运行以下脚本。 假设分配给附加的 OS 磁盘的驱动器号为 **F**。请将它替换为 VM 中的相应值。 

    ```
    reg load HKLM\BROKENSYSTEM F:\windows\system32\config\SYSTEM.hiv

    REM Set default values back on the broken service 
    reg add "HKLM\BROKENSYSTEM\ControlSet001\services\TermService" /v start /t REG_DWORD /d 3 /f
    reg add "HKLM\BROKENSYSTEM\ControlSet001\services\TermService" /v ObjectName /t REG_SZ /d "NT Authority\NetworkService" /f
    reg add "HKLM\BROKENSYSTEM\ControlSet001\services\TermService" /v type /t REG_DWORD /d 16 /f
    reg add "HKLM\BROKENSYSTEM\ControlSet002\services\TermService" /v start /t REG_DWORD /d 3 /f
    reg add "HKLM\BROKENSYSTEM\ControlSet002\services\TermService" /v ObjectName /t REG_SZ /d "NT Authority\NetworkService" /f
    reg add "HKLM\BROKENSYSTEM\ControlSet002\services\TermService" /v type /t REG_DWORD /d 16 /f
    ```

4. [分离 OS 磁盘并重新创建 VM](../windows/troubleshoot-recovery-disks-portal.md)。 然后检查是否解决了问题。

## <a name="need-help-contact-support"></a>需要帮助？ 联系支持人员

如果仍需帮助，请[联系支持人员](https://support.azure.cn/support/support-azure/)解决问题。

<!-- Update_Description: update meta properties, wording update, update link -->