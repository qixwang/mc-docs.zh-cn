---
title: 使用 Chocolatey 进行 Azure Automation State Configuration 持续部署
description: 介绍如何使用 Azure Automation State Configuration 通过 Chocolatey 包管理器进行 DevOps 持续部署。 包括使用完整 JSON 资源管理器模板和 PowerShell 源代码的示例。
services: automation
ms.subservice: dsc
origin.date: 08/08/2018
ms.date: 05/11/2020
ms.topic: conceptual
ms.openlocfilehash: 9949cb978b01227be3d4344f08f8db5d4830e4b7
ms.sourcegitcommit: be0a8e909fbce6b1b09699a721268f2fc7eb89de
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/29/2020
ms.locfileid: "84199571"
---
# <a name="provide-continuous-deployment-to-virtual-machines-using-automation-state-configuration-and-chocolatey"></a>使用 Automation State Configuration 和 Chocolatey 提供到虚拟机的持续部署

DevOps 领域中有许多工具可帮助你处理持续集成管道中的各个点。 Azure Automation [State Configuration](automation-dsc-overview.md) 是 DevOps 团队可以采用的新选项。 

Azure 自动化是 Azure 中的托管服务，可让你使用 Runbook、节点和共享资源（例如凭据、计划和全局变量）自动完成各种任务。 Azure Automation State Configuration 扩展了此自动化功能，包含 PowerShell Desired State Configuration (DSC) 工具。 以下是一个不错的 [概述](automation-dsc-overview.md)。

本文演示如何为 Windows 计算机设置持续部署 (CD)。 可以轻松扩展技术，在角色（例如网站）中按需添加任意数目的 Windows 计算机，并从该角色扩展到其他角色。

![IaaS VM 的持续部署](./media/automation-dsc-cd-chocolatey/cdforiaasvm.png)

## <a name="at-a-high-level"></a>概括而言

此处要处理的事项很多，幸好这些事项可划分成两个主要过程：

- 编写并测试代码，针对系统的主要和次要版本创建并发布安装包。
- 创建和管理用于安装和执行包中代码的 VM。  

完成这两个核心过程后，随着新版本的创建和部署，可以轻松地自动更新 VM 上的包。

## <a name="component-overview"></a>组件概述

