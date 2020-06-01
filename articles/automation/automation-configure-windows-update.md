---
title: 配置 Windows 更新设置以使用 Azure 更新管理
description: 本文介绍为使用 Azure 更新管理而需要配置的 Windows 更新设置。
services: automation
ms.subservice: update-management
origin.date: 05/04/2020
ms.date: 05/25/2020
ms.topic: conceptual
ms.openlocfilehash: a9c070255b2092cd2a5cb3054247f7525503d15e
ms.sourcegitcommit: 981a75a78f8cf74ab5a76f9e6b0dc5978387be4b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/22/2020
ms.locfileid: "83801916"
---
# <a name="configure-windows-update-settings-for-update-management"></a>为更新管理配置 Windows 更新客户端设置

Azure 更新管理依赖使用 [Windows 更新客户端](https://docs.microsoft.com/windows/deployment/update/windows-update-overview)来下载和安装 Windows 更新。 在连接到 Windows Server Update Services (WSUS) 或 Windows 更新时，Windows 更新客户端将使用特定的设置。 其中的许多设置可以通过以下方式进行管理：

- 本地组策略编辑器
- 组策略
- PowerShell
- 直接编辑注册表

更新管理遵循许多指定用于控制 Windows 更新客户端的设置。 如果使用设置来启用非 Windows 更新，更新管理也将管理这些更新。 如果你想要在更新部署发生之前启用更新下载，更新部署可以更快、更有效，且不太可能会超过维护时段。

有关在 Azure 订阅中设置 WSUS 并安全地使 Windows 虚拟机保持最新状态的其他建议，请查看[使用 WSUS 规划部署以在 Azure 中更新 Windows 虚拟机](https://docs.microsoft.com/azure/architecture/example-scenario/wsus/)。

## <a name="pre-download-updates"></a>下载前的更新

若要配置更新自动下载但不自动安装更新，可以使用组策略将“[配置自动更新设置](/windows-server/administration/windows-server-update-services/deploy/4-configure-group-policy-settings-for-automatic-updates##configure-automatic-updates)”设置为 **3**。 此设置启用在后台下载所需的更新，并在准备好安装更新时发出通知。 这样，更新管理仍受计划的控制，但可以在更新管理维护时段以外下载更新。 此行为可以防止更新管理中出现“已超过维护时段”错误。

可运行以下命令，使用 PowerShell 启用此设置：

```powershell
$WUSettings = (New-Object -com "Microsoft.Update.AutoUpdate").Settings
$WUSettings.NotificationLevel = 3
$WUSettings.Save()
```

## <a name="configure-reboot-settings"></a>配置重启设置

即使在“更新部署”设置中指定了“永不重启”，[通过编辑注册表来配置自动更新](https://docs.microsoft.com/windows/deployment/update/waas-wu-settings#configuring-automatic-updates-by-editing-the-registry)以及[用于管理重启的注册表项](/windows/deployment/update/waas-restart#registry-keys-used-to-manage-restart)中列出的注册表项也可能导致计算机重启。  请配置这些注册表项，使其最适合你的环境。

## <a name="enable-updates-for-other-microsoft-products"></a>启用其他 Microsoft 产品的更新

默认情况下，Windows 更新客户端配置为仅提供适用于 Windows 的更新。 如果启用“更新 Windows 时提供其他 Microsoft 产品的更新”设置，则还会收到其他产品的更新，包括 Microsoft SQL Server 和其他 Microsoft 软件的安全修补程序。 如果已下载并复制适用于 Windows 2016 和更高版本的最新[管理模板文件](https://support.microsoft.com/help/3087759/how-to-create-and-manage-the-central-store-for-group-policy-administra)，则可以配置此选项。

如果运行的是 Windows Server 2012 R2，则组策略无法配置此设置。 在这些计算机上运行以下 PowerShell 命令。 更新管理符合此设置。

```powershell
$ServiceManager = (New-Object -com "Microsoft.Update.ServiceManager")
$ServiceManager.Services
$ServiceID = "7971f918-a847-4430-9279-4a52d1efe18d"
$ServiceManager.AddService2($ServiceId,7,"")
```

## <a name="wsus-configuration-settings"></a>WSUS 配置设置

更新管理支持 WSUS 设置。 下面列出了为使用更新管理而可以配置的 WSUS 设置。

### <a name="intranet-microsoft-update-service-location"></a>Intranet Microsoft 更新服务位置

可以在“[指定 Intranet Microsoft 更新服务位置](https://docs.microsoft.com/windows/deployment/update/waas-wu-settings#specify-intranet-microsoft-update-service-location)”下指定用于扫描和下载更新的源。 默认情况下，Windows 更新客户端配置为从 Windows 更新下载更新。 将某台 WSUS 服务器指定为计算机的源时，如果更新未在 WSUS 中获批准，则更新部署将会失败。 

若要将计算机限制为该内部更新服务，请配置“[不要连接到任何 Windows 更新 Internet 位置](https://docs.microsoft.com/windows-server/administration/windows-server-update-services/deploy/4-configure-group-policy-settings-for-automatic-updates#do-not-connect-to-any-windows-update-internet-locations)”。 

## <a name="next-steps"></a>后续步骤

配置 Windows 更新设置后，可以按照[管理 Azure VM 的更新和修补程序](automation-tutorial-update-management.md)中的说明来计划更新部署。
