---
title: 在 Azure Windows VM 中运行脚本
description: 本主题介绍了如何在 Windows 虚拟机中运行脚本
services: automation
ms.service: automation
author: rockboyfor
ms.author: v-yeche
origin.date: 05/02/2018
ms.date: 02/18/2019
ms.topic: article
manager: digimobile
ms.openlocfilehash: e4a7fbfa920ffb1110337c45683155ec8192d459
ms.sourcegitcommit: c1ba5a62f30ac0a3acb337fb77431de6493e6096
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2020
ms.locfileid: "69539186"
---
# <a name="run-scripts-in-your-windows-vm"></a>在 Windows VM 中运行脚本

若要自动执行任务或解决问题，可能需要在 VM 中运行命令。 以下文章简要概述了可用于在 VM 中运行脚本和命令的功能。

## <a name="custom-script-extension"></a>自定义脚本扩展

[自定义脚本扩展](../extensions/custom-script-windows.md)主要用于发布部署配置和软件安装。

* 在 Azure 虚拟机中下载并运行脚本。
* 可以使用 Azure 资源管理器模板、Azure CLI、REST API、PowerShell 或 Azure 门户运行。
* 脚本文件可以从 Azure 存储或 GitHub 下载，或者在从 Azure 门户运行时从电脑提供。
* 在 Windows 计算机中运行 PowerShell 脚本，在 Linux 计算机中运行 Bash 脚本。
* 适用于部署后配置、软件安装和其他配置或管理任务。

<!--Verify successfully-->

## <a name="run-command"></a>运行命令

[运行命令](run-command.md)功能支持使用脚本进行虚拟机、应用程序管理和故障排除，并且即使在计算机不可访问时也可用，例如，如果来宾防火墙没有打开 RDP 或 SSH 端口。

* 在 Azure 虚拟机中运行脚本。
* 可以使用 [REST API](https://docs.microsoft.com/rest/api/compute/virtual%20machines%20run%20commands/runcommand)、[Azure CLI](https://docs.azure.cn/cli/vm/run-command?view=azure-cli-latest#az-vm-run-command-invoke) 或 [PowerShell](https://docs.microsoft.com/powershell/module/az.compute/invoke-azvmruncommand)
     运行<!--Not Available on [Azure portal](run-command.md)-->
* 在 Azure 门户中快速运行脚本并查看输出，并根据需要重复此过程。
* 可以直接键入脚本，也可以运行其中一个内置脚本。
* 在 Windows 计算机中运行 PowerShell 脚本，在 Linux 计算机中运行 Bash 脚本。
* 适用于虚拟机和应用程序管理以及在无法访问的虚拟机中运行脚本。

<!-- Not Available on ## Hybrid Runbook Worker -->

<!-- Not Available on ## Serial console-->
## <a name="next-steps"></a>后续步骤

详细了解可用于在 VM 中运行脚本和命令的不同功能。

* [自定义脚本扩展](../extensions/custom-script-windows.md)
* [运行命令](run-command.md)

<!-- Not Available on * [Hybrid Runbook Worker](../../automation/automation-hybrid-runbook-worker.md)-->
<!-- Not Available on * [Serial console](serial-console.md)-->
<!-- Update_Description: update meta properties -->
