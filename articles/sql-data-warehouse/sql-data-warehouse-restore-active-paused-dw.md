---
title: 还原现有数据仓库
description: 用于还原现有的 Azure SQL 数据仓库的操作指南。
services: sql-data-warehouse
author: WenJason
manager: digimobile
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.subservice: manage
origin.date: 08/29/2018
ms.date: 03/02/2020
ms.author: v-jay
ms.reviewer: igorstan
ms.custom: seo-lt-2019
ms.openlocfilehash: 4c84274e1f825051f457fbbaefc383b47b9291b6
ms.sourcegitcommit: 3c98f52b6ccca469e598d327cd537caab2fde83f
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/13/2020
ms.locfileid: "79293011"
---
# <a name="restore-an-existing-azure-sql-data-warehouse"></a>还原现有的 Azure SQL 数据仓库

本文介绍如何通过 Azure 门户和 PowerShell 还原现有的 SQL 数据仓库：

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

**验证 DTU 容量。** 每个 SQL 数据仓库都由一个具有默认 DTU 配额的 SQL 服务器（例如 myserver.database.chinacloudapi.cn）托管。 验证 SQL Server 的剩余 DTU 配额是否足够进行数据库还原。

## <a name="before-you-begin"></a>准备阶段

1. 确保[安装 Azure PowerShell](https://docs.microsoft.com/powershell/azure/overview)。
2. 有一个现有的还原点，需要从该点进行还原。 若要创建新的还原，请参阅[教程：新建用户定义的还原点](sql-data-warehouse-restore-points.md)。

## <a name="restore-an-existing-data-warehouse-through-powershell"></a>通过 PowerShell 还原现有数据仓库

若要从还原点还原现有的数据仓库，请使用 [Restore-AzSqlDatabase](https://docs.microsoft.com/powershell/module/az.sql/restore-azsqldatabase) PowerShell cmdlet。

1. 打开 PowerShell。

2. 连接到 Azure 帐户，并列出与帐户关联的所有订阅。

3. 选择包含要还原的数据库的订阅。

4. 列出数据仓库的还原点。

5. 使用 RestorePointCreationDate 选取所需的还原点。

6. 使用 [Restore-AzSqlDatabase](https://docs.microsoft.com/powershell/module/az.sql/restore-azsqldatabase) PowerShell cmdlet 将数据仓库还原到所需还原点。
        1. 若要将 SQL 数据仓库还原到另一逻辑服务器，请确保指定另一逻辑服务器名称。  该逻辑服务器也可以位于另一资源组和区域中。

7. 验证已还原的数据仓库是否处于联机状态。

8. 完成还原后，可以按[在恢复后配置数据库](../sql-database/sql-database-disaster-recovery.md#configure-your-database-after-recovery)中的说明配置恢复后的数据仓库。

```Powershell

$SubscriptionName="<YourSubscriptionName>"
$ResourceGroupName="<YourResourceGroupName>"
$ServerName="<YourServerNameWithoutURLSuffixSeeNote>"  # Without database.chinacloudapi.cn
#$TargetResourceGroupName="<YourTargetResourceGroupName>" # uncomment to restore to a different logical server.
#$TargetServerName="<YourtargetServerNameWithoutURLSuffixSeeNote>"  
$DatabaseName="<YourDatabaseName>"
$NewDatabaseName="<YourDatabaseName>"

Connect-AzAccount -Environment AzureChinaCloud
Get-AzSubscription
Select-AzSubscription -SubscriptionName $SubscriptionName

# Or list all restore points
Get-AzSqlDatabaseRestorePoints -ResourceGroupName $ResourceGroupName -ServerName $ServerName -DatabaseName $DatabaseName

# Get the specific database to restore
$Database = Get-AzSqlDatabase -ResourceGroupName $ResourceGroupName -ServerName $ServerName -DatabaseName $DatabaseName

# Pick desired restore point using RestorePointCreationDate "xx/xx/xxxx xx:xx:xx xx"
$PointInTime="<RestorePointCreationDate>"  

# Restore database from a restore point
$RestoredDatabase = Restore-AzSqlDatabase –FromPointInTimeBackup –PointInTime $PointInTime -ResourceGroupName $Database.ResourceGroupName -ServerName $Database.ServerName -TargetDatabaseName $NewDatabaseName –ResourceId $Database.ResourceID

# Use the following command to restore to a different logical server
#$RestoredDatabase = Restore-AzSqlDatabase –FromPointInTimeBackup –PointInTime $PointInTime -ResourceGroupName $Database.ResourceTargetGroupName -ServerName $TargetServerName -TargetDatabaseName $NewDatabaseName –ResourceId $Database.ResourceID

# Verify the status of restored database
$RestoredDatabase.status

```

## <a name="restore-an-existing-data-warehouse-through-the-azure-portal"></a>通过 Azure 门户还原现有数据仓库

1. 登录到 [Azure 门户](https://portal.azure.cn/)。
2. 导航到要从中进行还原的 SQL 数据仓库。
3. 在“概览”边栏选项卡顶部，选择“还原”  。

    ![ 还原概述](./media/sql-data-warehouse-restore-active-paused-dw/restoring-01.png)

4. 选择“自动还原点”或“用户定义的还原点”。   如果数据仓库没有任何自动还原点，请等待数小时或创建一个用户定义的还原点，然后再进行还原。 对于用户定义的还原点，请选择一个现有的，或者创建一个新的。 对于**服务器**，可以选取另一资源组和区域中的逻辑服务器，也可以创建一个新的。 在提供所有参数后，请单击“查看 + 还原”。 

    ![自动还原点](./media/sql-data-warehouse-restore-active-paused-dw/restoring-11.png)

## <a name="next-steps"></a>后续步骤
- [还原已删除的数据仓库](sql-data-warehouse-restore-deleted-dw.md)
- [从异地备份数据仓库还原](sql-data-warehouse-restore-from-geo-backup.md)

 
