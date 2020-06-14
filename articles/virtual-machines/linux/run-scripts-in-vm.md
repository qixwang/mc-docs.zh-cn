---
title: 在 Azure Linux VM 中运行脚本
description: 本主题介绍如何在虚拟机中运行脚本
services: automation
ms.service: virtual-machines-linux
author: Johnnytechn
ms.author: v-johya
ms.date: 06/05/2020
ms.topic: article
ms.openlocfilehash: f5598df4709687f3d32da64c4c605a587515db5d
ms.sourcegitcommit: 285649db9b21169f3136729c041e4d04d323229a
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/11/2020
ms.locfileid: "84683960"
---
# <a name="run-scripts-in-your-linux-vm"></a>在 Linux VM 中运行脚本

可能需要在 VM 中运行命令来自动完成任务或排查问题。 下文概述了可以用来在 VM 中运行脚本和命令的功能。

## <a name="custom-script-extension"></a>自定义脚本扩展

[自定义脚本扩展](../extensions/custom-script-linux.md)主要用于部署后配置和软件安装。

* 在 Azure 虚拟机中下载并运行脚本。
* 可以使用 Azure 资源管理器模板、Azure CLI、REST API、PowerShell 或 Azure 门户来运行。
* 脚本文件可以从 Azure 存储或 GitHub 下载，也可以从电脑获取（通过 Azure 门户运行时）。
* 在 Windows 计算机中运行 PowerShell 脚本，在 Linux 计算机中运行 Bash 脚本。
* 适用于部署后配置、软件安装以及其他配置或管理任务。

## <a name="run-command"></a>运行命令

[运行命令](run-command.md)功能支持使用脚本进行虚拟机、应用程序管理和故障排除，并且即使在计算机不可访问时也可用，例如，如果来宾防火墙没有打开 RDP 或 SSH 端口。

* 在 Azure 虚拟机中运行脚本。
* 可以使用 [REST API](https://docs.microsoft.com/rest/api/compute/virtual%20machines%20run%20commands/runcommand)、[Azure CLI](https://docs.azure.cn/cli/vm/run-command?view=azure-cli-latest#az-vm-run-command-invoke) 或 [PowerShell](https://docs.microsoft.com/powershell/module/az.compute/invoke-azvmruncommand) 运行

    <!--Not Available on Azure portal-->

* 在 Azure 门户中快速运行脚本并查看输出，并根据需要重复此过程。
* 可以直接键入脚本，也可以运行其中一个内置脚本。
* 在 Windows 计算机中运行 PowerShell 脚本，在 Linux 计算机中运行 Bash 脚本。
* 适用于虚拟机和应用程序管理以及在无法访问的虚拟机中运行脚本。

<!-- Not Available on ## Hybrid Runbook Worker-->
<!-- Not Available on ## Serial console-->
## <a name="next-steps"></a>后续步骤

详细了解可以用来在 VM 中运行脚本和命令的不同功能。

* [自定义脚本扩展](../extensions/custom-script-linux.md)
* [运行命令](run-command.md)

<!-- Not Available on * [Hybrid Runbook Worker](../../automation/automation-hybrid-runbook-worker.md)-->
<!-- Not Available on * [Serial console](serial-console.md)-->
<!-- Update_Description: wording update, update link -->