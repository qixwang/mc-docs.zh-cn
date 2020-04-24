---
title: PowerShell 示例 - 监视 - 缩放 - 弹性池 - Azure SQL 数据库 | Microsoft Docs
description: 用于在 Azure SQL 数据库中监视和缩放弹性池的 Azure PowerShell 脚本示例
services: sql-database
ms.service: sql-database
ms.subservice: performance
ms.custom: ''
ms.devlang: PowerShell
ms.topic: sample
author: WenJason
ms.author: v-jay
ms.reviewer: carlrab
manager: digimobile
origin.date: 03/12/2019
ms.date: 04/29/2019
ms.openlocfilehash: 1b1a143067f9e5e9694ed687373836adc47ab6dc
ms.sourcegitcommit: c1ba5a62f30ac0a3acb337fb77431de6493e6096
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2020
ms.locfileid: "64854677"
---
# <a name="use-powershell-to-monitor-and-scale-an-elastic-pool-in-azure-sql-database"></a>使用 PowerShell 在 Azure SQL 数据库中监视和缩放弹性池

此 PowerShell 脚本示例监视弹性池的性能指标，将其扩展到更高的计算大小，并基于性能指标之一创建警报规则。

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
# Set elastic pool names
$poolName = "MySamplePool"
# Set an admin login and password for your database
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

# Create elastic database pool
$elasticPool = New-AzSqlElasticPool -ResourceGroupName $resourceGroupName `
    -ServerName $serverName `
    -ElasticPoolName $poolName `
    -Edition "Standard" `
    -Dtu 50 `
    -DatabaseDtuMin 10 `
    -DatabaseDtuMax 50

# Create a server firewall rule that allows access from the specified IP range
$serverFirewallRule = New-AzSqlServerFirewallRule -ResourceGroupName $resourceGroupName `
    -ServerName $serverName `
    -FirewallRuleName "AllowedIPs" -StartIpAddress $startIp -EndIpAddress $endIp

# Create two blank database in the pool
$firstDatabase = New-AzSqlDatabase  -ResourceGroupName $resourceGroupName `
    -ServerName $serverName `
    -DatabaseName $firstDatabaseName `
    -ElasticPoolName $poolName
$secondDatabase = New-AzSqlDatabase  -ResourceGroupName $resourceGroupName `
    -ServerName $serverName `
    -DatabaseName $secondDatabaseName `
    -ElasticPoolName $poolName

# Monitor the pool
$monitorparameters = @{
  ResourceId = "/subscriptions/$($(Get-AzContext).Subscription.Id)/resourceGroups/$resourceGroupName/providers/Microsoft.Sql/servers/$serverName/elasticPools/$poolName"
  TimeGrain = [TimeSpan]::Parse("00:05:00")
  MetricNames = "dtu_consumption_percent"
}
(Get-AzMetric @monitorparameters -DetailedOutput).MetricValues

# Scale the pool
$elasticPool = Set-AzSqlElasticPool -ResourceGroupName $resourceGroupName `
    -ServerName $serverName `
    -ElasticPoolName $poolName `
    -Edition "Standard" `
    -Dtu 100 `
    -DatabaseDtuMin 20 `
    -DatabaseDtuMax 100

# Add an alert that fires when the pool utilization reaches 90%
Add-AzMetricAlertRule -ResourceGroup $resourceGroupName `
    -Name "mySampleAlertRule" `
    -Location $location `
    -TargetResourceId "/subscriptions/$($(Get-AzContext).Subscription.Id)/resourceGroups/$resourceGroupName/providers/Microsoft.Sql/servers/$serverName/elasticPools/$poolName" `
    -MetricName "dtu_consumption_percent" `
    -Operator "GreaterThan" `
    -Threshold 90 `
    -WindowSize $([TimeSpan]::Parse("00:05:00")) `
    -TimeAggregationOperator "Average" `
    -Action $(New-AzAlertRuleEmail -SendToServiceOwner)
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
| [New-AzSqlElasticPool](https://docs.microsoft.com/powershell/module/az.sql/new-azsqlelasticpool) | 创建弹性池。 |
| [New-AzSqlDatabase](https://docs.microsoft.com/powershell/module/az.sql/new-azsqldatabase) | 创建单一数据库或创建弹性池中的数据库。 |
| [Get-AzMetric](https://docs.microsoft.com/powershell/module/az.monitor/get-azmetric) | 显示数据库的大小使用情况信息。|
| [Add-AzMetricAlertRule](https://docs.microsoft.com/powershell/module/az.monitor/add-azmetricalertrule) | 添加或更新基于指标的警报规则。 |
| [Set-AzSqlElasticPool](https://docs.microsoft.com/powershell/module/az.sql/set-azsqlelasticpool) | 更新弹性池属性 |
| [Add-AzMetricAlertRule](https://docs.microsoft.com/powershell/module/az.monitor/add-azmetricalertrule) | 设置警报规则，以便在将来自动监视 DTU。 |
| [Remove-AzResourceGroup](https://docs.microsoft.com/powershell/module/az.resources/remove-azresourcegroup) | 删除资源组，包括所有嵌套的资源。 |
|||

## <a name="next-steps"></a>后续步骤

有关 Azure PowerShell 的详细信息，请参阅 [Azure PowerShell 文档](https://docs.microsoft.com/powershell/azure/overview)。

可以在 [Azure SQL 数据库 PowerShell 脚本](../sql-database-powershell-samples.md)中找到更多 SQL 数据库 PowerShell 脚本示例。
