---
title: Azure Stack HCI 和 Windows Admin Center 入门
description: 快速连接到现有 Azure Stack HCI 群集并使用 Windows Admin Center 来监视群集和存储性能。
author: WenJason
ms.author: v-jay
ms.topic: article
origin.date: 04/08/2020
ms.date: 05/18/2020
ms.openlocfilehash: 0db994c86352f2aeea9640659403c7b6b898eaba
ms.sourcegitcommit: 134afb420381acd8d6ae56b0eea367e376bae3ef
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/15/2020
ms.locfileid: "83422870"
---
# <a name="get-started-with-azure-stack-hci-and-windows-admin-center"></a>Azure Stack HCI 和 Windows Admin Center 入门

> 适用于：Windows Server 2019

本主题提供有关安装 Windows Admin Center、连接到 Azure Stack HCI 群集以及监视群集和存储性能的说明。

## <a name="install-windows-admin-center"></a>安装 Windows Admin Center

若要安装 Windows Admin Center，最简单的方法是使用本地 Windows 10 电脑，但你必须是本地管理员组的成员才能执行此操作。

1. 从 Microsoft 评估中心下载 [Windows Admin Center](https://www.microsoft.com/evalcenter/evaluate-windows-admin-center)。 尽管它会显示“开始评估”，但此版本是用于生产用途的正式发布版（包含为 Windows Server 许可证的一部分）。
2. 运行 WindowsAdminCenter.msi 文件进行安装。
3. 第一次启动 Windows Admin Center 时，桌面的通知区域中会显示一个图标。 右键单击此图标，然后选择“打开”可在默认浏览器中打开该工具。 在系统提示选择证书时，请确保选择 Windows Admin Center 客户端证书。

## <a name="add-and-connect-to-an-azure-stack-hci-cluster"></a>添加并连接到 Azure Stack HCI 群集

完成 Windows Admin Center 的安装之后，可以从“概述”主页添加要管理的群集。

1. 单击“所有连接”下的“+ 添加”。

    :::image type="content" source="media/get-started/addcluster.png" alt-text="[“添加群集”屏幕截图":::

2. 选择添加 Windows Server 群集：
    
    :::image type="content" source="media/get-started/chooseconnectiontype.png" alt-text="“选择连接类型”屏幕截图":::

3. 键入要管理的群集的名称，然后单击“添加”。 群集将添加到“概述”页上的“连接”列表中。

4. 在“所有连接”下，单击刚添加的群集的名称。 Windows Admin Center 将启动“群集管理器”，并直接转到该群集的 Windows Admin Center 仪表板。

## <a name="monitor-cluster-performance-with-the-windows-admin-center-dashboard"></a>通过 Windows Admin Center 仪表板监视群集性能

Windows Admin Center 仪表板提供有关服务器、驱动器和卷的警报和运行状况信息，以及有关 CPU、内存和存储使用情况的详细信息。 仪表板底部按小时、日、周、月或年显示群集性能信息，如 IOPS 和延迟。

:::image type="content" source="media/get-started/dashboard.png" alt-text="Windows Admin Center 仪表板屏幕截图":::

## <a name="monitor-performance-of-individual-components"></a>监视单个组件的性能

在仪表板左侧的“工具”菜单中，可以向下钻取到群集的任何组件，以查看虚拟机、服务器、卷和驱动器的摘要和清单。

### <a name="virtual-machines"></a>虚拟机

若要查看群集上运行的虚拟机的摘要，请从左侧的“工具”菜单中单击“虚拟机”。

:::image type="content" source="media/get-started/vms-summary.png" alt-text="虚拟机摘要":::

若要查看群集上运行的虚拟机及其状态、主机服务器、CPU 使用率、内存压力、内存需求、分配的内存和运行时间的完整清单，请单击页面顶部的“清单”。

:::image type="content" source="media/get-started/vms-inventory.png" alt-text="虚拟机清单":::

### <a name="servers"></a>服务器

若要查看群集中服务器的摘要，请单击左侧“工具”菜单中的“服务器”。

:::image type="content" source="media/get-started/servers-summary.png" alt-text="服务器摘要":::

若要查看群集中的服务器（包括其状态、运行时间、制造商、型号和序列号）的完整清单，请单击页面顶部的“清单”。

:::image type="content" source="media/get-started/servers-inventory.png" alt-text="服务器清单":::

### <a name="volumes"></a>卷

若要查看群集上的卷的摘要，请单击左侧“工具”菜单中的“卷”。

:::image type="content" source="media/get-started/volumes-summary.png" alt-text="卷摘要":::

若要查看群集上的卷（包括其状态、文件系统、复原能力、大小、存储使用情况和 IOPS）的完整清单，请单击页面顶部的“清单”。

:::image type="content" source="media/get-started/volumes-inventory.png" alt-text="卷清单":::

### <a name="drives"></a>驱动器

若要查看群集中的驱动器的摘要，请单击左侧“工具”菜单中的“驱动器”。

:::image type="content" source="media/get-started/drives-summary.png" alt-text="驱动器摘要":::

若要查看群集中的驱动器及其序列号、状态、型号、大小、类型、使用情况、位置、服务器和容量的完整清单，请单击页面顶部的“清单”。

:::image type="content" source="media/get-started/drives-inventory.png" alt-text="驱动器清单":::

### <a name="virtual-switches"></a>虚拟交换机

若要查看群集中虚拟交换机的设置，请单击左侧“工具”菜单中的“虚拟交换机”，然后单击要显示其设置的虚拟交换机的名称。 Windows Admin Center 将显示与虚拟交换机关联的网络适配器，包括其 IP 地址、连接状态、链接速度和 MAC 地址。

:::image type="content" source="media/get-started/virtual-switch-settings.png" alt-text="虚拟交换机设置":::

## <a name="add-counters-with-the-performance-monitor-tool"></a>使用性能监视器工具添加计数器

使用性能监视器工具可实时查看和比较 Windows、应用或设备的性能计数器。

1. 从左侧的“工具”菜单中选择“性能监视器”。
2. 单击“空白工作区”以启动新工作区，或者单击“还原以前的工作区”以还原以前的工作区。
    :::image type="content" source="media/get-started/performance-monitor.png" alt-text="性能监视器屏幕截图":::
3. 如果要创建新的工作区，请单击“添加计数器”按钮，然后选择要监视的一个或多个源服务器，或选择整个群集。
4. 选择要监视的对象和实例以及计数器和图形类型，以查看动态性能信息。
    :::image type="content" source="media/get-started/example-counter.png" alt-text="示例计数器屏幕截图":::
5. 选择顶部菜单中的“保存”>“另存为”，保存工作区。

## <a name="use-azure-monitor-for-monitoring-and-alerts"></a>将 Azure Monitor 用于监视和警报

还可以使用 [Azure Monitor](https://docs.microsoft.com/windows-server/manage/windows-admin-center/azure/azure-monitor)（需要 Azure 订阅）来收集用于分析和报告的事件与性能计数器，在检测到特定情况时采取措施，并通过电子邮件接收通知。 单击“工具”菜单中的“Azure Monitor”，以便从 Windows Admin Center 直接连接到 Azure。

## <a name="collect-diagnostics-information"></a>收集诊断信息

从“工具”菜单中选择“诊断”以收集用于排查群集问题的信息。 如果你致电 Azure 支持人员，他们可能会要求提供这些信息。

## <a name="next-steps"></a>后续步骤

若要深入了解性能监视，另请参阅：

- [存储空间直通的性能历史记录](https://docs.microsoft.com/windows-server/storage/storage-spaces/performance-history)
- [使用 Azure Monitor 监视 Azure Stack HCI](manage/azure-monitor.md)
