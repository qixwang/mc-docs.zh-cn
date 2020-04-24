---
title: PowerShell 示例 - 还原备份 - Azure SQL 数据库 | Microsoft Docs
description: 通过 Azure PowerShell 示例脚本演示如何使用自动备份将 Azure SQL 单一数据库还原到更早的时间点
services: sql-database
ms.service: sql-database
ms.subservice: backup-restore
ms.custom: ''
ms.devlang: PowerShell
ms.topic: sample
author: WenJason
ms.author: v-jay
ms.reviewer: carlrab
manager: digimobile
origin.date: 03/27/2019
ms.date: 04/29/2019
ms.openlocfilehash: 72abfe7538d6524ba78336f33ae480a484b6127c
ms.sourcegitcommit: c1ba5a62f30ac0a3acb337fb77431de6493e6096
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2020
ms.locfileid: "64855127"
---
# <a name="use-powershell-to-restore-an-azure-sql-single-database-to-an-earlier-point-in-time"></a>使用 PowerShell 将 Azure SQL 单一数据库还原到更早的时间点

此 PowerShell 脚本示例将 Azure SQL 数据库还原到特定的时间点。  

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]
[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

本教程需要 AZ PowerShell 1.4.0 或更高版本。 如果需要升级，请参阅[安装 Azure PowerShell 模块](https://docs.microsoft.com/powershell/azure/install-az-ps)。 此外，还需要运行 `Connect-AzAccount -EnvironmentName AzureChinaCloud` 以创建与 Azure 的连接。

## <a name="sample-script"></a>示例脚本

```powershell
# Connect-AzAccount -Environment AzureChinaCloud
$SubscriptionId = ''
# Set the resource group name and location for your server
$resourceGroupName = "myResourceGroup-$(Get-Random)"
$location = "chinaeast"
# Set an admin login and password for your server
$adminSqlLogin = "SqlAdmin"
$password = "ChangeYourAdminPassword1"
# Set server name - the logical server name has to be unique in the system
$serverName = "server-$(Get-Random)"
# The sample database name
$databaseName = "mySampleDatabase"
# The restored database names
$restoreDatabaseName = "MySampleDatabase_GeoRestore"
$pointInTimeRestoreDatabaseName = "MySampleDatabase_10MinutesAgo"
# The ip address range that you want to allow to access your server
$startIp = "0.0.0.0"
$endIp = "0.0.0.0"

# Set subscription 
Set-AzContext -SubscriptionId $subscriptionId 

# Create a resource group
$resourceGroup = New-AzResourceGroup -Name $resourceGroupName -Location $location

# Create a server with a system wide unique server name
$server = New-AzSqlServer -ResourceGroupName $resourceGroupName `
    -ServerName $serverName `
    -Location $location `
    -SqlAdministratorCredentials $(New-Object -TypeName System.Management.Automation.PSCredential -ArgumentList $adminSqlLogin, $(ConvertTo-SecureString -String $password -AsPlainText -Force))

# Create a server firewall rule that allows access from the specified IP range
$firewallRule = New-AzSqlServerFirewallRule -ResourceGroupName $resourceGroupName `
    -ServerName $serverName `
    -FirewallRuleName "AllowedIPs" -StartIpAddress $startIp -EndIpAddress $endIp

# Create a blank database with an S0 performance level
$database = New-AzSqlDatabase  -ResourceGroupName $resourceGroupName `
    -ServerName $serverName `
    -DatabaseName $databaseName `
    -RequestedServiceObjectiveName "S0" 

Start-Sleep -second 600

# Restore database to its state 7 minutes ago
# Note: Point-in-time restore requires database to be at least 5 minutes old
Restore-AzSqlDatabase `
      -FromPointInTimeBackup `
      -PointInTime (Get-Date).AddMinutes(-2) `
      -ResourceGroupName $resourceGroupName `
      -ServerName $serverName `
      -TargetDatabaseName $pointInTimeRestoreDatabaseName `
      -ResourceId $database.ResourceID `
      -Edition "Standard" `
      -ServiceObjectiveName "S0"
```

## <a name="clean-up-deployment"></a>清理部署

使用以下命令删除资源组及其相关的所有资源。

```powershell
Remove-AzResourceGroup -ResourceGroupName $resourcegroupname
```

## <a name="script-explanation"></a>脚本说明

此脚本使用以下命令。 表中的每条命令均链接到特定于命令的文档。

| Command | 说明 |
|---|---|
| [New-AzResourceGroup](https://docs.microsoft.com/powershell/module/az.resources/new-azresourcegroup) | 创建用于存储所有资源的资源组。 |
| [New-AzSqlServer](https://docs.microsoft.com/powershell/module/az.sql/new-azsqlserver) | 创建托管单一数据库或弹性池的 SQL 数据库服务器。 |
| [New-AzSqlDatabase](https://docs.microsoft.com/powershell/module/az.sql/new-azsqldatabase) | 在 SQL 数据库服务器中创建数据库作为独立数据库或共用数据库。 |
| [Get-AzSqlDatabaseGeoBackup](https://docs.microsoft.com/powershell/module/az.sql/get-azsqldatabasegeobackup) | 获取独立数据库或共用数据库的异地冗余备份。 |
| [Restore-AzSqlDatabase](https://docs.microsoft.com/powershell/module/az.sql/restore-azsqldatabase) | 还原 SQL 独立数据库或共用数据库。 |
| [Remove-AzSqlDatabase](https://docs.microsoft.com/powershell/module/az.sql/remove-azsqldatabase) | 删除 Azure SQL 独立数据库或共用数据库。 |
| [Get-AzSqlDeletedDatabaseBackup](https://docs.microsoft.com/powershell/module/az.sql/get-azsqldeleteddatabasebackup) | 获取可以还原的已删除的独立数据库或共用数据库。 |
| [Remove-AzResourceGroup](https://docs.microsoft.com/powershell/module/az.resources/remove-azresourcegroup) | 删除资源组，包括所有嵌套的资源。 |

## <a name="next-steps"></a>后续步骤

有关 Azure PowerShell 的详细信息，请参阅 [Azure PowerShell 文档](https://docs.microsoft.com/powershell/azure/overview)。

可以在 [Azure SQL 数据库 PowerShell 脚本](../sql-database-powershell-samples.md)中找到更多 SQL 数据库 PowerShell 脚本示例。
