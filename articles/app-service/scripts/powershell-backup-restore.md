---
title: PowerShell：还原应用备份
description: 了解如何使用 Azure PowerShell 自动部署和管理应用服务。 此示例演示如何从备份还原应用。
author: msangapu-msft
tags: azure-service-management
ms.assetid: a2a27d94-d378-4c17-a6a9-ae1e69dc4a72
ms.topic: sample
origin.date: 11/21/2018
ms.date: 08/13/2020
ms.author: v-tawe
ms.custom: mvc, seodec18
ms.openlocfilehash: 8664a8317b9d95ff41bd3700fd2469b61c35037e
ms.sourcegitcommit: 9d9795f8a5b50cd5ccc19d3a2773817836446912
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/14/2020
ms.locfileid: "88228134"
---
# <a name="restore-a-web-app-from-a-backup-using-azure-powershell"></a>使用 PowerShell 从备份中还原 Web 应用

此示例脚本从现有的 Web 应用中检索以前已完成的备份，然后通过重写其内容将其还原。 

必要时，请使用 [Azure PowerShell 指南](https://docs.microsoft.com/powershell/azure/)中的说明安装 Azure PowerShell，并运行 `Connect-AzAccount -Environment AzureChinaCloud` 创建与 Azure 的连接。 

## <a name="sample-script"></a>示例脚本

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

```powershell
$resourceGroupName = "myResourceGroup"
$webappname = "<replace-with-your-app-name>"


# List statuses of all backups that are complete or currently executing.
Get-AzWebAppBackupList -ResourceGroupName $resourceGroupName -Name $webappname

# Note the BackupID property of the backup you want to restore

# Get the backup object that you want to restore by specifying the BackupID
$backup = (Get-AzWebAppBackupList -ResourceGroupName $resourceGroupName -Name $webappname | where {$_.BackupId -eq <replace-with-BackupID>}) 

# Restore the app by overwriting it with the backup data
$backup | Restore-AzWebAppBackup -Overwrite
```

## <a name="clean-up-deployment"></a>清理部署 

如果不再需要 Web 应用，请使用以下命令删除资源组、Web 应用和所有相关的资源。

```powershell
Remove-AzResourceGroup -Name $resourceGroupName -Force
```

## <a name="script-explanation"></a>脚本说明

此脚本使用以下命令。 表中的每条命令均链接到特定于命令的文档。

| Command | 说明 |
|---|---|
| [Get-AzWebAppBackupList](https://docs.microsoft.com/powershell/module/az.websites/get-azwebappbackuplist) | 获取 Web 应用的备份列表。 |
| [Restore-AzWebAppBackup](https://docs.microsoft.com/powershell/module/az.websites/restore-azwebappbackup) | 从以前完成的备份中还原 Web 应用。 |

## <a name="next-steps"></a>后续步骤

有关 Azure PowerShell 模块的详细信息，请参阅 [Azure PowerShell 文档](https://docs.microsoft.com/powershell/azure/)。

可以在 [Azure PowerShell 示例](../samples-powershell.md)中找到 Azure 应用服务 Web 应用的其他 Azure Powershell 示例。
