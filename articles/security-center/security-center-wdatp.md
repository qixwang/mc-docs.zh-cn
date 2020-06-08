---
title: Microsoft Defender 高级威胁防护 - Azure 安全中心
description: 本文档介绍 Azure 安全中心与 Microsoft Defender 高级威胁防护之间的集成。
services: security-center
documentationcenter: na
author: memildin
manager: rkarlin
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/14/2020
ms.author: v-tawe
origin.date: 04/07/2020
ms.openlocfilehash: f3295be2da2f80a639f80612cbb70d6928ad37ac
ms.sourcegitcommit: cbaa1aef101f67bd094f6ad0b4be274bbc2d2537
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/28/2020
ms.locfileid: "84126643"
---
# <a name="microsoft-defender-advanced-threat-protection-with-azure-security-center"></a>Microsoft Defender 高级威胁防护与 Azure 安全中心

Azure 安全中心通过与 [Microsoft Defender 高级威胁防护](https://www.microsoft.com/microsoft-365/windows/microsoft-defender-atp) (ATP) 相集成来扩展其云工作负载保护平台产品/服务。
此项更改带来了全面的终结点检测和响应 (EDR) 功能。 使用 Microsoft Defender ATP 集成可以查明异常。 还可以检测和响应 Azure 安全中心所监视的服务器终结点上出现的高级攻击。

## <a name="microsoft-defender-atp-features-in-security-center"></a>安全中心的 Microsoft Defender ATP 功能

使用 Microsoft Defender ATP 时，你可获得：

- **高级入侵后检测传感器**：Windows 服务器的 Microsoft Defender ATP 传感器收集各种各样的行为信号。

- **基于分析的、由云提供支持的入侵后检测**：Microsoft Defender ATP 可快速应对不断变化的威胁。 它使用高级分析和大数据。 Microsoft Defender ATP 借助 Intelligent Security Graph 的强大功能得以增强，并结合 Windows、Azure 和 Office 中的信号来检测未知威胁。 它提供可以采取措施的警报，并可让你快速做出响应。

- **威胁智能**：Microsoft Defender ATP 在识别攻击者工具、方法和过程时生成警报。 它使用 Microsoft 威胁猎人和安全团队生成的，并由合作伙伴提供的情报补充的数据。

以下功能现已在 Azure 安全中心推出：

- **自动加入**：对于加入 Azure 安全中心的 Windows 服务器，将自动启用 Microsoft Defender ATP 传感器（运行 Windows Server 2019 的服务器除外）。

- **单一虚拟管理平台**：Azure 安全中心控制台显示 Microsoft Defender ATP 警报。

若要进一步调查，请使用 Microsoft Defender ATP。 Microsoft Defender ATP 提供其他信息，例如警报进程树和事件图形。 此外，还可以看到详细的机器时间线，其中显示了最长六个月的历史时段的每种行为。

![Microsoft Defender ATP 页，其中包含有关警报的详细信息](media/security-center-wdatp/image3.png)

## <a name="platform-support"></a>平台支持

安全中心的 Microsoft Defender ATP 支持在 Windows Server 2016、2012 R2 和 2008 R2 SP1 上进行检测；对于 Azure VM，需要标准层订阅，对于非 Azure VM，仅需要在工作区级别中的标准层。

> [!NOTE]
> 在你使用 Azure 安全中心监视服务器时，系统自动创建 Microsoft Defender ATP 租户，并且 Microsoft Defender ATP 数据默认存储在欧洲。 如果需要将数据移动到另一个位置，则需要联系 Microsoft 支持部门重置租户。 已为 Office 365 GCC 客户禁用了利用此集成的服务器终结点监视。

## <a name="onboarding-servers-to-security-center"></a>将服务器加入安全中心 

若要将服务器加入到安全中心，请从“Microsoft Defender ATP 服务器加入”单击“转到 Azure 安全中心加入服务器”。

1. 在“加入”区域中选择或创建用于存储数据的工作区。 <br>
2. 如果看不到所有工作区，可能是由于缺少权限，请确保已将工作区设置到“Azure 安全标准”层。 有关详细信息，请参阅[升级到安全中心的标准层以增强安全性](security-center-pricing.md)。
    
3. 选择“添加服务器”以查看有关如何安装 Log Analytics 代理的说明。 

4. 加入后，可以在“计算和应用”下监视计算机。

   ![加入计算机](media/security-center-wdatp/onboard-computers.png)

## <a name="enable-microsoft-defender-atp-integration"></a>启用 Microsoft Defender ATP 集成

若要查看是否已启用 Microsoft Defender ATP 集成，请选择“安全中心” > “定价与设置”> 单击你的订阅 。
在此处可以查看当前已启用的集成。

  ![Azure 安全中心威胁检测设置页，其中显示已启用 Microsoft Defender ATP 集成](media/security-center-wdatp/enable-integrations.png)

- 如果已将服务器载入 Azure 安全中心标准层，则不需要执行进一步的操作。 Azure 安全中心会自动将服务器加入 Microsoft Defender ATP。 加入过程可能最多需要 24 小时。

- 如果你从未将服务器载入 Azure 安全中心标准层，请照常将它们载入 Azure 安全中心。

- 如果通过 Windows Defender ATP 加入了服务器：
  - 请参阅有关[如何卸载服务器计算机](https://go.microsoft.com/fwlink/p/?linkid=852906)的指南文档。
  - 将这些服务器载入 Azure 安全中心。

## <a name="access-to-the-microsoft-defender-atp-portal"></a>访问 Microsoft Defender ATP 门户

遵照[为用户分配门户访问权限](https://docs.microsoft.com/windows/security/threat-protection/microsoft-defender-atp/assign-portal-access)中的说明操作。

## <a name="set-the-firewall-configuration"></a>设置防火墙配置

如果当 Microsoft Defender ATP 传感器从系统上下文建立连接时代理或防火墙阻止了匿名流量，请确保允许匿名流量。 遵照[在代理服务器中启用对 Microsoft Defender ATP 服务 URL 的访问](https://docs.microsoft.com/windows/security/threat-protection/microsoft-defender-atp/configure-proxy-internet#enable-access-to-microsoft-defender-atp-service-urls-in-the-proxy-server)中的说明操作。

## <a name="test-the-feature"></a>测试功能

生成良性的 Microsoft Defender ATP 测试警报：

1. 创建文件夹“C:\test-MDATP-test”。

1. 使用远程桌面访问 Windows Server 2012 R2 VM 或 Windows Server 2016 VM。 打开命令行窗口。

1. 在提示符下，复制并运行以下命令。 命令提示符窗口将自动关闭。

    ```
    powershell.exe -NoExit -ExecutionPolicy Bypass -WindowStyle Hidden (New-Object System.Net.WebClient).DownloadFile('http://127.0.0.1/1.exe', 'C:\\test-MDATP-test\\invoice.exe'); Start-Process 'C:\\test-MDATP-test\\invoice.exe'
    ```

   ![包含上述命令的命令提示符窗口](media/security-center-wdatp/image4.jpeg)

3. 如果该命令成功，则 Azure 安全中心仪表板和 Microsoft Defender ATP 门户中会显示一条新警报。 此警报可能要在几分钟之后才显示。

4. 若要在安全中心查看该警报，请转到“安全警报” > “可疑的 PowerShell 命令行” 。

5. 在调查窗口中，选择相应的链接转到 Microsoft Defender ATP 门户。

## <a name="next-steps"></a>后续步骤

- [Platforms and features supported by Azure Security Center](security-center-os-coverage.md)（Azure 安全中心支持的平台和功能）
- [在 Azure 安全中心设置安全策略](tutorial-security-policy.md)：了解如何为 Azure 订阅和资源组配置安全策略。
- [管理 Azure 安全中心的安全建议](security-center-recommendations.md)：了解建议如何帮助你保护 Azure 资源。
- [在 Azure 安全中心进行安全运行状况监视](security-center-monitoring.md)：了解如何监视 Azure 资源的运行状况。
