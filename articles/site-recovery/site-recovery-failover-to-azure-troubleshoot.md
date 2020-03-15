---
title: 对故障转移到 Azure 的故障进行排除 | Azure
description: 本指南介绍如何解决在故障转移到 Azure 中的常见错误
author: rockboyfor
manager: digimobile
ms.service: site-recovery
services: site-recovery
ms.topic: article
ms.workload: storage-backup-recovery
origin.date: 01/08/2020
ms.date: 02/24/2020
ms.author: v-yeche
ms.openlocfilehash: 8f746c775c42352e3e243b51136fe848ae6cc1f8
ms.sourcegitcommit: 3c98f52b6ccca469e598d327cd537caab2fde83f
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/13/2020
ms.locfileid: "79291585"
---
# <a name="troubleshoot-errors-when-failing-over-vmware-vm-or-physical-machine-to-azure"></a>解决将 VMware VM 或物理计算机故障转移到 Azure 时出现的错误

在执行从虚拟机到 Azure 的故障转移时，可能会收到以下错误之一。 若要解决错误，请为每个错误条件使用所述步骤。

## <a name="failover-failed-with-error-id-28031"></a>故障转移失败，错误 ID 为 28031

Site Recovery 无法在 Azure 中创建故障转移的虚拟机。 以下其中一个原因也可能导致此情况的发生：

