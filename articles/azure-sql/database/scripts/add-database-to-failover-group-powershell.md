---
title: PowerShell：将数据库添加到自动故障转移组
description: 使用 Azure PowerShell 示例脚本在 Azure SQL 数据库中创建一个数据库，将其添加到自动故障转移组，然后测试故障转移。
services: sql-database
ms.service: sql-database
ms.subservice: high-availability
ms.custom: sqldbrb=1
ms.devlang: PowerShell
ms.topic: sample
author: WenJason
ms.author: v-jay
ms.reviewer: carlrab
origin.date: 07/16/2019
ms.date: 07/13/2020
ms.openlocfilehash: c28fac10e6c8e6c0005dfc91e935507570582575
ms.sourcegitcommit: fa26665aab1899e35ef7b93ddc3e1631c009dd04
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/10/2020
ms.locfileid: "86227743"
---
# <a name="use-powershell-to-add-a-database-to-a-failover-group"></a>使用 PowerShell 将数据库添加到故障转移组

[!INCLUDE[appliesto-sqldb](../../includes/appliesto-sqldb.md)]

此 PowerShell 脚本示例创建一个 Azure SQL 数据库中的单一数据库，创建一个故障转移组，向其中添加该数据库，然后测试故障转移。

[!INCLUDE [quickstarts-free-trial-note](../../../../includes/quickstarts-free-trial-note.md)]
[!INCLUDE [updated-for-az](../../../../includes/updated-for-az.md)]

本教程需要 AZ PowerShell 1.4.0 或更高版本。 如果需要升级，请参阅[安装 Azure PowerShell 模块](https://docs.microsoft.com/powershell/azure/install-az-ps)。 如果在本地运行 PowerShell，则还需运行 `Connect-AzAccount -Environment AzureChinaCloud` 来创建与 Azure 的连接。

## <a name="sample-scripts"></a>示例脚本

```azurepowershell
# Set variables for your server and database
$subscriptionId = '<SubscriptionID>'
$randomIdentifier = $(Get-Random)
$resourceGroupName = "myResourceGroup-$randomIdentifier"
$location = "China East 2"
$adminLogin = "azureuser"
$password = "PWD27!"+(New-Guid).Guid
$serverName = "mysqlserver-$randomIdentifier"
$databaseName = "mySampleDatabase"
$drLocation = "China North 2"
$drServerName = "mysqlsecondary-$randomIdentifier"
$failoverGroupName = "failovergrouptutorial-$randomIdentifier"


# The ip address range that you want to allow to access your server 
# Leaving at 0.0.0.0 will prevent outside-of-azure connections
$startIp = "0.0.0.0"
$endIp = "0.0.0.0"

# Show randomized variables
Write-host "Resource group name is" $resourceGroupName 
Write-host "Password is" $password  
Write-host "Server name is" $serverName 
Write-host "DR Server name is" $drServerName 
Write-host "Failover group name is" $failoverGroupName

# Set subscription ID
Set-AzContext -SubscriptionId $subscriptionId

# Create a resource group
Write-host "Creating resource group..."
$resourceGroup = New-AzResourceGroup -Name $resourceGroupName -Location $location -Tag @{Owner="SQLDB-Samples"}
$resourceGroup


# Create a server with a system wide unique server name
Write-host "Creating primary logical server..."
$server = New-AzSqlServer -ResourceGroupName $resourceGroupName `
   -ServerName $serverName `
   -Location $location `
   -SqlAdministratorCredentials $(New-Object -TypeName System.Management.Automation.PSCredential `
   -ArgumentList $adminLogin, $(ConvertTo-SecureString -String $password -AsPlainText -Force))
$server

# Create a server firewall rule that allows access from the specified IP range
Write-host "Configuring firewall for primary logical server..."
$serverFirewallRule = New-AzSqlServerFirewallRule -ResourceGroupName $resourceGroupName `
   -ServerName $serverName `
   -FirewallRuleName "AllowedIPs" -StartIpAddress $startIp -EndIpAddress $endIp
$serverFirewallRule

# Create General Purpose Gen5 database with 2 vCore
Write-host "Creating a gen5 2 vCore database..."
$database = New-AzSqlDatabase  -ResourceGroupName $resourceGroupName `
   -ServerName $serverName `
   -DatabaseName $databaseName `
   -Edition GeneralPurpose `
   -VCore 2 `
   -ComputeGeneration Gen5 `
   -MinimumCapacity 2 `
   -SampleName "AdventureWorksLT"
$database

# Create a secondary server in the failover region
Write-host "Creating a secondary logical server in the failover region..."
$drServer = New-AzSqlServer -ResourceGroupName $resourceGroupName `
   -ServerName $drServerName `
   -Location $drLocation `
   -SqlAdministratorCredentials $(New-Object -TypeName System.Management.Automation.PSCredential `
      -ArgumentList $adminlogin, $(ConvertTo-SecureString -String $password -AsPlainText -Force))
$drServer


# Create a failover group between the servers
$failovergroup = Write-host "Creating a failover group between the primary and secondary server..."
New-AzSqlDatabaseFailoverGroup `
   -ResourceGroupName $resourceGroupName `
   -ServerName $serverName `
   -PartnerServerName $drServerName  `
   -FailoverGroupName $failoverGroupName `
   -FailoverPolicy Automatic `
   -GracePeriodWithDataLossHours 2
$failovergroup

# Add the database to the failover group
Write-host "Adding the database to the failover group..." 
Get-AzSqlDatabase `
   -ResourceGroupName $resourceGroupName `
   -ServerName $serverName `
   -DatabaseName $databaseName | `
Add-AzSqlDatabaseToFailoverGroup `
   -ResourceGroupName $resourceGroupName `
   -ServerName $serverName `
   -FailoverGroupName $failoverGroupName
Write-host "Successfully added the database to the failover group..." 

# Check role of secondary replica
Write-host "Confirming the secondary replica is secondary...." 
(Get-AzSqlDatabaseFailoverGroup `
   -FailoverGroupName $failoverGroupName `
   -ResourceGroupName $resourceGroupName `
   -ServerName $drServerName).ReplicationRole

