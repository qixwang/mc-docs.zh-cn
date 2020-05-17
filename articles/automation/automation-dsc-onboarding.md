---
title: 加入 Azure Automation State Configuration 管理的计算机
description: 如何设置通过 Azure Automation State Configuration 进行管理的计算机
services: automation
ms.service: automation
ms.subservice: dsc
author: WenJason
ms.author: v-jay
ms.topic: conceptual
origin.date: 12/10/2019
ms.date: 05/11/2020
manager: digimobile
ms.openlocfilehash: 935b21951adb3bf38d889af5092ff3629ced0f62
ms.sourcegitcommit: 7443ff038ea8afe511f7419d9c550d27fb642246
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/09/2020
ms.locfileid: "83001637"
---
# <a name="onboarding-machines-for-management-by-azure-automation-state-configuration"></a>加入 Azure Automation State Configuration 管理的计算机

## <a name="why-manage-machines-with-azure-automation-state-configuration"></a>为何要使用 Azure 自动化状态配置来管理计算机？

Azure Automation State Configuration 是一个配置管理服务，适用于任何云或本地数据中心内的 Desired State Configuration (DSC) 节点。 可以通过在 Azure 门户中的“配置管理”下选择“State Configuration (DSC)”来访问它。   

使用此服务可从中心的安全位置跨数千台计算机快速轻松进行缩放。 可以轻松加入计算机、为其分配声明性配置并查看显示每台计算机是否符合指定的所需状态的报告。

适用于 DSC 的 Azure Automation State Configuration 服务类似于 PowerShell 脚本中的 Azure 自动化 Runbook。 换句话说，Azure 自动化以帮助你管理 PowerShell 脚本的相同方式帮助你管理 DSC 配置。 若要详细了解使用 Azure Automation State Configuration 的好处，请参阅 [Azure Automation State Configuration 概述](automation-dsc-overview.md)。

Azure Automation State Configuration 可用于管理各种不同的计算机：

- Azure 虚拟机
- Azure 虚拟机（经典）
- 位于本地或 Azure 以外的云中的物理/虚拟 Windows 计算机
- 位于本地、Azure 或 Azure 以外的云中的物理/虚拟 Linux 计算机

如果你未准备好从云管理计算机配置，可以使用 Azure Automation State Configuration 作为仅限报告的终结点。 使用此功能可以通过 DSC 设置（推送）配置，以及在 Azure 自动化中查看报告详细信息。

