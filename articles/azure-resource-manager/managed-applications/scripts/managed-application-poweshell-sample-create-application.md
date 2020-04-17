---
title: Azure PowerShell 脚本示例 - 部署托管应用程序
description: 提供 Azure PowerShell 示例脚本，该脚本将托管应用程序定义部署到订阅。
author: rockboyfor
ms.devlang: powershell
ms.topic: sample
origin.date: 10/27/2017
ms.date: 01/20/2020
ms.author: v-yeche
ms.openlocfilehash: a868479797a4842e03c45da5a9f05d70dcac02ee
ms.sourcegitcommit: c1ba5a62f30ac0a3acb337fb77431de6493e6096
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2020
ms.locfileid: "76170703"
---
# <a name="deploy-a-managed-application-for-a-service-catalog-with-powershell"></a>使用 PowerShell 为服务目录部署托管应用程序

此脚本从服务目录部署托管应用程序定义。

[!INCLUDE [quickstarts-free-trial-note](../../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>示例脚本

[!INCLUDE [updated-for-az](../../../../includes/updated-for-az.md)]

```powershell
# Create resource group
New-AzResourceGroup -Name applicationGroup -Location chinaeast

# Get ID of managed application definition
$appid=(Get-AzManagedApplicationDefinition -ResourceGroupName appDefinitionGroup -Name ManagedStorage).ManagedApplicationDefinitionId

# Create the managed application
New-AzManagedApplication `
  -Name storageApp `
  -Location chinaeast `
  -Kind ServiceCatalog `
  -ResourceGroupName applicationGroup `
  -ManagedApplicationDefinitionId $appid `
  -ManagedResourceGroupName "InfrastructureGroup" `
  -Parameter "{`"storageAccountNamePrefix`": {`"value`": `"demostorage`"}, `"storageAccountType`": {`"value`": `"Standard_LRS`"}}"
```

## <a name="script-explanation"></a>脚本说明

此脚本使用以下命令部署托管应用程序。 表中的每条命令均链接到特定于命令的文档。

| Command | 说明 |
|---|---|
| [New-AzManagedApplication](https://docs.microsoft.com/powershell/module/az.resources/new-azmanagedapplication) | 创建托管应用程序。 提供模板的定义 ID 和参数。 |

## <a name="next-steps"></a>后续步骤

* 有关托管应用程序的简介，请参阅 [Azure 托管应用程序概述](../overview.md)。
* 有关 PowerShell 的详细信息，请参阅 [Azure PowerShell 文档](https://docs.microsoft.com/powershell/azure/get-started-azureps)。

<!-- Update_Description: new article about managed application poweshell sample create application -->
<!--NEW.date: 01/20/2020-->