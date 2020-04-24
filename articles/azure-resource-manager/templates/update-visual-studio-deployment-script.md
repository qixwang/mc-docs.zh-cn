---
title: 更新 Visual Studio 的模板部署脚本以使用 Az PowerShell
description: 将 Visual Studio 模板部署脚本从 AzureRM 更新为 Az PowerShell
author: rockboyfor
ms.topic: conceptual
origin.date: 01/31/2020
ms.date: 03/23/2020
ms.author: v-yeche
ms.openlocfilehash: a68d95c57fada923b2be426bc35405114292e6de
ms.sourcegitcommit: c1ba5a62f30ac0a3acb337fb77431de6493e6096
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2020
ms.locfileid: "79543907"
---
# <a name="update-visual-studio-template-deployment-script-to-use-az-powershell-module"></a>更新 Visual Studio 模板部署脚本以使用 Az PowerShell 模块

Visual Studio 16.4 支持在模板部署脚本中使用 Az PowerShell 模块。 但是，Visual Studio 不会自动安装该模块。 若要使用 Az 模块，需要执行以下四个步骤：

1. [卸载 AzureRM 模块](https://docs.microsoft.com/powershell/azure/uninstall-az-ps#uninstall-the-azurerm-module)
1. [安装 Az 模块](https://docs.microsoft.com/powershell/azure/install-az-ps)
1. 将 Visual Studio 更新到16.4
1. 更新项目中的部署脚本。

## <a name="update-visual-studio-to-164"></a>将 Visual Studio 更新到16.4

将 Visual Studio 的安装更新到版本 16.4 或更高版本。 在升级过程中，请确保未选中 Azure PowerShell 组件。 由于已通过库安装了 Az 模块，因此不希望重新安装 AzureRM 模块。

如果已升级到 16.4，并且已选中 Azure PowerShell 组件，则可以通过运行 Visual Studio 安装程序将其卸载。 请勿在“Azure 工作负荷”或“单个组件”页上选择 Azure PowerShell 组件。

## <a name="update-the-deployment-script-in-your-project"></a>更新项目中的部署脚本

在部署脚本中，将出现的所有字符串“AzureRm”替换为“Az”。 可以参考此 [gist](https://gist.github.com/cweining/d2da2479418ea403499c4306dcf4f619) 中的修订内容来查看更改。 有关将脚本升级到 Az 模块的详细信息，请参阅[将 Azure PowerShell 从 AzureRM 迁移到 Az](https://docs.microsoft.com/powershell/azure/migrate-from-azurerm-to-az)。

## <a name="next-steps"></a>后续步骤

若要了解如何使用 Visual Studio 项目，请参阅[通过 Visual Studio 创建和部署 Azure 资源组项目](create-visual-studio-deployment-project.md)。

<!-- Update_Description: update meta properties, wording update, update link -->