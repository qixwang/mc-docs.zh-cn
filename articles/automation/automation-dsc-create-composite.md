---
title: 将配置转换为 Azure 自动化状态配置的复合资源
description: 本文介绍如何将配置转换为 Azure 自动化状态配置的复合资源。
keywords: dsc,powershell,配置,安装程序
services: automation
ms.service: automation
ms.subservice: dsc
author: WenJason
ms.author: v-jay
origin.date: 08/08/2019
ms.date: 08/10/2020
ms.topic: conceptual
manager: digimobile
ms.openlocfilehash: 31985b9c690c1e08296a5f73d045a9e8e5b23b5b
ms.sourcegitcommit: e6b216b180734783219378410e13192e314a4497
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/05/2020
ms.locfileid: "87790070"
---
# <a name="convert-configurations-to-composite-resources"></a>将配置转换为复合资源

> 适用于：Windows PowerShell 5.1

开始创作配置后，你可以快速创建“方案”来管理设置组。
示例有：

- 创建 Web 服务器
- 创建 DNS 服务器
- 创建 SharePoint 服务器
- 配置 SQL 群集
- 管理防火墙设置
- 管理密码设置

如果你有兴趣与他人共享此项工作，最佳选择是将配置打包为[复合资源](https://docs.microsoft.com/powershell/scripting/dsc/resources/authoringresourcecomposite)。
首次创建复合资源可能会很困难。

> [!NOTE]
> 本文引用由开放源代码社区维护的解决方案。
> 支持仅以 GitHub 协作的形式提供，而不是由 Azure 提供。

## <a name="community-project-compositeresource"></a>社区项目：CompositeResource

为了解决此难题，已创建了一个名为 [CompositeResource](https://github.com/microsoft/compositeresource) 的社区维护的解决方案。

CompositeResource 自动执行从配置创建新模块的过程。
首先在工作站上[使用点获取](https://devblogs.microsoft.com/scripting/how-to-reuse-windows-powershell-functions-in-scripts/)配置脚本（或生成服务器），使其加载到内存中。
接下来，请使用 CompositeResource 模块提供的函数自动执行转换，而不是运行配置以生成 MOF 文件。
该 cmdlet 将加载配置的内容，获取参数列表，并使用所需的所有内容生成新的模块。

生成模块后，可以在每次进行更改时递增版本并添加发行说明，然后将其发布到自己的 [PowerShellGet 存储库](https://powershellexplained.com/2018-03-03-Powershell-Using-a-NuGet-server-for-a-PSRepository/?utm_source=blog&utm_medium=blog&utm_content=psscriptrepo)。

创建包含配置（或多个配置）的复合资源模块后，可以在 Azure 中的[可组合创作体验](./compose-configurationwithcompositeresources.md)中使用它们，或将其添加到 [DSC 配置脚本](https://docs.microsoft.com/powershell/scripting/dsc/configurations/configurations)以生成 MOF 文件，并[将 MOF 文件上传到 Azure 自动化](./tutorial-configure-servers-desired-state.md#create-and-upload-a-configuration-to-azure-automation)。
然后，在[本地](./automation-dsc-onboarding.md#enable-physicalvirtual-linux-machines)或[在 Azure 中](./automation-dsc-onboarding.md#enable-azure-vms)注册服务器以拉取配置。
该项目的最新更新还发布了 Azure 自动化自动执行从 PowerShell 库导入配置的过程的 [runbook](https://www.powershellgallery.com/packages?q=DscGallerySamples)。

若要尝试自动创建 DSC 的复合资源，请访问 [PowerShell 库](https://www.powershellgallery.com/packages/compositeresource/) 并下载解决方案，或单击“项目网站”以查看[文档](https://github.com/microsoft/compositeresource)。

## <a name="next-steps"></a>后续步骤

- 若要了解 PowerShell DSC，请参阅 [Windows PowerShell Desired State Configuration 概述](https://docs.microsoft.com/powershell/scripting/dsc/overview/overview)。
- 了解 [DSC 资源](https://docs.microsoft.com/powershell/scripting/dsc/resources/resources)中的 PowerShell DSC 资源。
- 有关本地 Configuration Manager 配置的详细信息，请参阅[配置本地 Configuration Manager](https://docs.microsoft.com/powershell/scripting/dsc/managing-nodes/metaconfig)。
