---
title: 快速入门：在 Azure SQL 数据仓库中横向扩展计算资源 - PowerShell | Microsoft Docs
description: 使用 PowerShell 在 Azure SQL 数据仓库中缩放计算资源 横向扩展计算为提高性能或缩放重新计算以节约成本。
services: sql-data-warehouse
author: WenJason
manager: digimobile
ms.service: sql-data-warehouse
ms.topic: quickstart
ms.subservice: implement
origin.date: 04/17/2018
ms.date: 10/21/2019
ms.author: v-jay
ms.reviewer: igorstan
ms.openlocfilehash: 1519ceee16d8d0d623e0b66d43b3bbf3acec28a0
ms.sourcegitcommit: 3c98f52b6ccca469e598d327cd537caab2fde83f
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/13/2020
ms.locfileid: "79293316"
---
# <a name="quickstart-scale-compute-in-azure-sql-data-warehouse-in-azure-powershell"></a>快速入门：使用 Azure PowerShell 在 Azure SQL 数据仓库中缩放计算资源

使用 Azure PowerShell 在 Azure SQL 数据仓库中缩放计算资源。 [横向扩展计算](sql-data-warehouse-manage-compute-overview.md)以提高性能或按比例缩减计算以节约成本。

如果没有 Azure 订阅，可在开始前创建一个 [1 元人民币试用](https://www.azure.cn/pricing/1rmb-trial/)帐户。

## <a name="before-you-begin"></a>准备阶段

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

本快速入门假定你已有可缩放的 SQL 数据仓库。 如果需要创建一个 SQL 数据仓库，可使用[创建并连接 - 门户](create-data-warehouse-portal.md)创建名为“mySampleDataWarehouse”的数据仓库  。

## <a name="log-in-to-azure"></a>登录 Azure

使用 [Connect-AzAccount -EnvironmentName AzureChinaCloud](https://docs.microsoft.com/powershell/module/az.accounts/connect-azaccount) 命令登录到 Azure 订阅，并按屏幕说明操作。

```powershell
Connect-AzAccount -EnvironmentName AzureChinaCloud
```

若要查看正在使用的订阅，请运行 [Get-AzSubscription](https://docs.microsoft.com/powershell/module/az.accounts/get-azsubscription)。

```powershell
Get-AzSubscription
```

如果需要使用与默认订阅不同的订阅，请运行 [Set-AzContext](https://docs.microsoft.com/powershell/module/az.accounts/set-azcontext)。

```powershell
Set-AzContext -SubscriptionName "MySubscription"
```

## <a name="look-up-data-warehouse-information"></a>查找数据仓库信息

查找计划暂停和恢复的数据仓库的数据库名称、服务器名称和资源组。

按照以下步骤查找数据仓库的位置信息。

1. 登录到 [Azure 门户](https://portal.azure.cn/)。
2. 在 Azure 门户的左侧页面中，单击“SQL 数据仓库”  。
3. 从“SQL 数据仓库”页中选择“mySampleDataWarehouse”   。 此操作打开数据仓库。

    ![服务器名称和资源组](media/pause-and-resume-compute-powershell/locate-data-warehouse-information.png)

4. 记下将用作数据库名称的数据仓库名称。 请记住，数据仓库是一种数据库。 同时记下服务器名称和资源组。 执行暂停和恢复命令时会用到。
5. 如果服务器是 foo.database.chinacloudapi.cn，请在 PowerShell cmdlet 中仅使用第一部分作为服务器名称。 在上图中，完整的服务器名称为 newserver-20181129.database.chinacloudapi.cn。 我们将使用 newserver-20180430 作为 PowerShell cmdlet 中的服务器名称  。

## <a name="scale-compute"></a>缩放计算

在 SQL 数据仓库中，可以通过调整数据仓库单位来增加或减少计算资源。 [创建和 Connect - 门户](create-data-warehouse-portal.md)创建 **mySampleDataWarehouse** 并初始化 400 DWU。 以下步骤调整为 DWU **mySampleDataWarehouse**。

若要更改数据仓库单位，请使用 [Set-AzSqlDatabase](https://docs.microsoft.com/powershell/module/az.sql/set-azsqldatabase) PowerShell cmdlet。 以下示例将数据库 **mySampleDataWarehouse**（在服务器 **mynewserver-20181129** 上资源组 **myResourceGroup** 中托管）的数据仓库单位设置为 DW300c。

```Powershell
Set-AzSqlDatabase -ResourceGroupName "myResourceGroup" -DatabaseName "mySampleDataWarehouse" -ServerName "mynewserver-20181129" -RequestedServiceObjectiveName "DW300c"
```

## <a name="check-data-warehouse-state"></a>检查数据仓库状态

若要查看数据仓库的当前状态，使用 [Get-AzSqlDatabase](https://docs.microsoft.com/powershell/module/az.sql/get-azsqldatabase) PowerShell cmdlet。 此 cmdlet 获取资源组 **myResourceGroup** 和服务器 **mynewserver-20181129.database.chinacloudapi.cn** 中 **mySampleDataWarehouse** 数据库的状态。

```powershell
$database = Get-AzSqlDatabase -ResourceGroupName myResourceGroup -ServerName mynewserver-20181129 -DatabaseName mySampleDataWarehouse
$database
```

这会导致以下类似的结果

```powershell
ResourceGroupName             : myResourceGroup
ServerName                    : mynewserver-20181129
DatabaseName                  : mySampleDataWarehouse
Location                      : China East 2
DatabaseId                    : 34d2ffb8-b70a-40b2-b4f9-b0a39833c974
Edition                       : DataWarehouse
CollationName                 : SQL_Latin1_General_CP1_CI_AS
CatalogCollation              :
MaxSizeBytes                  : 263882790666240
Status                        : Online
CreationDate                  : 11/20/2017 9:18:12 PM
CurrentServiceObjectiveId     : 284f1aff-fee7-4d3b-a211-5b8ebdd28fea
CurrentServiceObjectiveName   : DW300c
RequestedServiceObjectiveId   : 284f1aff-fee7-4d3b-a211-5b8ebdd28fea
RequestedServiceObjectiveName :
ElasticPoolName               :
EarliestRestoreDate           :
Tags                          :
ResourceId                    : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/
                                resourceGroups/myResourceGroup/providers/Microsoft.Sql/servers/mynewserver-20181129/databases/mySampleDataWarehouse
CreateMode                    :
ReadScale                     : Disabled
ZoneRedundant                 : False
```

可以在输出中查看数据库的 **Status**（状态）。 在本例中，可以看到此数据库处于联机状态。  运行此命令后，应收到“联机”、“正在暂停”、“正在恢复”、“正在缩放”和“已暂停”等状态值。

若要查看数据库本身的状态，请使用以下命令：

```powershell
$database | Select-Object DatabaseName,Status
```

## <a name="next-steps"></a>后续步骤
现在已暂停并恢复了数据仓库的计算。 若要了解有关 Azure SQL 数据仓库的详细信息，请继续有关加载数据的教程。

> [!div class="nextstepaction"]
>[将数据加载到 SQL 数据仓库](load-data-from-azure-blob-storage-using-polybase.md)
