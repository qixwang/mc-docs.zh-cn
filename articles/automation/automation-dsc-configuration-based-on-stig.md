---
title: 基于 STIG 为 Azure Automation State Configuration 配置数据
description: 本文介绍如何基于 DoD STIG 为 Azure Automation State Configuration 配置数据。
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
ms.openlocfilehash: a173a31b785973e5bf4f67e90e73eb0955a4e53f
ms.sourcegitcommit: e6b216b180734783219378410e13192e314a4497
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/05/2020
ms.locfileid: "87790075"
---
# <a name="configure-data-based-on-security-technical-information-guide-stig"></a>基于安全技术信息指南 (STIG) 配置数据

> 适用于：Windows PowerShell 5.1

首次创建配置内容可能很困难。
在许多情况下，目标是按照希望符合行业建议的“基线”自动配置服务器。

> [!NOTE]
> 本文引用了一个由开源社区维护的解决方案。
> 支持仅以 GitHub 协作的形式提供，而不是由 Azure 提供。

## <a name="community-project-powerstig"></a>社区项目：PowerSTIG

名为 [PowerSTIG](https://github.com/microsoft/powerstig) 的社区项目旨在通过以下方式来解决此问题：基于提供的关于 STIG（安全技术实施指南）的[公共信息](https://public.cyber.mil/stigs/)生成 DSC 内容。

处理基线比听起来要复杂得多。
许多组织需要[记录规则例外](https://github.com/microsoft/powerstig#powerstigdata)并大规模管理该数据。
PowerSTIG 通过提供[复合资源](https://github.com/microsoft/powerstig#powerstigdsc)来处理配置的每个领域，而不是尝试在一个大文件中处理整个设置范围，从而解决了此问题。

生成配置后，可以使用 [DSC 配置脚本](https://docs.microsoft.com/powershell/scripting/dsc/configurations/configurations)生成 MOF 文件，并[将 MOF 文件上传到 Azure 自动化](./tutorial-configure-servers-desired-state.md#create-and-upload-a-configuration-to-azure-automation)。
然后从[本地](./automation-dsc-onboarding.md#enable-physicalvirtual-linux-machines)或[在 Azure 中](./automation-dsc-onboarding.md#enable-azure-vms)注册服务器以拉取配置。

若要试用 PowerSTIG，请访问 [PowerShell 库](https://www.powershellgallery.com)并下载解决方案，或者单击“项目站点”以查看[文档](https://github.com/microsoft/powerstig)。

## <a name="next-steps"></a>后续步骤

- 若要了解 PowerShell DSC，请参阅 [Windows PowerShell Desired State Configuration 概述](https://docs.microsoft.com/powershell/scripting/dsc/overview/overview)。
- 了解 [DSC 资源](https://docs.microsoft.com/powershell/scripting/dsc/resources/resources)中的 PowerShell DSC 资源。
- 有关本地 Configuration Manager 配置的详细信息，请参阅[配置本地 Configuration Manager](https://docs.microsoft.com/powershell/scripting/dsc/managing-nodes/metaconfig)。
