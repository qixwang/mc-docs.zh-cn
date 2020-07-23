---
title: PowerShell：配置弹性池活动异地复制
description: 为 Azure SQL 数据库中的共用数据库设置活动异地复制并进行故障转移的 Azure PowerShell 示例脚本。
services: sql-database
ms.service: sql-database
ms.subservice: high-availability
ms.custom: sqldbrb=1
ms.devlang: PowerShell
ms.topic: sample
author: WenJason
ms.author: v-jay
ms.reviewer: carlrab
origin.date: 03/12/2019
ms.date: 07/13/2020
ms.openlocfilehash: dc4668782d91e63a285ed4b28ac9836519e0e87b
ms.sourcegitcommit: fa26665aab1899e35ef7b93ddc3e1631c009dd04
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/10/2020
ms.locfileid: "86227194"
---
# <a name="use-powershell-to-configure-active-geo-replication-for-a-pooled-database-in-azure-sql-database"></a>使用 PowerShell 为 Azure SQL 数据库中的共用数据库配置活动异地复制
[!INCLUDE[appliesto-sqldb](../../includes/appliesto-sqldb.md)]

此 Azure PowerShell 脚本示例为 Azure SQL 数据库中的共用数据库配置活动异地复制，并将其故障转移到数据库的次要副本。

[!INCLUDE [quickstarts-free-trial-note](../../../../includes/quickstarts-free-trial-note.md)]
[!INCLUDE [updated-for-az](../../../../includes/updated-for-az.md)]

