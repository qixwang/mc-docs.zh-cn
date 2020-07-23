---
title: PowerShell：在弹性池之间移动数据库
description: 使用 Azure PowerShell 示例脚本在两个弹性池之间移动 SQL 数据库中的数据库。
services: sql-database
ms.service: sql-database
ms.subservice: elastic-pools
ms.custom: sqldbrb=1
ms.devlang: PowerShell
ms.topic: sample
author: WenJason
ms.reviewer: ''
ms.author: v-jay
origin.date: 03/12/2019
ms.date: 07/13/2020
ms.openlocfilehash: b8cf9b936855caa5644cc26050eed7e489c3cab2
ms.sourcegitcommit: fa26665aab1899e35ef7b93ddc3e1631c009dd04
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/10/2020
ms.locfileid: "86227725"
---
# <a name="use-powershell-to-create-elastic-pools-and-move-a-database-between-them"></a>使用 PowerShell 创建弹性池，并在其间移动数据库

[!INCLUDE[appliesto-sqldb](../../includes/appliesto-sqldb.md)]

此 PowerShell 脚本示例创建两个弹性池，将 SQL 数据库中的池数据库从一个 SQL 弹性池移动到另一个 SQL 弹性池中，然后将池数据库从 SQL 弹性池移出，成为 Azure SQL 数据库中的单个数据库。

[!INCLUDE [quickstarts-free-trial-note](../../../../includes/quickstarts-free-trial-note.md)]
[!INCLUDE [updated-for-az](../../../../includes/updated-for-az.md)]

本教程需要 Az PowerShell 1.4.0 或更高版本。 如果需要进行升级，请参阅 [Install Azure PowerShell module](https://docs.microsoft.com/powershell/azure/install-az-ps)（安装 Azure PowerShell 模块）。 此外，还需要运行 `Connect-AzAccount -EnvironmentName AzureChinaCloud` 以创建与 Azure 的连接。

## <a name="sample-script"></a>示例脚本

```azurepowershell
# Connect-AzAccount -Environment AzureChinaCloud
$SubscriptionId = ''
# Set the resource group name and location for your server
$resourceGroupName = "myResourceGroup-$(Get-Random)"
$location = "chinaeast"
# Set elastic pool names
$firstPoolName = "MyFirstPool"
$secondPoolName = "MySecondPool"
# Set an admin login and password for your server
$adminSqlLogin = "SqlAdmin"
$password = "<EnterYourComplexPasswordHere>"
# The logical server name has to be unique in the system
$serverName = "server-$(Get-Random)"
# The sample database names
$firstDatabaseName = "myFirstSampleDatabase"
$secondDatabaseName = "mySecondSampleDatabase"
# The ip address range that you want to allow to access your server
$startIp = "0.0.0.0"
$endIp = "0.0.0.0"

# Set subscription 
Set-AzContext -SubscriptionId $subscriptionId 

# Create a new resource group
$resourceGroup = New-AzResourceGroup -Name $resourceGroupName -Location $location

# Create a new server with a system wide unique server name
$server = New-AzSqlServer -ResourceGroupName $resourceGroupName `
    -ServerName $serverName `
    -Location $location `
    -SqlAdministratorCredentials $(New-Object -TypeName System.Management.Automation.PSCredential -ArgumentList $adminSqlLogin, $(ConvertTo-SecureString -String $password -AsPlainText -Force))

# Create a server firewall rule that allows access from the specified IP range
$serverFirewallRule = New-AzSqlServerFirewallRule -ResourceGroupName $resourceGroupName `
    -ServerName $serverName `
    -FirewallRuleName "AllowedIPs" -StartIpAddress $startIp -EndIpAddress $endIp

# Create two elastic database pools
$firstPool = New-AzSqlElasticPool -ResourceGroupName $resourceGroupName `
    -ServerName $servername `
    -ElasticPoolName $firstPoolName `
    -Edition "Standard" `
    -Dtu 50 `
    -DatabaseDtuMin 10 `
    -DatabaseDtuMax 20
$secondPool = New-AzSqlElasticPool -ResourceGroupName $resourceGroupName `
    -ServerName $serverName `
    -ElasticPoolName $secondPoolName `
    -Edition "Standard" `
    -Dtu 50 `
    -DatabaseDtuMin 10 `
    -DatabaseDtuMax 50

# Create two blank databases in the first pool
$firstDatabase = New-AzSqlDatabase  -ResourceGroupName $resourceGroupName `
    -ServerName $serverName `
    -DatabaseName $firstDatabaseName `
    -ElasticPoolName $firstPoolName
$secondDatabase = New-AzSqlDatabase  -ResourceGroupName $resourceGroupName `
    -ServerName $serverName `
    -DatabaseName $secondDatabaseName `
    -ElasticPoolName $secondPoolName

# Move the database to the second pool
$firstDatabase = Set-AzSqlDatabase -ResourceGroupName $resourceGroupName `
    -ServerName $serverName `
    -DatabaseName $firstDatabaseName `
    -ElasticPoolName $secondPoolName

# Move the database into a standalone performance level
$firstDatabase = Set-AzSqlDatabase -ResourceGroupName $resourceGroupName `
    -ServerName $serverName `
    -DatabaseName $firstDatabaseName `
    -RequestedServiceObjectiveName "S0"

# Clean up deployment 
# Remove-AzResourceGroup -ResourceGroupName $resourceGroupName
```

## <a name="clean-up-deployment"></a>清理部署

使用以下命令删除资源组及其相关的所有资源。

```powershell
Remove-AzResourceGroup -ResourceGroupName $resourcegroupname
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
| [Remove-AzResourceGroup](https://docs.microsoft.com/powershell/module/az.resources/remove-azresourcegroup) | 删除资源组，包括所有嵌套的资源。 |
|||

## <a name="next-steps"></a>后续步骤

有关 Azure PowerShell 的详细信息，请参阅 [Azure PowerShell 文档](https://docs.microsoft.com/powershell/azure/overview)。

可以在 [Azure SQL 数据库 PowerShell 脚本](../powershell-script-content-guide.md)中找到更多 SQL 数据库 PowerShell 脚本示例。