* 没有足够的配额可用于创建虚拟机：可以通过转到“订阅” -> “使用情况 + 配额”来检查可用配额。 可以打开 [新的支持请求](https://support.azure.cn/support/support-azure/) 来增加此配额。

* 尝试在同一个可用性集中故障转移不同大小系列的虚拟机。 确保在同一个可用性集中选择相同大小系列的所有虚拟机。 可以转到虚拟机的“计算和网络”设置来更改大小，然后重试故障转移。

* 订阅上有一个阻止创建虚拟机的策略。 请更改此策略以允许创建虚拟机，然后重试故障转移。

## <a name="failover-failed-with-error-id-28092"></a>故障转移失败，错误 ID 为 28092

Site Recovery 无法为故障转移的虚拟机创建网络接口。 请确保订阅中有足够的配额来创建网络接口。 可以通过转到“订阅” -> “使用情况 + 配额”来检查可用配额。 可以打开 [新的支持请求](https://support.azure.cn/support/support-azure/) 来增加此配额。 如果你拥有足够的配额，则这可能是一个间歇性的问题，请重试该操作。 如果即使在重试后问题仍然存在，请在本文档结尾处留下注释。  

## <a name="failover-failed-with-error-id-70038"></a>故障转移失败，错误 ID 为 70038

Site Recovery 无法在 Azure 中创建故障转移的经典虚拟机。 这可能是因为：

* 创建虚拟机所需的其中一个资源（如虚拟网络）不存在。 在虚拟机的“计算和网络”设置下创建虚拟网络，或者将设置修改为已经存在的虚拟网络，然后重试故障转移。

## <a name="failover-failed-with-error-id-170010"></a>故障转移失败，错误 ID 为 170010

Site Recovery 无法在 Azure 中创建故障转移的虚拟机。 发生此情况可能是因为本地虚拟机上执行的混合的一个内部活动失败。

若要启动 Azure 中的任何计算机，Azure 环境需要某些驱动程序处于引导启动状态，并要求 DHCP 之类的服务处于自动启动状态。 因此，在进行故障转移时，混合活动会将 **atapi、intelide、storflt、vmbus 和 storvsc 驱动程序**的启动类型转换为引导启动。 它还会将 DHCP 之类的一些服务的启动类型转换为自动启动。 此活动可能会由于环境特定问题而失败。 

若要手动更改 **Windows 来宾 OS** 的驱动程序的启动类型，请执行以下步骤：

1. [下载](https://download.microsoft.com/download/5/D/6/5D60E67C-2B4F-4C51-B291-A97732F92369/Script-no-hydration.ps1)“非混合”脚本并如下所述运行脚本。 此脚本检查 VM 是否需要混合。

    `.\Script-no-hydration.ps1`

    如果需要混合，则它提供以下结果：

        REGISTRY::HKEY_LOCAL_MACHINE\SYSTEM\ControlSet001\services\storvsc           start =  3 expected value =  0

        This system doesn't meet no-hydration requirement.

    如果 VM 满足“非混合”要求，则脚本将提供结果“此系统满足‘非混合’要求”。 在此情况下，所有驱动程序和服务都处于 Azure 所需的状态并且不需要在 VM 上进行混合。

2. 如果 VM 不满足“非混合”要求，请如下所述运行“非混合”脚本。

    `.\Script-no-hydration.ps1 -set`

    这将转换驱动程序的启动类型并提供如下所示的结果：

        REGISTRY::HKEY_LOCAL_MACHINE\SYSTEM\ControlSet001\services\storvsc           start =  3 expected value =  0 

        Updating registry:  REGISTRY::HKEY_LOCAL_MACHINE\SYSTEM\ControlSet001\services\storvsc   start =  0 

        This system is now no-hydration compatible. 

## <a name="unable-to-connectrdpssh-to-the-failed-over-virtual-machine-due-to-grayed-out-connect-button-on-the-virtual-machine"></a>由于虚拟机上的“连接”按钮已灰显，无法连接/通过 RDP/SSH 连接到已故障转移的虚拟机

在 Azure 中，如果故障转移后的 VM 上的“连接”  按钮灰显，并且你未通过快速路由或站点到站点 VPN 连接来连接到 Azure，则执行以下操作：

1. 转到“虚拟机”   > “网络”  ，单击所需网络接口的名称。  ![network-interface](media/site-recovery-failover-to-azure-troubleshoot/network-interface.PNG)
2. 导航到“IP 配置”  ，然后单击所需 IP 配置的名称字段。 ![IPConfigurations](media/site-recovery-failover-to-azure-troubleshoot/IpConfigurations.png)
3. 若要启用公共 IP 地址，请单击“启用”  。 ![启用 IP](media/site-recovery-failover-to-azure-troubleshoot/Enable-Public-IP.png)
4. 单击“配置所需设置”   > “新建”  。 ![新建](media/site-recovery-failover-to-azure-troubleshoot/Create-New-Public-IP.png)
5. 输入公共地址的名称，选择“SKU”  和“分配”  的默认选项，然后单击“确定”  。
6. 现在，单击“保存”  以保存所做的更改。
7. 关闭面板并导航到虚拟机的“概述”  部分以进行连接/通过 RDP 连接。

## <a name="unable-to-connectrdpssh---vm-connect-button-available"></a>无法连接/RDP/SSH - VM“连接”按钮不可用

在 Azure 中，如果故障转移后的 VM 上的“连接”  按钮可用（未灰显），则在虚拟机上检查**启动诊断**并检查是否存在[此文章](../virtual-machines/windows/boot-diagnostics.md)中列出的错误。

1. 如果虚拟机尚未启动，请尝试故障转移到以前的恢复点。
2. 如果虚拟机中的应用程序未启动，请尝试故障转移到应用一致的恢复点。
3. 如果虚拟机已加入域，请确保域控制器正确运行。 可以按照下面给出的步骤执行此操作：

    a. 在同一网络中创建一台新虚拟机。

    b. 确保它能够加入到预期要在其中应启动故障转移的虚拟机的同一域。

    c. 如果域控制器未  正常工作，请尝试使用本地管理员帐户登录到故障转移的虚拟机。
4. 如果使用自定义 DNS 服务器，请确保可以访问该服务器。 可以按照下面给出的步骤执行此操作：

    a. 在同一网络中创建一台新虚拟机并且

    b. 检查虚拟机是否能够使用自定义 DNS 服务器执行名称解析

>[!Note]
>启用除“启动诊断”以外的任何设置，都需要在故障转移之前在虚拟机中安装 Azure VM 代理

<!--Not Available on ## Unable to open serial console after failover of a UEFI based machine into Azure-->

## <a name="unexpected-shutdown-message-event-id-6008"></a>意外的关闭消息（事件 ID 6008）

在故障转移后启动 Windows VM 时，如果在恢复后的 VM 上收到意外的关闭消息，则表明在用于故障转移的恢复点中未捕获 VM 关闭状态。 当恢复到 VM 未完全关闭的时间点时会发生此情况。

通常情况下不需要担心此消息，对于计划外故障转移，通常可以忽略此消息。 如果故障转移是计划内的，请确保 VM 在故障转移前正确关闭，并留出足够的时间供用来将挂起的本地复制数据发送到 Azure。 然后，使用**故障转移**屏幕上的[最新](site-recovery-failover.md#run-a-failover)选项，以便将 Azure 上挂起的任何数据都处理到恢复点中，然后将该恢复点用于 VM 故障转移。

## <a name="unable-to-select-the-datastore"></a>无法选择数据存储

如果在尝试重新保护已经历故障转移的虚拟机时在 Azure 门户中看不到数据存储，则表明存在此问题。 这是因为主目标在添加到 Azure Site Recovery 的 vCenter 下未被视为虚拟机。

若要详细了解如何重新保护虚拟机，请参阅[故障转移到 Azure 后，将计算机重新保护并故障回复到本地站点](vmware-azure-reprotect.md)。

若要解决问题，请执行以下操作：

在用于管理源计算机的 vCenter 中手动创建主目标。 在完成下一个 vCenter 发现和刷新结构操作后，数据存储会变得可用。

> [!Note]
> 
> 这些发现和刷新结构操作可能需要长达 30 分钟的时间才能完成。 

## <a name="linux-master-target-registration-with-cs-fails-with-an-ssl-error-35"></a>Linux 主目标注册到 CS 失败，出现 SSL 错误 35 

由于在主目标上启用了代理身份验证，将 Azure Site Recovery 主目标注册到配置服务器的操作失败。 

在安装日志中出现以下字符串表明存在该错误： 

```
RegisterHostStaticInfo encountered exception config/talwrapper.cpp(107)[post] CurlWrapper Post failed : server : 10.38.229.221, port : 443, phpUrl : request_handler.php, secure : true, ignoreCurlPartialError : false with error: [at curlwrapperlib/curlwrapper.cpp:processCurlResponse:231]   failed to post request: (35) - SSL connect error. 
```

若要解决问题，请执行以下操作：

1. 在配置服务器 VM 上打开命令提示符，使用以下命令验证代理设置：

        cat /etc/environment 
        echo $http_proxy 
        echo $https_proxy 

2. 如果上述命令的输出表明 http_proxy 或 https_proxy 设置已定义，请使用下述方法之一来取消阻止主目标与配置服务器的通信：

    - 下载 [PsExec 工具](https://aka.ms/PsExec)。
    - 使用此工具访问系统用户上下文，确定是否已配置代理地址。 
    - 如果代理已配置，请使用 PsExec 工具在系统用户上下文中打开 IE。

            **psexec -s -i "%programfiles%\Internet Explorer\iexplore.exe"**

    - 若要确保主目标服务器能够与配置服务器通信，请执行以下操作：

        - 在 Internet Explorer 中修改代理设置，通过代理绕过主目标服务器 IP 地址。   
        或
        - 在主目标服务器上禁用代理。 

## <a name="next-steps"></a>后续步骤
- 对[到 Windows VM 的 RDP 连接](../virtual-machines/windows/troubleshoot-rdp-connection.md)进行故障排除
- 对[到 Linux VM 的 SSH 连接](../virtual-machines/linux/detailed-troubleshoot-ssh-connection.md)进行故障排除

如需更多帮助，请在 [Site Recovery 论坛](https://support.azure.cn/support/contact/)提出疑问。 我们的活动社区应能够为你提供帮助。

<!-- Update_Description: update meta properties, wording update, update link -->