本教程需要 Az PowerShell 1.4.0 或更高版本。 如果需要进行升级，请参阅 [Install Azure PowerShell module](https://docs.microsoft.com/powershell/azure/install-az-ps)（安装 Azure PowerShell 模块）。 此外，还需要运行 `Connect-AzAccount -EnvironmentName AzureChinaCloud` 以创建与 Azure 的连接。

## <a name="sample-scripts"></a>示例脚本

```azurepowershell
# Connect-AzAccount -Environment AzureChinaCloud
$SubscriptionId = ''
# Set the resource group name and location for your serverw
$primaryResourceGroupName = "myPrimaryResourceGroup-$(Get-Random)"
$secondaryResourceGroupName = "mySecondaryResourceGroup-$(Get-Random)"
$primaryLocation = "chinaeast"
$secondaryLocation = "chinanorth"
# The logical server names have to be unique in the system
$primaryServerName = "primary-server-$(Get-Random)"
$secondaryServerName = "secondary-server-$(Get-Random)"
# Set an admin login and password for your servers
$adminSqlLgin = "SqlAdmin"
$password = "ChangeYourAdminPassword1"
# The sample database name
$databaseName = "mySampleDatabase"
# The ip address ranges that you want to allow to access your servers
$primaryStartIp = "0.0.0.0"
$primaryEndIp = "0.0.0.0"
$secondaryStartIp = "0.0.0.0"
$secondaryEndIp = "0.0.0.0"
# The elastic pool names
$primaryPoolName = "PrimaryPool"
$secondarypoolname = "SecondaryPool"

# Set subscription 
Set-AzContext -SubscriptionId $subscriptionId 

# Create two new resource groups
$primaryResourceGroup = New-AzResourceGroup -Name $primaryResourceGroupName -Location $primaryLocation
$secondaryResourceGroup = New-AzResourceGroup -Name $secondaryResourceGroupName -Location $secondaryLocation

# Create two new logical servers with a system wide unique server name

$primaryServer = New-AzSqlServer -ResourceGroupName $primaryResourceGroupName `
    -ServerName $primaryServerName `
    -Location $primaryLocation `
    -SqlAdministratorCredentials $(New-Object -TypeName System.Management.Automation.PSCredential -ArgumentList $adminSqlLgin, $(ConvertTo-SecureString -String $password -AsPlainText -Force))
$secondaryServer = New-AzSqlServer -ResourceGroupName $secondaryResourceGroupName `
    -ServerName $secondaryServerName `
    -Location $secondaryLocation `
    -SqlAdministratorCredentials $(New-Object -TypeName System.Management.Automation.PSCredential -ArgumentList $adminSqlLgin, $(ConvertTo-SecureString -String $password -AsPlainText -Force))

# Create a server firewall rule for each server that allows access from the specified IP range
$primaryServerFirewallRule = New-AzSqlServerFirewallRule -ResourceGroupName $primaryResourceGroupName `
    -ServerName $primaryServerName `
    -FirewallRuleName "AllowedIPs" -StartIpAddress $primaryStartIp -EndIpAddress $primaryEndIp
$secondaryServerFirewallRule = New-AzSqlServerFirewallRule -ResourceGroupName $secondaryResourceGroupName `
    -ServerName $secondaryServerName `
    -FirewallRuleName "AllowedIPs" -StartIpAddress $secondaryStartIp -EndIpAddress $secondaryEndIp

# Create a pool in each of the servers
$primaryPool = New-AzSqlElasticPool -ResourceGroupName $primaryResourceGroupName `
    -ServerName $primaryServerName `
    -ElasticPoolName $primaryPoolName `
    -Edition "Standard" `
    -Dtu 50 `
    -DatabaseDtuMin 10 `
    -DatabaseDtuMax 50
$secondaryPool = New-AzSqlElasticPool -ResourceGroupName $secondaryResourceGroupName `
    -ServerName $secondaryServerName `
    -ElasticPoolName $secondaryPoolName `
    -Edition "Standard" `
    -Dtu 50 `
    -DatabaseDtuMin 10 `
    -DatabaseDtuMax 50

# Create a blank database in the pool on the primary server
$database = New-AzSqlDatabase  -ResourceGroupName $primaryResourceGroupName `
    -ServerName $primaryServerName `
    -DatabaseName $databaseName `
    -ElasticPoolName $primaryPoolName

# Establish Active Geo-Replication
$database = Get-AzSqlDatabase -ResourceGroupName $primaryResourceGroupName `
    -ServerName $primaryServerName `
    -DatabaseName $databaseName
$database | New-AzSqlDatabaseSecondary -PartnerResourceGroupName $secondaryResourceGroupName `
    -PartnerServerName $secondaryServerName `
    -SecondaryElasticPoolName $secondaryPoolName `
    -AllowConnections "All"

# Initiate a planned failover
$database = Get-AzSqlDatabase -ResourceGroupName $secondaryResourceGroupName `
    -ServerName $secondaryServerName `
    -DatabaseName $databaseName 
$database | Set-AzSqlDatabaseSecondary -PartnerResourceGroupName $primaryResourceGroupName -Failover

    
# Monitor Geo-Replication config and health after failover
$database = Get-AzSqlDatabase -ResourceGroupName $secondaryResourceGroupName `
    -ServerName $secondaryServerName `
    -DatabaseName $databaseName
$database | Get-AzSqlDatabaseReplicationLink -PartnerResourceGroupName $primaryResourceGroupName `
    -PartnerServerName $primaryServerName

# Clean up deployment 
# Remove-AzResourceGroup -ResourceGroupName $primaryResourceGroupName
# Remove-AzResourceGroup -ResourceGroupName $secondaryResourceGroupName
```

## <a name="clean-up-deployment"></a>清理部署

使用以下命令删除资源组及其相关的所有资源。

```powershell
Remove-AzResourceGroup -ResourceGroupName $primaryresourcegroupname
Remove-AzResourceGroup -ResourceGroupName $secondaryresourcegroupname
```

## <a name="script-explanation"></a>脚本说明

此脚本使用以下命令。 表中的每条命令链接到特定于命令的文档。

| 命令 | 注释 |
|---|---|
| [New-AzResourceGroup](https://docs.microsoft.com/powershell/module/az.resources/new-azresourcegroup) | 创建用于存储所有资源的资源组。 |
| [New-AzSqlServer](https://docs.microsoft.com/powershell/module/az.sql/new-azsqlserver) | 创建托管数据库和弹性池的服务器。 |
| [New-AzSqlElasticPool](https://docs.microsoft.com/powershell/module/az.sql/new-azsqlelasticpool) | 创建弹性池。 |
| [New-AzSqlDatabase](https://docs.microsoft.com/powershell/module/az.sql/new-azsqldatabase) | 在服务器中创建数据库。 |
| [Set-AzSqlDatabase](https://docs.microsoft.com/powershell/module/az.sql/set-azsqldatabase) | 更新数据库属性，或者将数据库移入、移出弹性池或在弹性池之间移动。 |
| [New-AzSqlDatabaseSecondary](https://docs.microsoft.com/powershell/module/az.sql/new-azsqldatabasesecondary)| 为现有数据库创建辅助数据库，并开始数据复制。 |
| [Get-AzSqlDatabase](https://docs.microsoft.com/powershell/module/az.sql/get-azsqldatabase)| 获取一个或多个数据库。 |
| [Set-AzSqlDatabaseSecondary](https://docs.microsoft.com/powershell/module/az.sql/set-azsqldatabasesecondary)| 将辅助数据库切换为主数据库，以便启动故障转移。|
| [Get-AzSqlDatabaseReplicationLink](https://docs.microsoft.com/powershell/module/az.sql/get-azsqldatabasereplicationlink) | 获取 Azure SQL 数据库和资源组或逻辑 SQL 服务器之间的异地复制链路。 |
| [Remove-AzResourceGroup](https://docs.microsoft.com/powershell/module/az.resources/remove-azresourcegroup) | 删除资源组，包括所有嵌套的资源。 |
|||

## <a name="next-steps"></a>后续步骤

有关 Azure PowerShell 的详细信息，请参阅 [Azure PowerShell 文档](https://docs.microsoft.com/powershell/azure/overview)。

可以在 [Azure SQL 数据库 PowerShell 脚本](../powershell-script-content-guide.md)中找到更多 SQL 数据库 PowerShell 脚本示例。