[apt-get](https://en.wikipedia.org/wiki/Advanced_Packaging_Tool) 之类的包管理器在 Linux 领域耳熟能详，但在 Windows 领域并不被大家所熟悉。
[Chocolatey](https://chocolatey.org/) 就是这样一个工具，Scott Hanselman 的有关该工具主题的[博客](https://www.hanselman.com/blog/IsTheWindowsUserReadyForAptget.aspx)对此工具进行了深入介绍。 简单地说，Chocolatey 可让你使用命令行将中央存储库中的包安装到 Windows 操作系统。 可以创建和管理自己的存储库，Chocolatey 可以从指定的任何数量的存储库来安装包。

[PowerShell DSC](https://docs.microsoft.com/powershell/scripting/dsc/overview/overview) 是一个 PowerShell 工具，用于为计算机声明你所需要的配置。 例如，如果你想要安装 Chocolatey、安装 IIS、打开端口 80、安装网站 1.0.0 版，DSC 本地配置管理器 (LCM) 可实现这种配置。 DSC 拉取服务器包含一个计算机配置存储库。 每台计算机上的 LCM 定期检查计算机的配置是否与存储的配置匹配。 它可以报告状态，也可以尝试让计算机恢复到与存储的配置匹配。 可以编辑“拉”服务器上存储的配置，使一台计算机或一组计算机与更改的配置匹配。

DSC 资源是具有特定功能（例如管理网络、Active Directory 或 SQL Server）的代码模块。 Chocolatey DSC 资源知道如何访问 NuGet 服务器（以及其他组件）、下载包、安装包，等等。 [PowerShell 库](https://www.powershellgallery.com/packages?q=dsc+resources&prerelease=&sortOrder=package-title)中有其他许多 DSC 资源。 可将这些模块安装到 Azure Automation State Configuration 拉取服务器以供配置使用。

资源管理器模板以声明方式生成基础结构，例如网络、子网、网络安全性和路由、负载均衡器、NIC、VM，等等。 [此文](../azure-resource-manager/management/deployment-models.md)将资源管理器部署模型（声明性）与 Azure 服务管理（简称 ASM，也称经典）部署模型（命令性）做了比较。 此文还介绍了核心资源提供程序：计算、存储和网络。

资源管理器模板的一项主要功能是能够在预配时将 VM 扩展安装到 VM 中。 VM 扩展具有特定功能，例如运行自定义脚本、安装防病毒软件和运行 DSC 配置脚本。 有许多其他类型的 VM 扩展。

## <a name="quick-trip-around-the-diagram"></a>示意图速览

首先，需要编写、生成并测试代码，然后创建安装包。 Chocolatey 可以处理各种类型的安装包，例如 MSI、MSU、ZIP。 如果 Chocolatey 的本机功能不足以满足需要，还有 PowerShell 的完整功能可执行实际安装。 将包放入可访问的位置 – 包存储库。 本用例使用 Azure Blob 存储帐户中的公共文件夹，但它可以位于任何位置。 Chocolatey 原生可配合 NuGet 服务器和其他某些工具一起管理包元数据。 [本文](https://github.com/chocolatey/choco/wiki/How-To-Host-Feed) 介绍了相应的选项。 本用例使用 NuGet。 Nuspec 是包的元数据。 Nuspec 信息将编译成 NuPkg，并存储在 NuGet 服务器中。 当配置按名称请求某个包并引用 NuGet 服务器时，VM 上的 Chocolatey DSC 资源会抓取并安装该包。 也可以请求特定版本的包。

插图的左下方有一个 Azure 资源管理器模板。 在本用例中，VM 扩展将 VM 作为节点注册到 Azure Automation State Configuration 拉取服务器。 配置在拉取服务器中存储两次：一次以纯文本格式存储，一次以编译为 MOF 文件的形式存储。 在 Azure 门户中，MOF 代表节点配置，而不是简单的配置。 它是与节点关联的项目，因此节点知道它的配置。 以下详细信息演示如何将节点配置分配给节点。

创建、编译 Nuspec，并将其存储在 NuGet 服务器中是一件很简单的事。 并且已在管理 VM。 

持续部署的下一步需要设置拉取服务器一次、向它注册节点一次，然后创建初始配置并将其存储在服务器上。 将包升级并部署到存储库后，只需按需刷新该配置以及拉取服务器上的节点配置即可。

如果不是从资源管理器模板着手，也没有关系。 可以通过 PowerShell 命令将 VM 注册到拉取服务器。 有关详细信息，请参阅[登记由 Azure Automation State Configuration 管理的计算机](automation-dsc-onboarding.md)。

## <a name="about-the-usage-example"></a>关于本用例

本文中的用例从来自于 Azure 库的通用 Windows Server 2012 R2 映像中的一个 VM 开始。 用户可以从任何存储的映像开始，并使用 DSC 配置对其进行调整。
不过，更改已刻入映像的配置要比使用 DSC 动态更新配置难得多。

将此技巧运用于 VM 时，不需要使用资源管理器模板和 VM 扩展。 即使 VM 不在 Azure 上，也能由 CD 管理。 只需在 VM 上安装 Chocolatey 并配置 LCM，以使其知道请求服务器的所在位置即可。

在生产环境中的 VM 上更新包时，在安装更新的过程中，需要将 VM 从轮换列表中排除。 具体的操作根据情况而有很大的差异。 例如，如果 VM 在 Azure 负载均衡器后面，则可以添加自定义探测。 更新 VM 时，让探测终结点返回 400。 可在配置中进行所需的调整来造成这种更改，但更新完成时，调整将切换为返回 200。

GitHub 上的 [此 Visual Studio 项目](https://github.com/sebastus/ARM/tree/master/CDIaaSVM) 中提供了本用例的完整源代码。

## <a name="step-1-set-up-the-pull-server-and-automation-account"></a>步骤 1：设置拉取服务器和自动化帐户

在经过身份验证的 (`Connect-AzureRmAccount -EnvironmentName AzureChinaCloud`) PowerShell 命令行中：（如果设置请求服务器，则可能需要几分钟时间）

```powershell
New-AzureRmResourceGroup –Name MY-AUTOMATION-RG –Location MY-RG-LOCATION-IN-QUOTES
New-AzureRmAutomationAccount –ResourceGroupName MY-AUTOMATION-RG –Location MY-RG-LOCATION-IN-QUOTES –Name MY-AUTOMATION-ACCOUNT
```

## <a name="step-2-make-vm-extension-tweaks-to-the-resource-manager-template"></a>步骤 2：使 VM 扩展根据资源管理器模板进行调整

此 [Azure 快速入门模板](https://github.com/Azure/azure-quickstart-templates/tree/master/dsc-extension-azure-automation-pullserver)提供了 VM 注册（使用 PowerShell DSC VM 扩展）的详细信息。
此步骤将新的 VM 注册到“拉”服务器的 State Configuration 节点列表中。 此注册的一部分指定要应用到节点的节点配置。 此节点配置尚无需存在于拉取服务器中，因此该操作最初可以在步骤 4 中执行。 但在步骤 2 中，需要确定节点名称和配置名称。 在本用例中，节点名称为“isvbox”，配置名称为“ISVBoxConfig”。 因此，节点配置名称（会在 DeploymentTemplate.json 中指定）为“ISVBoxConfig.isvbox”。

## <a name="step-3-add-required-dsc-resources-to-the-pull-server"></a>步骤 3：将所需的 DSC 资源添加到拉取服务器

还有一种手动方法，但该方法只能对每个资源使用一次，除非以后要升级该资源。

>[!NOTE]
>适用于 Windows 计算机的 PowerShell 集成模块的文件夹结构与 Azure 自动化所需的文件夹结构稍有不同。 

1. 安装 [Windows Management Framework v5](https://aka.ms/wmf5latest)（对于 Windows 10 来说不需要）。

2. 安装集成模块。

    ```azurepowershell
    Install-Module –Name MODULE-NAME`    <—grabs the module from the PowerShell Gallery
    ```

3. 将模块文件夹从 **c:\Program Files\WindowsPowerShell\Modules\MODULE-NAME** 复制到临时文件夹。

4. 删除主文件夹中的示例和文档。

5. 压缩主文件夹，并使用该文件夹的名称为 ZIP 文件命名。

6. 将 ZIP 文件放到可访问的 HTTP 位置，例如 Azure 存储帐户中的 Blob 存储。

7. 运行以下命令。

    ```azurepowershell
    New-AzureRmAutomationModule `
      -ResourceGroupName MY-AUTOMATION-RG -AutomationAccountName MY-AUTOMATION-ACCOUNT `
      -Name MODULE-NAME –ContentLinkUri 'https://STORAGE-URI/CONTAINERNAME/MODULE-NAME.zip'
    ```

随附的示例针对 cChoco 和 xNetworking 执行这些步骤。 

## <a name="step-4-add-the-node-configuration-to-the-pull-server"></a>步骤 4：将节点配置添加到拉取服务器

首次将配置导入请求服务器并进行编译时并没有什么特别之处。 对相同的配置进行后续导入或编译时，过程完全相同。 每次更新包且需要向外推送到生产环境时，在确保配置文件（包括包的新版本）正确之后，就可以执行此步骤。 下面是配置文件 **ISVBoxConfig.ps1**：

```powershell
Configuration ISVBoxConfig
{
    Import-DscResource -ModuleName cChoco
    Import-DscResource -ModuleName xNetworking

    Node 'isvbox' {

        cChocoInstaller installChoco
        {
            InstallDir = 'C:\choco'
        }

        WindowsFeature installIIS
        {
            Ensure = 'Present'
            Name   = 'Web-Server'
        }

        xFirewall WebFirewallRule
        {
            Direction    = 'Inbound'
            Name         = 'Web-Server-TCP-In'
            DisplayName  = 'Web Server (TCP-In)'
            Description  = 'IIS allow incoming web site traffic.'
            Enabled       = 'True'
            Action       = 'Allow'
            Protocol     = 'TCP'
            LocalPort    = '80'
            Ensure       = 'Present'
        }

        cChocoPackageInstaller trivialWeb
        {
            Name      = 'trivialweb'
            Version   = '1.0.0'
            Source    = 'MY-NUGET-V2-SERVER-ADDRESS'
            DependsOn = '[cChocoInstaller]installChoco','[WindowsFeature]installIIS'
        }
    }
}
```

New-ConfigurationScript.ps1：

```powershell
Import-AzureRmAutomationDscConfiguration `
    -ResourceGroupName MY-AUTOMATION-RG –AutomationAccountName MY-AUTOMATION-ACCOUNT `
    -SourcePath C:\temp\AzureAutomationDsc\ISVBoxConfig.ps1 `
    -Published –Force

$jobData = Start-AzureRmAutomationDscCompilationJob `
    -ResourceGroupName MY-AUTOMATION-RG –AutomationAccountName MY-AUTOMATION-ACCOUNT `
    -ConfigurationName ISVBoxConfig

$compilationJobId = $jobData.Id

Get-AzureRmAutomationDscCompilationJob `
    -ResourceGroupName MY-AUTOMATION-RG –AutomationAccountName MY-AUTOMATION-ACCOUNT `
    -Id $compilationJobId
```

这些步骤会生成放在拉取服务器上的名为 **ISVBoxConfig.isvbox** 的新节点配置。 生成的节点配置名称为 `configurationName.nodeName`。

## <a name="step-5-create-and-maintain-package-metadata"></a>步骤 5：创建和维护包元数据

对于放入包存储库中的每一个包，需要使用 Nuspec 来描述它。 该 Nuspec 必须经过编译并存储在 NuGet 服务器上。 [此处](https://docs.nuget.org/create/creating-and-publishing-a-package)对该过程进行了说明。 

可以使用 **MyGet.org** 作为 NuGet 服务器。 可以购买此服务，但也可以获取免费的初学者 SKU。 在 [NuGet](https://www.nuget.org/) 中，可以找到为专用包安装你自己的 NuGet 服务器的说明。

## <a name="step-6-tie-it-all-together"></a>步骤 6：汇总

每当有某个版本通过 QA 并获得部署批准时，即会创建包，更新 nuspec 和 nupkg 并将其部署到 NuGet 服务器。 此外，必须更新配置（步骤 4），使之与新版本号相符。 然后，必须将配置发送到拉取服务器并对其进行编译。

然后，依赖于该配置的 VM 提取并安装更新。 其中的每项更新都很简单 - 只需一两行的 PowerShell 代码。 就 Azure DevOps 来说，有些更新封装在可一起链接到内部版本内的生成任务中。 [本文](https://www.visualstudio.com/docs/alm-devops-feature-index#continuous-delivery) 提供更多详细信息。 此 [GitHub 存储库](https://github.com/Microsoft/vso-agent-tasks)详细说明了可用的生成任务。

## <a name="related-articles"></a>相关文章
* [Azure Automation DSC overview（Azure 自动化 DSC 概述）](automation-dsc-overview.md)
* [Onboarding machines for management by Azure Automation DSC（登记由 Azure 自动化 DSC 管理的计算机）](automation-dsc-onboarding.md)

## <a name="next-steps"></a>后续步骤

- 有关概述，请参阅 [Azure Automation State Configuration](automation-dsc-overview.md)。
- 有关入门信息，请参阅 [Azure 自动化 State Configuration 入门](automation-dsc-getting-started.md)。
- 若要了解如何编译 DSC 配置，以便将它们分配给目标节点，请参阅[在 Azure 自动化 State Configuration 中编译配置](automation-dsc-compile.md)。
- 有关 PowerShell cmdlet 参考，请参阅 [Azure Automation State Configuration cmdlet](https://docs.microsoft.com/powershell/module/azurerm.automation/#automation)
- 有关定价信息，请参阅 [Azure Automation State Configuration 定价](https://azure.cn/pricing/details/automation/)。
- 若要查看在持续部署管道中使用 Azure Automation State Configuration 的示例，请参阅[使用 Azure Automation State Configuration 和 Chocolatey 进行持续部署](automation-dsc-cd-chocolatey.md)。