# Failover to secondary server
Write-host "Failing over failover group to the secondary..." 
Switch-AzSqlDatabaseFailoverGroup `
   -ResourceGroupName $resourceGroupName `
   -ServerName $drServerName `
   -FailoverGroupName $failoverGroupName
Write-host "Failed failover group successfully to" $drServerName 



# Revert failover to primary server
Write-host "Failing over failover group to the primary...." 
Switch-AzSqlDatabaseFailoverGroup `
   -ResourceGroupName $resourceGroupName `
   -ServerName $serverName `
   -FailoverGroupName $failoverGroupName
Write-host "Failed failover group successfully back to" $serverName


# Show randomized variables
Write-host "Resource group name is" $resourceGroupName 
Write-host "Password is" $password  
Write-host "Server name is" $serverName 
Write-host "DR Server name is" $drServerName 
Write-host "Failover group name is" $failoverGroupName

# Clean up resources by removing the resource group
# Write-host "Removing resource group..."
# Remove-AzResourceGroup -ResourceGroupName $resourceGroupName
# Write-host "Resource group removed =" $resourceGroupName
```

## <a name="clean-up-deployment"></a>清理部署

使用以下命令删除资源组及其相关的所有资源。

```powershell
Remove-AzResourceGroup -ResourceGroupName $resourceGroupName
```

## <a name="script-explanation"></a>脚本说明

此脚本使用以下命令。 表中的每条命令均链接到特定于命令的文档。

| Command | 说明 |
|---|---|
| [New-AzResourceGroup](https://docs.microsoft.com/powershell/module/az.resources/new-azresourcegroup) | 创建用于存储所有资源的资源组。 |
| [New-AzSqlServer](https://docs.microsoft.com/powershell/module/az.sql/new-azsqlserver) | 创建服务器。 |
| [New-AzSqlServerFirewallRule](https://docs.microsoft.com/powershell/module/az.sql/new-azsqlserverfirewallrule) | 为服务器创建服务器级防火墙规则。 |
| [New-AzSqlDatabase](https://docs.microsoft.com/powershell/module/az.sql/new-azsqldatabase) | 创建新数据库。 |
| [New-AzSqlDatabaseFailoverGroup](https://docs.microsoft.com/powershell/module/az.sql/new-azsqldatabasefailovergroup) | 新建故障转移组。 |
| [Get-AzSqlDatabase](https://docs.microsoft.com/powershell/module/az.sql/get-azsqldatabase) | 获取一个或多个数据库。 |
| [Add-AzSqlDatabaseToFailoverGroup](https://docs.microsoft.com/powershell/module/az.sql/add-azsqldatabasetofailovergroup) | 将一个或更多个数据库添加到故障转移组。 |
| [Get-AzSqlDatabaseFailoverGroup](https://docs.microsoft.com/powershell/module/az.sql/get-azsqldatabasefailovergroup) | 获取或列出故障转移组。 |
| [Switch-AzSqlDatabaseFailoverGroup](https://docs.microsoft.com/powershell/module/az.sql/switch-azsqldatabasefailovergroup)| 执行故障转移组的故障转移。 |
| [Remove-AzResourceGroup](https://docs.microsoft.com/powershell/module/az.resources/remove-azresourcegroup) | 删除资源组 |

## <a name="next-steps"></a>后续步骤

有关 Azure PowerShell 的详细信息，请参阅 [Azure PowerShell 文档](https://docs.microsoft.com/powershell/azure/overview)。

可以在 [Azure SQL 数据库 PowerShell 脚本](../powershell-script-content-guide.md)中找到更多 SQL 数据库 PowerShell 脚本示例。
