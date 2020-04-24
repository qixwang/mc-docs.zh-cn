---
title: PowerShell 示例 - 移动 Azure SQL 数据库 - 弹性池 | Microsoft Docs
description: 使用 PowerShell 在弹性池之间移动 SQL 数据库的 Azure PowerShell 示例脚本
services: sql-database
ms.service: sql-database
ms.subservice: elastic-pools
ms.custom: ''
ms.devlang: PowerShell
ms.topic: sample
author: WenJason
ms.reviewer: ''
ms.author: v-jay
manager: digimobile
origin.date: 03/12/2019
ms.date: 04/29/2019
ms.openlocfilehash: 065b2dd2f201f80565ed673d439f7850d23366ff
ms.sourcegitcommit: c1ba5a62f30ac0a3acb337fb77431de6493e6096
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2020
ms.locfileid: "64854751"
---
# <a name="use-powershell-to-create-elastic-pools-and-move-databases-between-elastic-pools"></a>使用 PowerShell 创建弹性池，并在弹性池之间移动数据库

此 PowerShell 脚本示例创建两个弹性池，将数据库从一个弹性池移到另一个弹性池中，再将数据库移出弹性池，以实现单一数据库计算大小。

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
# Set elastic poool names
$firstPoolName = "MyFirstPool"
$secondPoolName = "MySecondPool"
# Set an admin login and password for your server
$adminSqlLogin = "SqlAdmin"
$password = "ChangeYourAdminPassword1"
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
    -DatabaseName "MySecondSampleDatabase" `
    -ElasticPoolName "MySecondPool"

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
| [New-AzSqlServer](https://docs.microsoft.com/powershell/module/az.sql/new-azsqlserver) | 创建用于承载单一数据库或弹性池的 SQL 数据库服务器。 |
| [New-AzSqlElasticPool](https://docs.microsoft.com/powershell/module/az.sql/new-azsqlelasticpool) | 创建弹性池。 |
| [New-AzSqlDatabase](https://docs.microsoft.com/powershell/module/az.sql/new-azsqldatabase) | 在 SQL 数据库服务器中创建数据库作为独立数据库或共用数据库。 |
| [Set-AzSqlDatabase](https://docs.microsoft.com/powershell/module/az.sql/set-azsqldatabase) | 更新数据库属性，或者将数据库移入、移出弹性池或在弹性池之间移动。 |
| [Remove-AzResourceGroup](https://docs.microsoft.com/powershell/module/az.resources/remove-azresourcegroup) | 删除资源组，包括所有嵌套的资源。 |
|||

## <a name="next-steps"></a>后续步骤

有关 Azure PowerShell 的详细信息，请参阅 [Azure PowerShell 文档](https://docs.microsoft.com/powershell/azure/overview)。

可以在 [Azure SQL 数据库 PowerShell 脚本](../sql-database-powershell-samples.md)中找到更多 SQL 数据库 PowerShell 脚本示例。
