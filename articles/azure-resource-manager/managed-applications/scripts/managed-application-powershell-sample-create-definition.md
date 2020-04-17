---
title: 创建托管应用程序定义 - Azure PowerShell
description: 提供在 Azure 订阅中创建托管应用程序定义的 Azure PowerShell 脚本示例。
author: rockboyfor
ms.devlang: powershell
ms.topic: sample
origin.date: 10/27/2017
ms.date: 01/20/2020
ms.author: v-yeche
ms.openlocfilehash: 4f473a405cf85c150633bdecec1d0932071c4d85
ms.sourcegitcommit: c1ba5a62f30ac0a3acb337fb77431de6493e6096
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2020
ms.locfileid: "76170710"
---
# <a name="create-a-managed-application-definition-with-powershell"></a>使用 PowerShell 创建托管应用程序定义

此脚本会将托管应用程序定义发布到服务目录。

[!INCLUDE [quickstarts-free-trial-note](../../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>示例脚本

[!INCLUDE [updated-for-az](../../../../includes/updated-for-az.md)]

```powershell
# Create resource group
New-AzResourceGroup -Name appDefinitionGroup -Location chinaeast

# Get Azure Active Directory group to manage the application
$groupid=(Get-AzADGroup -SearchString appManagers).Id

# Get role
$roleid=(Get-AzRoleDefinition -Name Owner).Id

# Create the definition for a managed application
New-AzManagedApplicationDefinition `
  -Name "ManagedStorage" `
  -Location "chinaeast" `
  -ResourceGroupName appDefinitionGroup `
  -LockLevel ReadOnly `
  -DisplayName "Managed Storage Account" `
  -Description "Managed Az.Storage Account" `
  -Authorization "${groupid}:$roleid" `
  -PackageFileUri "https://raw.githubusercontent.com/Azure/azure-managedapp-samples/master/samples/201-managed-storage-account/managedstorage.zip"

```

## <a name="script-explanation"></a>脚本说明

此脚本使用以下命令创建托管应用程序定义。 表中的每条命令均链接到特定于命令的文档。

| Command | 说明 |
|---|---|
| [New-AzManagedApplicationDefinition](https://docs.microsoft.com/powershell/module/az.resources/new-azmanagedapplicationdefinition) | 创建托管应用程序定义。 提供包含所需文件的包。 |

## <a name="next-steps"></a>后续步骤

* 有关托管应用程序的简介，请参阅 [Azure 托管应用程序概述](../overview.md)。
* 有关 PowerShell 的详细信息，请参阅 [Azure PowerShell 文档](https://docs.microsoft.com/powershell/azure/get-started-azureps)。

<!-- Update_Description: new article about managed application powershell sample create definition -->
<!--NEW.date: 01/20/2020-->