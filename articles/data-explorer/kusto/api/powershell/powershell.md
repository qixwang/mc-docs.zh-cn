---
title: PowerShell 提供的 Kusto .NET 客户端库 - Azure 数据资源管理器
description: 本文介绍如何在 Azure 数据资源管理器中使用 PowerShell 提供的 .NET 客户端库。
services: data-explorer
author: orspod
ms.author: v-tawe
ms.reviewer: rkarlin
ms.service: data-explorer
ms.topic: reference
origin.date: 05/29/2019
ms.date: 07/01/2020
ms.openlocfilehash: 65ba9bf2bc465ba85a7cd5610e410e7e30a596fc
ms.sourcegitcommit: 9bc3e55f01e0999f05e7b4ebaea95f3ac91d32eb
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/10/2020
ms.locfileid: "86226338"
---
# <a name="using-the-net-client-libraries-from-powershell"></a>使用 PowerShell 提供的 .NET 客户端库

PowerShell 脚本可以通过 PowerShell 内置的与任意（非 PowerShell）.NET 库的集成来使用 Azure 数据资源管理器 .NET 客户端库。

## <a name="getting-the-net-client-libraries-for-scripting-with-powershell"></a>获取 .NET 客户端库以使用 PowerShell 编写脚本

开始通过 PowerShell 使用 Azure 数据资源管理器 .NET 客户端库。

1. 下载 [`Microsoft.Azure.Kusto.Tools` NuGet 包](https://www.nuget.org/packages/Microsoft.Azure.Kusto.Tools/)。
1. 提取包中“tools”目录的内容（使用 `7-zip` 之类的存档工具）。
1. 从 PowerShell 调用 `[System.Reflection.Assembly]::LoadFrom("path")`，以加载所需的库。 
    - 命令的 `path` 参数应指示所提取文件的位置。
1. 加载所有相关的 .NET 程序集后，请执行以下操作：
   1. 创建 Kusto 连接字符串。
   1. 实例化查询提供程序或管理提供程序。
   1. 运行查询或命令，如以下[示例](powershell.md#examples)所示。

有关详细信息，请参阅 [Azure 数据资源管理器客户端库](../netfx/about-kusto-data.md)。

## <a name="examples"></a>示例

### <a name="initialization"></a>初始化

```powershell
#  Part 1 of 3
#  ------------
#  Packages location - This is an example of the location from where you extract the Microsoft.Azure.Kusto.Tools package.
#  Please make sure you load the types from a local directory and not from a remote share.
$packagesRoot = "C:\Microsoft.Azure.Kusto.Tools\Tools"

#  Part 2 of 3
#  ------------
#  Loading the Kusto.Client library and its dependencies
dir $packagesRoot\* | Unblock-File
[System.Reflection.Assembly]::LoadFrom("$packagesRoot\Kusto.Data.dll")

#  Part 3 of 3
#  ------------
#  Defining the connection to your cluster / database
$clusterUrl = "https://help.kusto.chinacloudapi.cn;Fed=True"
$databaseName = "Samples"

#   Option A: using Azure AD User Authentication
$kcsb = New-Object Kusto.Data.KustoConnectionStringBuilder ($clusterUrl, $databaseName)
 
#   Option B: using Azure AD application Authentication
#     $applicationId = "application ID goes here"
#     $applicationKey = "application key goes here"
#     $authority = "authority goes here"
#     $kcsb = $kcsb.WithAadApplicationKeyAuthentication($applicationId, $applicationKey, $authority)
```

### <a name="example-running-an-admin-command"></a>示例：运行管理命令

```powershell
$adminProvider = [Kusto.Data.Net.Client.KustoClientFactory]::CreateCslAdminProvider($kcsb)
$command = [Kusto.Data.Common.CslCommandGenerator]::GenerateDiagnosticsShowCommand()
Write-Host "Executing command: '$command' with connection string: '$($kcsb.ToString())'"
$reader = $adminProvider.ExecuteControlCommand($command)
$reader.Read() # this reads a single row/record. If you have multiple ones returned, you can read in a loop 
$isHealthy = $Reader.GetBoolean(0)
Write-Host "IsHealthy = $isHealthy"
```

输出为：
```
IsHealthy = True
```

### <a name="example-running-a-query"></a>示例：运行查询

```powershell
$queryProvider = [Kusto.Data.Net.Client.KustoClientFactory]::CreateCslQueryProvider($kcsb)
$query = "StormEvents | limit 5"
Write-Host "Executing query: '$query' with connection string: '$($kcsb.ToString())'"
#   Optional: set a client request ID and set a client request property (e.g. Server Timeout)
$crp = New-Object Kusto.Data.Common.ClientRequestProperties
$crp.ClientRequestId = "MyPowershellScript.ExecuteQuery." + [Guid]::NewGuid().ToString()
$crp.SetOption([Kusto.Data.Common.ClientRequestProperties]::OptionServerTimeout, [TimeSpan]::FromSeconds(30))

#   Execute the query
$reader = $queryProvider.ExecuteQuery($query, $crp)

# Do something with the result datatable, for example: print it formatted as a table, sorted by the 
# "StartTime" column, in descending order
$dataTable = [Kusto.Cloud.Platform.Data.ExtendedDataReader]::ToDataSet($reader).Tables[0]
$dataView = New-Object System.Data.DataView($dataTable)
$dataView | Sort StartTime -Descending | Format-Table -AutoSize
```

输出为：

|StartTime           |EndTime             |EpisodeID |EventID |状态          |EventType         |InjuriesDirect |InjuriesIndirect |DeathsDirect |DeathsIndirect
|---------           |-------             |--------- |------- |-----          |---------         |-------------- |---------------- |------------ |--------------
|2007-12-30 16:00:00 |2007-12-30 16:05:00 |    11749 |  64588 |佐治亚州        |雷雨大风 |             0 |               0 |           0 |             0
|2007-12-20 07:50:00 |2007-12-20 07:53:00 |    12554 |  68796 |密西西比州    |雷雨大风 |             0 |               0 |           0 |             0
|2007-09-29 08:11:00 |2007-09-29 08:11:00 |    11091 |  61032 |大西洋南部 |水龙卷风       |             0 |               0 |           0 |             0
|2007-09-20 21:57:00 |2007-09-20 22:05:00 |    11078 |  60913 |佛罗里达州        |龙卷风           |             0 |               0 |           0 |             0
|2007-09-18 20:00:00 |2007-09-19 18:00:00 |    11074 |  60904 |佛罗里达州        |暴雨        |             0 |               0 |           0 |             0