> [!NOTE]
> 如果安装的 Azure VM Desired State Configuration 扩展版本高于 2.70，则无需支付额外的费用即可使用 Azure Automation State Configuration 来管理 Azure VM。 有关详细信息，请参阅[**自动化定价页**](https://azure.cn/pricing/details/automation/)。

本文的以下各部分概述了如何将上面列出的计算机加入到 Azure Automation State Configuration。

## <a name="onboarding-azure-vms"></a>加入 Azure VM

Azure Automation State Configuration 可让你使用 Azure 门户、Azure 资源管理器模板或 PowerShell 轻松加入 Azure VM 以进行配置管理。 在幕后，在不需要管理员远程连接到 VM 的情况下，Azure VM Desired State Configuration 扩展会在 Azure Automation State Configuration 中注册 VM。 由于 Azure 扩展以异步方式运行，本文的[排查 Azure 虚拟机加入问题](#troubleshooting-azure-virtual-machine-onboarding)部分提供了跟踪其注册进度或对其进行故障排除的步骤。

> [!NOTE]
>将 DSC 部署到 Linux 节点时会使用 **/tmp** 文件夹。 在将 `nxautomation` 等模块安装到相应的位置之前，会临时下载这些模块进行验证。 为确保正确安装模块，适用于 Linux 的 Log Analytics 代理需要对 **/tmp** 文件夹拥有读/写权限。<br><br>
>适用于 Linux 的 Log Analytics 代理以 `omsagent` 用户身份运行。 若要向 `omsagent` 用户授予写入权限，请运行命令 `setfacl -m u:omsagent:rwx /tmp`。

### <a name="onboard-a-vm-using-azure-portal"></a>使用 Azure 门户加入 VM

若要通过 [Azure 门户](https://portal.azure.cn/)将 Azure VM 加入到 Azure Automation State Configuration，请执行以下操作：

1. 导航到要在其中加入 VM 的 Azure 自动化帐户。 

2. 在“State Configuration”页上，选择“节点”选项卡，然后单击“添加”。  

3. 选择要加入的 VM。

4. 如果虚拟机未安装 PowerShell 所需状态扩展且电源状态为“正在运行”，请单击“连接”  。

5. 在“注册”下，输入用例所需的 [PowerShell DSC 本地配置管理器值](https://docs.microsoft.com/powershell/scripting/dsc/managing-nodes/metaConfig)。  （可选）可以输入要分配给 VM 的节点配置。

![加入](./media/automation-dsc-onboarding/DSC_Onboarding_6.png)

### <a name="onboard-a-vm-using-azure-resource-manager-templates"></a>使用 Azure 资源管理器模板加入 VM

可以使用 Azure 资源管理器模板部署 VM 并将其加入到 Azure Automation State Configuration。 有关将现有 VM 加入 Azure Automation State Configuration 的示例模板，请参阅 [Desired State Configuration 管理的服务器](https://azure.microsoft.com/resources/templates/101-automation-configuration/)。 如果你正在管理虚拟机规模集，请参阅 [Azure 自动化管理的虚拟机规模集配置](https://azure.microsoft.com/resources/templates/201-vmss-automation-dsc/)中的示例模板。

### <a name="onboard-machines-using-powershell"></a>使用 PowerShell 加入计算机

可以在 PowerShell 中使用 [Register-AzAutomationDscNode](https://docs.microsoft.com/powershell/module/az.automation/register-azautomationdscnode) cmdlet 将 VM 加入到 Azure Automation State Configuration。 

> [!NOTE]
>`Register-AzAutomationDscNode` cmdlet 目前仅针对运行 Windows 的计算机进行了实现，因为该 cmdlet 仅触发 Windows 扩展。

### <a name="register-vms-across-azure-subscriptions"></a>跨 Azure 订阅注册 VM

注册其他 Azure 订阅中的 VM 的最佳方法是使用 Azure 资源管理器部署模板中的 DSC 扩展。 [Desired State Configuration 扩展与 Azure 资源管理器模板](/virtual-machines/extensions/dsc-template)中提供了示例。

若要查找在模板中用作参数的注册密钥和注册 URL，请参阅文本中的[使用注册安全加入](#onboarding-securely-using-registration)部分。

## <a name="onboarding-physicalvirtual-windows-machines-on-premises-or-in-a-cloud-other-than-azure"></a>加入位于本地或 Azure 以外的云中的物理/虚拟 Windows 计算机

可将本地运行的或在其他云环境中运行的 Windows 服务器加入到 Azure Automation State Configuration。 这些服务器必须能够[对 Azure 进行出站访问](automation-dsc-overview.md#network-planning)。

1. 确保已在要加入到 Azure Automation State Configuration 的计算机上安装最新版本的 [WMF 5](https://aka.ms/wmf5latest)。 此外，WMF 5 还必须安装在用于执行加入操作的计算机上。
1. 根据[生成 DSC 元配置](#generating-dsc-metaconfigurations)部分中的指导创建包含所需 DSC 元配置的文件夹。 
1. 使用以下 cmdlet 将 PowerShell DSC 元配置远程应用到要加入的计算机。 

   ```powershell
   Set-DscLocalConfigurationManager -Path C:\Users\joe\Desktop\DscMetaConfigs -ComputerName MyServer1, MyServer2
   ```

1. 如果无法远程应用 PowerShell DSC 元配置，请将 **metaconfigurations** 文件夹复制到要加入的计算机。 然后添加用于在计算机本地调用 [Set-DscLocalConfigurationManager](https://docs.microsoft.com/powershell/module/psdesiredstateconfiguration/set-dsclocalconfigurationmanager?view=powershell-5.1) 的代码。
1. 使用 Azure 门户或 cmdlet 验证要加入的计算机是否在 Azure 自动化帐户中显示为已注册的 State Configuration 节点。

## <a name="onboarding-physicalvirtual-linux-machines-on-premises-or-in-a-cloud-other-than-azure"></a>加入位于本地或者 Azure 以外的云中的物理/虚拟 Linux 计算机

可将本地运行的或在其他云环境中运行的 Linux 服务器加入到 Azure Automation State Configuration。 这些服务器必须能够[对 Azure 进行出站访问](automation-dsc-overview.md#network-planning)。

1. 确保已在希望加入到 Azure Automation State Configuration 的计算机上安装[适用于 Linux 的 PowerShell Desired State Configuration](https://github.com/Microsoft/PowerShell-DSC-for-Linux) 的最新版本。
2. 如果 [PowerShell DSC 本地配置管理器默认值](https://docs.microsoft.com/powershell/scripting/dsc/managing-nodes/metaConfig4)与用例匹配，并且你想要加入计算机，使其从/向 Azure Automation State Configuration 拉取/报告信息，请执行以下操作：

   - 在要加入到 Azure Automation State Configuration 的每台 Linux 计算机上，通过 `Register.py` 使用 PowerShell DSC 本地配置管理器默认值执行加入。

     `/opt/microsoft/dsc/Scripts/Register.py <Automation account registration key> <Automation account registration URL>`

   - 若要查找自动化帐户的注册密钥和注册 URL，请参阅本文的[使用注册安全加入](#onboarding-securely-using-registration)部分。

3. 如果 PowerShell DSC 本地配置管理器 (LCM) 默认值与用例不匹配，或者你想要加入仅向 Azure Automation State Configuration 报告的计算机，请执行步骤 4-7。 否则，请直接转到步骤 7。

4. 根据[生成 DSC 元配置](#generating-dsc-metaconfigurations)部分中的指导生成包含所需 DSC 元配置的文件夹。

5. 确保在用于加入的计算机上安装了最新版本的 [WMF 5](https://aka.ms/wmf5latest)。

6. 添加以下代码以将 PowerShell DSC 元配置远程应用到要加入的计算机。

    ```powershell
    $SecurePass = ConvertTo-SecureString -String '<root password>' -AsPlainText -Force
    $Cred = New-Object System.Management.Automation.PSCredential 'root', $SecurePass
    $Opt = New-CimSessionOption -UseSsl -SkipCACheck -SkipCNCheck -SkipRevocationCheck

    # need a CimSession for each Linux machine to onboard
    $Session = New-CimSession -Credential $Cred -ComputerName <your Linux machine> -Port 5986 -Authentication basic -SessionOption $Opt

    Set-DscLocalConfigurationManager -CimSession $Session -Path C:\Users\joe\Desktop\DscMetaConfigs
    ```

7. 如果无法远程应用 PowerShell DSC 元配置，请将对应于远程计算机的元配置从步骤 4 中所述的文件夹复制到 Linux 计算机。

8. 添加代码以在每台要加入到 Azure Automation State Configuration 的 Linux 计算机上本地调用 `Set-DscLocalConfigurationManager.py`。

   `/opt/microsoft/dsc/Scripts/SetDscLocalConfigurationManager.py -configurationmof <path to metaconfiguration file>`

9. 使用 Azure 门户或 cmdlet 确保要加入的计算机现在在 Azure 自动化帐户中显示为已注册的 DSC 节点。

## <a name="generating-dsc-metaconfigurations"></a>生成 DSC 元配置

若要将任何计算机加入到 Azure Automation State Configuration，可以生成 [DSC 元配置](https://docs.microsoft.com/powershell/scripting/dsc/managing-nodes/metaConfig)。 此配置告知 DSC 代理要从 Azure Automation State Configuration 拉取信息和/或向其报告。 可以使用 PowerShell DSC 配置或 Azure 自动化 PowerShell cmdlet 来生成 Azure Automation State Configuration 的 DSC 元配置。

> [!NOTE]
> DSC 元配置包含将计算机登记到进行管理的自动化帐户的机密。 请务必适当保护所创建的任何 DSC 元配置，或者在使用后将其删除。

元配置的代理支持由 LCM（Windows PowerShell DSC 引擎）控制。 LCM 在所有目标节点上运行，负责调用 DSC 元配置脚本中包含的配置资源。 可以通过在 `ConfigurationRepositoryWeb`、`ResourceRepositoryWeb` 和 `ReportServerWeb` 块中按需包含代理 URL 和代理凭据的定义，在元配置中包含代理支持。 请参阅[配置本地配置管理器](https://docs.microsoft.com/powershell/scripting/dsc/managing-nodes/metaconfig?view=powershell-7)。

### <a name="generate-dsc-metaconfigurations-using-a-dsc-configuration"></a>使用 DSC 配置生成 DSC 元配置

1. 在本地环境中，以计算机管理员身份打开 VSCode（或偏好的编辑器）。 计算机上必须已安装最新版本的 [WMF 5](https://aka.ms/wmf5latest) 。
1. 在本地复制以下脚本。 此脚本包含用于创建元配置的 PowerShell DSC 配置，以及用于开始执行元配置创建操作的命令。

    > [!NOTE]
    > State Configuration 节点配置名称在 Azure 门户中区分大小写。 如果大小写不匹配，节点将不会显示在“节点”选项卡下。 

   ```powershell
   # The DSC configuration that will generate metaconfigurations
   [DscLocalConfigurationManager()]
   Configuration DscMetaConfigs
   {
        param
        (
            [Parameter(Mandatory=$True)]
            [String]$RegistrationUrl,

            [Parameter(Mandatory=$True)]
            [String]$RegistrationKey,

            [Parameter(Mandatory=$True)]
            [String[]]$ComputerName,

            [Int]$RefreshFrequencyMins = 30,

            [Int]$ConfigurationModeFrequencyMins = 15,

            [String]$ConfigurationMode = 'ApplyAndMonitor',

            [String]$NodeConfigurationName,

            [Boolean]$RebootNodeIfNeeded= $False,

            [String]$ActionAfterReboot = 'ContinueConfiguration',

            [Boolean]$AllowModuleOverwrite = $False,

            [Boolean]$ReportOnly
        )

        if(!$NodeConfigurationName -or $NodeConfigurationName -eq '')
        {
            $ConfigurationNames = $null
        }
        else
        {
            $ConfigurationNames = @($NodeConfigurationName)
        }

        if($ReportOnly)
        {
            $RefreshMode = 'PUSH'
        }
        else
        {
            $RefreshMode = 'PULL'
        }

        Node $ComputerName
        {
            Settings
            {
                RefreshFrequencyMins           = $RefreshFrequencyMins
                RefreshMode                    = $RefreshMode
                ConfigurationMode              = $ConfigurationMode
                AllowModuleOverwrite           = $AllowModuleOverwrite
                RebootNodeIfNeeded             = $RebootNodeIfNeeded
                ActionAfterReboot              = $ActionAfterReboot
                ConfigurationModeFrequencyMins = $ConfigurationModeFrequencyMins
            }

            if(!$ReportOnly)
            {
            ConfigurationRepositoryWeb AzureAutomationStateConfiguration
                {
                    ServerUrl          = $RegistrationUrl
                    RegistrationKey    = $RegistrationKey
                    ConfigurationNames = $ConfigurationNames
                }

                ResourceRepositoryWeb AzureAutomationStateConfiguration
                {
                    ServerUrl       = $RegistrationUrl
                    RegistrationKey = $RegistrationKey
                }
            }

            ReportServerWeb AzureAutomationStateConfiguration
            {
                ServerUrl       = $RegistrationUrl
                RegistrationKey = $RegistrationKey
            }
        }
   }

    # Create the metaconfigurations
    # NOTE: DSC Node Configuration names are case sensitive in the portal.
    # TODO: edit the below as needed for your use case
   $Params = @{
        RegistrationUrl = '<fill me in>';
        RegistrationKey = '<fill me in>';
        ComputerName = @('<some VM to onboard>', '<some other VM to onboard>');
        NodeConfigurationName = 'SimpleConfig.webserver';
        RefreshFrequencyMins = 30;
        ConfigurationModeFrequencyMins = 15;
        RebootNodeIfNeeded = $False;
        AllowModuleOverwrite = $False;
        ConfigurationMode = 'ApplyAndMonitor';
        ActionAfterReboot = 'ContinueConfiguration';
        ReportOnly = $False;  # Set to $True to have machines only report to AA DSC but not pull from it
   }

   # Use PowerShell splatting to pass parameters to the DSC configuration being invoked
   # For more info about splatting, run: Get-Help -Name about_Splatting
   DscMetaConfigs @Params
   ```

1. 填写自动化帐户的注册密钥和 URL，以及要登记的计算机名称。 所有其他参数都是可选的。 若要查找自动化帐户的注册密钥和注册 URL，请参阅[使用注册安全加入](#onboarding-securely-using-registration)部分。

1. 如果希望计算机向 Azure Automation State Configuration 报告 DSC 状态信息但不拉取配置或 PowerShell 模块，请将 `ReportOnly` 参数设置为 true。

1. 如果未设置 `ReportOnly`，计算机将向 Azure Automation State Configuration 报告 DSC 状态信息并拉取配置或 PowerShell 模块。 在 `ConfigurationRepositoryWeb`、`ResourceRepositoryWeb` 和 `ReportServerWeb` 块中相应地设置参数。

1. 运行该脚本。 现在，你（作为管理员）应已获得一个名为 **DscMetaConfigs** 的工作目录文件夹，其中包含要加入的计算机的 PowerShell DSC 元配置。

    ```powershell
    Set-DscLocalConfigurationManager -Path ./DscMetaConfigs
    ```

### <a name="generate-dsc-metaconfigurations-using-azure-automation-cmdlets"></a>使用 Azure 自动化 cmdlet 生成 DSC 元配置

如果 PowerShell DSC LCM 默认值与用例匹配，并且你想要加入计算机以便从 Azure Automation State Configuration 拉取信息并向其报告，可以使用 Azure 自动化 cmdlet 更轻松地生成所需的 DSC 元配置。

1. 在本地环境中，以计算机管理员身份打开 PowerShell 控制台或 VSCode。
2. 使用 [Connect-AzAccount](https://docs.microsoft.com/powershell/module/Az.Accounts/Connect-AzAccount?view=azps-3.7.0) 连接到 Azure 资源管理器。
3. 从你正在设置节点的自动化帐户中下载要加入的计算机的 PowerShell DSC 元配置。

   ```powershell
   # Define the parameters for Get-AzAutomationDscOnboardingMetaconfig using PowerShell Splatting
   $Params = @{
       ResourceGroupName = 'ContosoResources'; # The name of the Resource Group that contains your Azure Automation account
       AutomationAccountName = 'ContosoAutomation'; # The name of the Azure Automation account where you want a node on-boarded to
       ComputerName = @('web01', 'web02', 'sql01'); # The names of the computers that the metaconfiguration will be generated for
       OutputFolder = "$env:UserProfile\Desktop\";
   }
   # Use PowerShell splatting to pass parameters to the Azure Automation cmdlet being invoked
   # For more info about splatting, run: Get-Help -Name about_Splatting
   Get-AzAutomationDscOnboardingMetaconfig @Params
   ```

1. 现在，你（作为管理员）应已获得一个 **DscMetaConfigs** 文件夹，其中包含要加入的计算机的 PowerShell DSC 元配置。

    ```powershell
    Set-DscLocalConfigurationManager -Path $env:UserProfile\Desktop\DscMetaConfigs
    ```

## <a name="onboarding-securely-using-registration"></a>使用注册安全地加入

可以通过 WMF 5 DSC 注册协议将计算机安全地加入到 Azure 自动化帐户。 此协议允许 DSC 节点对 PowerShell DSC 拉取或报告服务器（包括 Azure Automation State Configuration）进行身份验证。 节点将通过注册 URL 注册到服务器，并使用注册密钥进行身份验证。 在注册期间，DSC 节点和 DSC 拉取/报告服务器会协商唯一证书，在注册后，节点将使用该证书对服务器进行身份验证。 此过程可防止加入的节点相互模拟，例如当节点遭到入侵并出现恶意行为时。 注册后，注册密钥不再用于身份验证，而是从节点中删除。

可以从 Azure 门户中“帐户设置”  下的“密钥”  中获取 State Configuration 注册协议所需的信息。 

![Azure 自动化密钥和 URL](./media/automation-dsc-onboarding/DSC_Onboarding_4.png)

- 注册 URL 是“密钥”页上的“URL”字段。
- 注册密钥是“密钥”页上“主访问密钥”字段或“辅助访问密钥”字段的值。   可以使用其中的任一密钥。

为了提高安全性，随时可以在“密钥”页上重新生成自动化帐户的主访问密钥和辅助访问密钥。 密钥重新生成可以防止将来的节点注册使用以前的密钥。

## <a name="re-registering-a-node"></a>重新注册节点

在将计算机注册为 Azure Automation State Configuration 中的 DSC 节点之后，将来你可能会出于多种原因而需要重新注册该节点。

- **证书续订。** 对于 Windows Server 2019 之前的 Windows Server 版本，每个节点自动协商唯一的身份验证证书，该证书在一年之后过期。 如果证书过期且未续订，则节点将无法与 Azure 自动化通信，并且会标记为 `Unresponsive`。 目前，当证书即将过期时，PowerShell DSC 注册协议无法自动续订证书，必须在一年之后重新注册这些节点。 在重新注册之前，请确保每个节点正在运行 WMF 5 RTM。 

    在证书过期之前的 90 天或更短时间内重新注册，或在证书过期之后的任意时间点重新注册，会导致生成并使用新证书。 Windows Server 2019 和更高版本中提供了此问题的解决方法。

- **更改了 DSC LCM 值。** 你可能需要更改初始注册节点期间设置的 [PowerShell DSC LCM 值](https://docs.microsoft.com/powershell/scripting/dsc/managing-nodes/metaConfig4)，例如 `ConfigurationMode`。 目前只能通过重新注册来更改这些 DSC 代理值。 但分配给节点的节点配置值除外。 可以直接在 Azure Automation DSC 中更改此值。

可以使用本文档中所述的任何加入方法，像最初注册节点时那样重新注册节点。 重新注册节点之前，不需要从 Azure Automation State Configuration 中注销节点。

## <a name="troubleshooting-azure-virtual-machine-onboarding"></a>排查 Azure 虚拟机登记问题

Azure Automation State Configuration 可让你轻松加入 Azure Windows VM 以进行配置管理。 在幕后，Azure VM Desired State Configuration 扩展用于向 Azure Automation State Configuration 注册 VM。 由于 Azure VM Desired State Configuration 扩展以异步方式运行，跟踪其进度和排查其执行问题可能很重要。

> [!NOTE]
> 对于将 Azure Windows VM 加入到使用 Azure VM Desired State Configuration 扩展的 Azure Automation State Configuration 的任何方法，可能需要经过最多一小时，Azure 自动化才会将此 VM 显示为已注册。 出现这种延迟的原因是，Azure VM Desired State Configuration 扩展会在 VM 上安装 WMF 5.0，这样才能将 VM 加入到 Azure Automation State Configuration。

若要排查 Azure VM Desired State Configuration 扩展的问题或查看其状态：

1. 在 Azure 门户中，导航到所要加入的 VM。
2. 在“设置”  下单击“扩展”  。 
3. 然后根据所用的操作系统单击“DSC”或“DSCForLinux”。   
4. 有关详细信息，可以单击“查看详细状态”  。

有关故障排除的详细信息，请参阅[排查 Azure 自动化 Desired State Configuration (DSC) 问题](./troubleshoot/desired-state-configuration.md)。

## <a name="next-steps"></a>后续步骤

- 有关入门信息，请参阅 [Azure 自动化 State Configuration 入门](automation-dsc-getting-started.md)。
- 若要了解如何编译 DSC 配置，以便将它们分配给目标节点，请参阅[在 Azure 自动化 State Configuration 中编译配置](automation-dsc-compile.md)。
- 有关 PowerShell cmdlet 参考，请参阅 [Azure 自动化 State Configuration cmdlet](https://docs.microsoft.com/powershell/module/az.automation#automation)。
- 有关定价信息，请参阅 [Azure Automation State Configuration 定价](https://azure.cn/pricing/details/automation/)。
- 有关在持续部署管道中使用 Azure Automation State Configuration 的示例，请参阅[用法示例：使用 Azure Automation State Configuration 和 Chocolatey 持续部署到虚拟机](automation-dsc-cd-chocolatey.md)。
