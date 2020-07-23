---
title: 使用 PowerShell 监视和缩放 Azure SQL 数据库中的单一数据库
description: 使用 Azure PowerShell 示例脚本监视和缩放 Azure SQL 数据库中的单一数据库。
services: sql-database
ms.service: sql-database
ms.subservice: performance
ms.custom: sqldbrb=1
ms.devlang: PowerShell
ms.topic: sample
author: WenJason
ms.author: v-jay
ms.reviewer: carlrab
origin.date: 03/12/2019
ms.date: 07/13/2019
ms.openlocfilehash: 045806754b0fdf4960a55a907ebb242d3d1493ee
ms.sourcegitcommit: fa26665aab1899e35ef7b93ddc3e1631c009dd04
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/10/2020
ms.locfileid: "86227727"
---
# <a name="use-powershell-to-monitor-and-scale-a-single-database-in-azure-sql-database"></a>使用 PowerShell 监视和缩放 Azure SQL 数据库中的单一数据库

[!INCLUDE[appliesto-sqldb](../../includes/appliesto-sqldb.md)]

此 PowerShell 脚本示例监视单一数据库的性能指标，将其缩放到更高的计算大小，并根据某个性能指标创建预警规则。

[!INCLUDE [quickstarts-free-trial-note](../../../../includes/quickstarts-free-trial-note.md)]
[!INCLUDE [updated-for-az](../../../../includes/updated-for-az.md)]

本教程需要 Az PowerShell 1.4.0 或更高版本。 如果需要进行升级，请参阅 [Install Azure PowerShell module](https://docs.microsoft.com/powershell/azure/install-az-ps)（安装 Azure PowerShell 模块）。 此外，还需要运行 `Connect-AzAccount -EnvironmentName AzureChinaCloud` 以创建与 Azure 的连接。

## <a name="sample-script"></a>示例脚本

```azurepowershell
# Connect-AzAccount -Environment AzureChinaCloud
# The SubscriptionId in which to create these objects
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
$serverFirewallRule = New-AzSqlServerFirewallRule -ResourceGroupName $resourceGroupName `
    -ServerName $serverName `
    -FirewallRuleName "AllowedIPs" -StartIpAddress $startIp -EndIpAddress $endIp

# Create a blank database with an S0 performance level
$database = New-AzSqlDatabase  -ResourceGroupName $resourceGroupName `
    -ServerName $serverName `
    -DatabaseName $databaseName `
    -RequestedServiceObjectiveName "S0" `
    -SampleName "AdventureWorksLT"

# Monitor the DTU consumption on the imported database in 5 minute intervals
$MonitorParameters = @{
  ResourceId = "/subscriptions/$($(Get-AzContext).Subscription.Id)/resourceGroups/$resourceGroupName/providers/Microsoft.Sql/servers/$serverName/databases/$databaseName"
  TimeGrain = [TimeSpan]::Parse("00:05:00")
  MetricNames = "dtu_consumption_percent"
}
(Get-AzMetric @MonitorParameters -DetailedOutput).MetricValues

# Scale the database performance to Standard S1
$database = Set-AzSqlDatabase -ResourceGroupName $resourceGroupName `
    -ServerName $servername `
    -DatabaseName $databasename `
    -Edition "Standard" `
    -RequestedServiceObjectiveName "S1"

# Set an alert rule to automatically monitor DTU in the future
Add-AzMetricAlertRule -ResourceGroup $resourceGroupName `
    -Name "MySampleAlertRule" `
    -Location $location `
    -TargetResourceId "/subscriptions/$($(Get-AzContext).Subscription.Id)/resourceGroups/$resourceGroupName/providers/Microsoft.Sql/servers/$serverName/databases/$databaseName" `
    -MetricName "dtu_consumption_percent" `
    -Operator "GreaterThan" `
    -Threshold 90 `
    -WindowSize $([TimeSpan]::Parse("00:05:00")) `
    -TimeAggregationOperator "Average" `
    -Action $(New-AzAlertRuleEmail -SendToServiceOwner)

# Clean up deployment 
# Remove-AzResourceGroup -ResourceGroupName $resourceGroupName
```

> [!NOTE]
> 有关指标的完整列表，请参阅[支持的指标](../../../azure-monitor/platform/metrics-supported.md#microsoftsqlserversdatabases)。
> [!TIP]
> 使用 [Get-AzSqlDatabaseActivity](https://docs.microsoft.com/powershell/module/az.sql/get-azsqldatabaseactivity) 获取数据库操作的状态，并使用 [Stop-AzSqlDatabaseActivity](https://docs.microsoft.com/powershell/module/az.sql/stop-azsqldatabaseactivity) 取消数据库更新操作。

## <a name="clean-up-deployment"></a>清理部署

使用以下命令删除资源组及其相关的所有资源。

```powershell
Remove-AzResourceGroup -ResourceGroupName $resourcegroupname
```

## <a name="script-explanation"></a>脚本说明

此脚本使用以下命令。 表中的每条命令链接到特定于命令的文档。

| 命令 | 注释 |
|---|---|
 [New-AzResourceGroup](https://docs.microsoft.com/powershell/module/az.resources/new-azresourcegroup) | 创建用于存储所有资源的资源组。 |
| [New-AzSqlServer](https://docs.microsoft.com/powershell/module/az.sql/new-azsqlserver) | 创建托管单一数据库或弹性池的服务器。 |
| [Get-AzMetric](https://docs.microsoft.com/powershell/module/az.monitor/get-azmetric) | 显示数据库的大小使用情况信息。|
| [Set-AzSqlDatabase](https://docs.microsoft.com/powershell/module/az.sql/set-azsqldatabase) | 更新数据库属性，或者将数据库移入、移出弹性池或在弹性池之间移动。 |
| [Add-AzMetricAlertRule](https://docs.microsoft.com/powershell/module/az.monitor/add-azmetricalertrule) | 设置预警规则，以便在将来自动监视指标。 |
| [Remove-AzResourceGroup](https://docs.microsoft.com/powershell/module/az.resources/remove-azresourcegroup) | 删除资源组，包括所有嵌套的资源。 |
|||

## <a name="next-steps"></a>后续步骤

有关 Azure PowerShell 的详细信息，请参阅 [Azure PowerShell 文档](https://docs.microsoft.com/powershell/azure/overview)。

可以在 [Azure PowerShell 脚本](../powershell-script-content-guide.md)中找到其他 PowerShell 脚本示例。
