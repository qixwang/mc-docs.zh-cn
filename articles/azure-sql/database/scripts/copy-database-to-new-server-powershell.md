---
title: PowerShell：将数据库复制到新服务器
description: 将数据库复制到新服务器的 Azure PowerShell 示例脚本
services: sql-database
ms.service: sql-database
ms.subservice: data-movement
ms.custom: ''
ms.devlang: PowerShell
ms.topic: sample
author: WenJason
ms.author: v-jay
ms.reviewer: carlrab
origin.date: 03/12/2019
ms.date: 07/13/2019
ms.openlocfilehash: e4b6c1d004727f665f3e51d22cbe392c07da679a
ms.sourcegitcommit: fa26665aab1899e35ef7b93ddc3e1631c009dd04
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/10/2020
ms.locfileid: "86227908"
---
# <a name="use-powershell-to-copy-a-database-to-a-new-server"></a>使用 PowerShell 将数据库复制到新服务器
[!INCLUDE[appliesto-sqldb](../../includes/appliesto-sqldb.md)]

以下 Azure PowerShell 脚本示例在新服务器的 Azure SQL 数据库中创建现有数据库的副本。

[!INCLUDE [quickstarts-free-trial-note](../../../../includes/quickstarts-free-trial-note.md)]
[!INCLUDE [updated-for-az](../../../../includes/updated-for-az.md)]

本教程需要 Az PowerShell 1.4.0 或更高版本。 如果需要进行升级，请参阅 [Install Azure PowerShell module](https://docs.microsoft.com/powershell/azure/install-az-ps)（安装 Azure PowerShell 模块）。 此外，还需要运行 `Connect-AzAccount -EnvironmentName AzureChinaCloud` 以创建与 Azure 的连接。

## <a name="copy-a-database-to-a-new-server"></a>将数据库复制到新服务器

```azurepowershell
# Connect-AzAccount -Environment AzureChinaCloud
# The SubscriptionId in which to create these objects
$SubscriptionId = ''
# Set the resource group name and location for your source server
$sourceResourceGroupName = "mySourceResourceGroup-$(Get-Random)"
$sourceResourceGroupLocation = "chinaeast"
# Set the resource group name and location for your target server
$targetResourceGroupname = "myTargetResourceGroup-$(Get-Random)"
$targetResourceGroupLocation = "chinanorth"
# Set an admin login and password for your server
$adminSqlLogin = "SqlAdmin"
$password = "ChangeYourAdminPassword1"
# The logical server names have to be unique in the system
$sourceServerName = "source-server-$(Get-Random)"
$targetServerName = "target-server-$(Get-Random)"
# The sample database name
$sourceDatabaseName = "mySampleDatabase"
$targetDatabaseName = "CopyOfMySampleDatabase"
# The ip address range that you want to allow to access your servers
$sourceStartIp = "0.0.0.0"
$sourceEndIp = "0.0.0.0"
$targetStartIp = "0.0.0.0"
$targetEndIp = "0.0.0.0"

# Set subscription 
Set-AzContext -SubscriptionId $subscriptionId 

# Create two new resource groups
$sourceResourceGroup = New-AzResourceGroup -Name $sourceResourceGroupName -Location $sourceResourceGroupLocation
$targetResourceGroup = New-AzResourceGroup -Name $targetResourceGroupname -Location $targetResourceGroupLocation

# Create a server with a system wide unique server name
$sourceResourceGroup = New-AzSqlServer -ResourceGroupName $sourceResourceGroupName `
    -ServerName $sourceServerName `
    -Location $sourceResourceGroupLocation `
    -SqlAdministratorCredentials $(New-Object -TypeName System.Management.Automation.PSCredential -ArgumentList $adminSqlLogin, $(ConvertTo-SecureString -String $password -AsPlainText -Force))
$targetResourceGroup = New-AzSqlServer -ResourceGroupName $targetResourceGroupname `
    -ServerName $targetServerName `
    -Location $targetResourceGroupLocation `
    -SqlAdministratorCredentials $(New-Object -TypeName System.Management.Automation.PSCredential -ArgumentList $adminSqlLogin, $(ConvertTo-SecureString -String $password -AsPlainText -Force))

# Create a server firewall rule that allows access from the specified IP range
$sourceServerFirewallRule = New-AzSqlServerFirewallRule -ResourceGroupName $sourceResourceGroupName `
    -ServerName $sourceServerName `
    -FirewallRuleName "AllowedIPs" -StartIpAddress $sourcestartip -EndIpAddress $sourceEndIp
$targetServerFirewallRule = New-AzSqlServerFirewallRule -ResourceGroupName $targetResourceGroupname `
    -ServerName $targetServerName `
    -FirewallRuleName "AllowedIPs" -StartIpAddress $targetStartIp -EndIpAddress $targetEndIp

# Create a blank database in the source-server with an S0 performance level
$sourceDatabase = New-AzSqlDatabase  -ResourceGroupName $sourceResourceGroupName `
    -ServerName $sourceServerName `
    -DatabaseName $sourceDatabaseName -RequestedServiceObjectiveName "S0"

# Copy source database to the target server 
$databaseCopy = New-AzSqlDatabaseCopy -ResourceGroupName $sourceResourceGroupName `
    -ServerName $sourceServerName `
    -DatabaseName $sourceDatabaseName `
    -CopyResourceGroupName $targetResourceGroupname `
    -CopyServerName $targetServerName `
    -CopyDatabaseName $targetDatabaseName 

# Clean up deployment 
# Remove-AzResourceGroup -ResourceGroupName $sourceResourceGroupName
# Remove-AzResourceGroup -ResourceGroupName $targetResourceGroupname
```

## <a name="clean-up-deployment"></a>清理部署

使用以下命令删除资源组及其相关的所有资源。

```powershell
Remove-AzResourceGroup -ResourceGroupName $sourceresourcegroupname
Remove-AzResourceGroup -ResourceGroupName $targetresourcegroupname
```

## <a name="script-explanation"></a>脚本说明

此脚本使用以下命令。 表中的每条命令均链接到特定于命令的文档。

| 命令 | 注释 |
|---|---|
| [New-AzResourceGroup](https://docs.microsoft.com/powershell/module/az.resources/new-azresourcegroup) | 创建用于存储所有资源的资源组。 |
| [New-AzSqlServer](https://docs.microsoft.com/powershell/module/az.sql/new-azsqlserver) | 创建托管数据库和弹性池的服务器。 |
| [New-AzSqlDatabase](https://docs.microsoft.com/powershell/module/az.sql/new-azsqldatabase) | 创建数据库或弹性池。 |
| [New-AzSqlDatabaseCopy](https://docs.microsoft.com/powershell/module/az.sql/new-azsqldatabasecopy) | 创建当前使用快照的数据库副本。 |
| [Remove-AzResourceGroup](https://docs.microsoft.com/powershell/module/az.resources/remove-azresourcegroup) | 删除资源组，包括所有嵌套的资源。 |
|||

## <a name="next-steps"></a>后续步骤

有关 Azure PowerShell 的详细信息，请参阅 [Azure PowerShell 文档](https://docs.microsoft.com/powershell/azure/overview)。

可以在 [Azure SQL 数据库 PowerShell 脚本](../powershell-script-content-guide.md)中找到更多 SQL 数据库 PowerShell 脚本示例。
