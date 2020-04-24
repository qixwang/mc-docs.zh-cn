---
title: PowerShell 示例 - 故障转移组 - Azure SQL 数据库弹性池
description: Azure PowerShell 示例脚本，用于创建 Azure SQL 数据库弹性池，将其添加到故障转移组，然后测试故障转移。
services: sql-database
ms.service: sql-database
ms.subservice: high-availability
ms.custom: ''
ms.devlang: PowerShell
ms.topic: sample
author: WenJason
ms.author: v-jay
ms.reviewer: carlrab
origin.date: 07/16/2019
ms.date: 12/16/2019
ms.openlocfilehash: d28a94cebcc19fa50b696f2efabed6967a2fd46e
ms.sourcegitcommit: c1ba5a62f30ac0a3acb337fb77431de6493e6096
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2020
ms.locfileid: "75348504"
---
# <a name="use-powershell-to-add-an-azure-sql-database-elastic-pool-to-a-failover-group"></a>使用 PowerShell 将 Azure SQL 数据库弹性池添加到故障转移组 

此 PowerShell 脚本示例创建一个单一数据库，将其添加到弹性池，创建一个故障转移组，然后测试故障转移。 

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]
[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

本教程需要 AZ PowerShell 1.4.0 或更高版本。 如果需要升级，请参阅[安装 Azure PowerShell 模块](https://docs.microsoft.com/powershell/azure/install-az-ps)。 如果在本地运行 PowerShell，则还需运行 `Connect-AzAccount -Environment AzureChinaCloud` 来创建与 Azure 的连接。

## <a name="sample-scripts"></a>示例脚本

```azurepowershell
# Set variables for your server and database
$subscriptionId = '<Subscription-ID>'
$randomIdentifier = $(Get-Random)
$resourceGroupName = "myResourceGroup-$randomIdentifier"
$location = "China East 2"
$adminLogin = "azureuser"
$password = "PWD27!"+(New-Guid).Guid
$serverName = "mysqlserver-$randomIdentifier"
$poolName = "myElasticPool"
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

# Create a server with a system-wide unique server name
Write-host "Creating primary logical server..."
New-AzSqlServer -ResourceGroupName $resourceGroupName `
   -ServerName $serverName `
   -Location $location `
   -SqlAdministratorCredentials $(New-Object -TypeName System.Management.Automation.PSCredential `
   -ArgumentList $adminLogin, $(ConvertTo-SecureString -String $password -AsPlainText -Force))
Write-host "Primary logical server = " $serverName

# Create a server firewall rule that allows access from the specified IP range
Write-host "Configuring firewall for primary logical server..."
New-AzSqlServerFirewallRule -ResourceGroupName $resourceGroupName `
   -ServerName $serverName `
   -FirewallRuleName "AllowedIPs" -StartIpAddress $startIp -EndIpAddress $endIp
Write-host "Firewall configured" 

# Create General Purpose Gen5 database with 2 vCore
Write-host "Creating a gen5 2 vCore database..."
$database = New-AzSqlDatabase  -ResourceGroupName $resourceGroupName `
   -ServerName $serverName `
   -DatabaseName $databaseName `
   -Edition "GeneralPurpose" `
   -VCore 2 `
   -ComputeGeneration Gen5 `
   -MinimumCapacity 1 `
   -SampleName "AdventureWorksLT"
$database

# Create primary Gen5 elastic 2 vCore pool
Write-host "Creating elastic pool..."
$elasticPool = New-AzSqlElasticPool -ResourceGroupName $resourceGroupName `
    -ServerName $serverName `
    -ElasticPoolName $poolName `
    -Edition "GeneralPurpose" `
    -vCore 2 `
    -ComputeGeneration Gen5
$elasticPool

# Add single db into elastic pool
Write-host "Creating elastic pool..."
$addDatabase = Set-AzSqlDatabase -ResourceGroupName $resourceGroupName `
    -ServerName $serverName `
    -DatabaseName $databaseName `
    -ElasticPoolName $poolName
$addDatabase

# Create a secondary server in the failover region
Write-host "Creating a secondary logical server in the failover region..."
New-AzSqlServer -ResourceGroupName $resourceGroupName `
   -ServerName $drServerName `
   -Location $drLocation `
   -SqlAdministratorCredentials $(New-Object -TypeName System.Management.Automation.PSCredential `
      -ArgumentList $adminlogin, $(ConvertTo-SecureString -String $password -AsPlainText -Force))
Write-host "Secondary logical server =" $drServerName

# Create a server firewall rule that allows access from the specified IP range
Write-host "Configuring firewall for secondary logical server..."
New-AzSqlServerFirewallRule -ResourceGroupName $resourceGroupName `
   -ServerName $drServerName `
   -FirewallRuleName "AllowedIPs" -StartIpAddress $startIp -EndIpAddress $endIp
Write-host "Firewall configured" 

# Create secondary Gen5 elastic 2 vCore pool
Write-host "Creating secondary elastic pool..."
$elasticPool = New-AzSqlElasticPool -ResourceGroupName $resourceGroupName `
    -ServerName $drServerName `
    -ElasticPoolName $poolName `
    -Edition "GeneralPurpose" `
    -vCore 2 `
    -ComputeGeneration Gen5
$elasticPool


# Create a failover group between the servers
Write-host "Creating failover group..." 
New-AzSqlDatabaseFailoverGroup `
  -ResourceGroupName $resourceGroupName `
   -ServerName $serverName `
   -PartnerServerName $drServerName  `
   -FailoverGroupName $failoverGroupName `
   -FailoverPolicy Automatic `
   -GracePeriodWithDataLossHours 2
Write-host "Failover group created successfully." 

# Add elastic pool to the failover group
Write-host "Enumerating databases in elastic pool...." 
$FailoverGroup = Get-AzSqlDatabaseFailoverGroup `
                 -ResourceGroupName $resourceGroupName `
                 -ServerName $serverName `
                 -FailoverGroupName $failoverGroupName
$databases = Get-AzSqlElasticPoolDatabase `
               -ResourceGroupName $resourceGroupName `
               -ServerName $serverName `
               -ElasticPoolName $poolName
Write-host "Adding databases to failover group..." 
$failoverGroup = $failoverGroup | Add-AzSqlDatabaseToFailoverGroup `
                                  -Database $databases 
$failoverGroup

# Check role of secondary replica
Write-host "Confirming the secondary server is secondary...." 
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
Write-host "Failover group failed over to" $drServerName 

# Check role of secondary replica
Write-host "Confirming the secondary server is now primary" 
(Get-AzSqlDatabaseFailoverGroup `
   -FailoverGroupName $failoverGroupName `
   -ResourceGroupName $resourceGroupName `
   -ServerName $drServerName).ReplicationRole

# Revert failover to primary server
Write-host "Failing over failover group to the primary...." 
Switch-AzSqlDatabaseFailoverGroup `
   -ResourceGroupName $resourceGroupName `
   -ServerName $serverName `
   -FailoverGroupName $failoverGroupName
Write-host "Failover group failed over to" $serverName 

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
| [New-AzSqlServer](https://docs.microsoft.com/powershell/module/az.sql/new-azsqlserver) | 创建托管单一数据库和弹性池的 SQL 数据库服务器。 |
| [New-AzSqlServerFirewallRule](https://docs.microsoft.com/powershell/module/az.sql/new-azsqlserverfirewallrule) | 为逻辑服务器创建防火墙规则。 | 
| [New-AzSqlDatabase](https://docs.microsoft.com/powershell/module/az.sql/new-azsqldatabase) | 新建 Azure SQL 数据库单一数据库。 | 
| [New-AzSqlElasticPool](https://docs.microsoft.com/powershell/module/az.sql/new-azsqlelasticpool) | 为 Azure SQL 数据库创建弹性数据库池。| 
| [Set-AzSqlDatabase](https://docs.microsoft.com/powershell/module/az.sql/set-azsqldatabase) | 设置数据库的属性，或将现有数据库移到弹性池中。 | 
| [New-AzSqlDatabaseFailoverGroup](https://docs.microsoft.com/powershell/module/az.sql/new-azsqldatabasefailovergroup) | 新建故障转移组。 |
| [Get-AzSqlDatabase](https://docs.microsoft.com/powershell/module/az.sql/get-azsqldatabase) | 获取一个或更多个 SQL 数据库。 |
| [Add-AzSqlDatabaseToFailoverGroup](https://docs.microsoft.com/powershell/module/az.sql/add-azsqldatabasetofailovergroup) | 将一个或更多个 Azure SQL 数据库添加到故障转移组。 |
| [Get-AzSqlDatabaseFailoverGroup](https://docs.microsoft.com/powershell/module/az.sql/get-azsqldatabasefailovergroup) | 获取或列出 Azure SQL 数据库故障转移组。 |
| [Switch-AzSqlDatabaseFailoverGroup](https://docs.microsoft.com/powershell/module/az.sql/switch-azsqldatabasefailovergroup)| 执行 Azure SQL 数据库故障转移组的故障转移。 |
| [Remove-AzResourceGroup](https://docs.microsoft.com/powershell/module/az.resources/remove-azresourcegroup) | 删除资源组 | 

## <a name="next-steps"></a>后续步骤

有关 Azure PowerShell 的详细信息，请参阅 [Azure PowerShell 文档](https://docs.microsoft.com/powershell/azure/overview)。

可以在 [Azure SQL 数据库 PowerShell 脚本](../sql-database-powershell-samples.md)中找到更多 SQL 数据库 PowerShell 脚本示例。
