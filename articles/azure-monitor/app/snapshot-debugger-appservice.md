---
title: 在 Azure 应用服务中为 .NET 应用启用 Snapshot Debugger | Microsoft Docs
description: 在 Azure 应用服务中为 .NET 应用启用快照调试器
ms.topic: conceptual
ms.reviewer: mbullwin
author: lingliw
manager: digimobile
origin.date: 03/07/2019
ms.date: 6/4/2019
ms.author: v-lingwu
ms.openlocfilehash: 5598d7af79a79c0a3a58f0ec898fb8cbe9b2e47f
ms.sourcegitcommit: b7fe28ec2de92b5befe61985f76c8d0216f23430
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/06/2020
ms.locfileid: "78850374"
---
# <a name="enable-snapshot-debugger-for-net-apps-in-azure-app-service"></a>在 Azure 应用服务中为 .NET 应用启用快照调试器

快照调试器当前适用于按 Windows 服务计划在 Azure 应用服务上运行的 ASP.NET 和 ASP.NET Core 应用。

<a name="installation"></a>
##  <a name="enable-snapshot-debugger"></a>启用快照调试器
若要为应用启用快照调试器，请遵循下面的说明。 如果你在运行另一种类型的 Azure 服务，则下面提供了用于在其他受支持平台上启用快照调试器的说明：
* [Azure 云服务](snapshot-debugger-vm.md?toc=/azure/azure-monitor/toc.json)
* [Azure Service Fabric 服务](snapshot-debugger-vm.md?toc=/azure/azure-monitor/toc.json)
* [Azure 虚拟机和虚拟机规模集](snapshot-debugger-vm.md?toc=/azure/azure-monitor/toc.json)
* [本地虚拟机或物理计算机](snapshot-debugger-vm.md?toc=/azure/azure-monitor/toc.json)

如果使用的是 .NET Core 预览版，请按照[为其他环境启用 Snapshot Debugger 的说明](snapshot-debugger-vm.md?toc=/azure/azure-monitor/toc.json)首先将 [Microsoft.ApplicationInsights.SnapshotCollector](https://www.nuget.org/packages/Microsoft.ApplicationInsights.SnapshotCollector) NuGet 包包含在应用程序中，然后完成下面的其余说明。 

预安装 Application Insights 快照调试器作为应用程序服务运行时的一部分，但需启用它才能获得适用于应用服务应用的快照。 部署应用后，即使在源代码中包括了 Application Insights SDK，也要执行以下步骤来启用快照调试器。

1.  转到 Azure 门户中的“应用服务”窗格。
2. 导航到“设置”>“Application Insights”窗格  。

   ![在应用服务门户上启用 App Insights](./media/snapshot-debugger/applicationinsights-appservices.png)

3. 按窗格中的说明创建新资源，或者选择现有的 App Insights 资源，以便监视应用。 另外，请确保快照调试器的两个开关都为“开”  。

   ![添加 App Insights 站点扩展][Enablement UI]

4. 现已使用应用服务应用设置启用了快照调试器。

    ![快照调试器的应用设置][snapshot-debugger-app-setting]

## <a name="disable-snapshot-debugger"></a>禁用快照调试器

执行与**启用快照调试器**相同的步骤，但将快照调试器的两个开关都切换到“关”  。
我们建议在所有应用上启用快照调试器，以简化对应用程序异常的诊断。

## <a name="next-steps"></a>后续步骤

- 为应用程序生成可触发异常的流量。 然后等待 10 到 15 分钟，这样快照就会发送到 Application Insights 实例。
- 请参见 Azure 门户中的[快照](snapshot-debugger.md?toc=/azure/azure-monitor/toc.json#view-snapshots-in-the-portal)。
- 排查 Snapshot Debugger 问题时如需帮助，请参阅 [ Snapshot Debugger 故障排除](snapshot-debugger-troubleshoot.md?toc=/azure/azure-monitor/toc.json)。

[Enablement UI]: ./media/snapshot-debugger/enablement-ui.png
[snapshot-debugger-app-setting]:./media/snapshot-debugger/snapshot-debugger-app-setting.png





