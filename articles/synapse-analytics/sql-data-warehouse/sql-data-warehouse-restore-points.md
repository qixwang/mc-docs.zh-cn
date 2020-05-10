---
title: 用户定义的还原点
description: 如何创建 SQL 池的还原点。
services: synapse-analytics
author: WenJason
manager: digimobile
ms.service: synapse-analytics
ms.topic: conceptual
ms.subservice: ''
origin.date: 07/03/2019
ms.date: 05/11/2020
ms.author: v-jay
ms.reviewer: igorstan
ms.custom: seo-lt-2019
ms.openlocfilehash: 0f4f36aa27c6d5e53836b9749afd881f1c162ed8
ms.sourcegitcommit: f8d6fa25642171d406a1a6ad6e72159810187933
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/28/2020
ms.locfileid: "82198754"
---
# <a name="user-defined-restore-points"></a>用户定义的还原点

本文介绍了如何使用 PowerShell 和 Azure 门户为 Azure Synapse Analytics 中的 SQL 池新建用户定义的还原点。

## <a name="create-user-defined-restore-points-through-powershell"></a>通过 PowerShell 创建用户定义的还原点

若要创建用户定义的还原点，请使用 [New-AzSqlDatabaseRestorePoint](https://docs.microsoft.com/powershell/module/az.sql/new-azsqldatabaserestorepoint?toc=/synapse-analytics/sql-data-warehouse/toc.json&bc=/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json) PowerShell cmdlet。

1. 开始之前，请确保[安装 Azure PowerShell](https://docs.microsoft.com/powershell/azure/overview?toc=/synapse-analytics/sql-data-warehouse/toc.json&bc=/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json)。
2. 打开 PowerShell。
3. 连接到 Azure 帐户，并列出与帐户关联的所有订阅。
4. 选择包含要还原的数据库的订阅。
5. 为数据仓库的即时复制创建还原点。

```Powershell

$SubscriptionName="<YourSubscriptionName>"
$ResourceGroupName="<YourResourceGroupName>"
$ServerName="<YourServerNameWithoutURLSuffixSeeNote>"  # Without database.chinacloudapi.cn
$DatabaseName="<YourDatabaseName>"
$Label = "<YourRestorePointLabel>"

Connect-AzAccount -Environment AzureChinaCloud
Get-AzSubscription
Select-AzSubscription -SubscriptionName $SubscriptionName

# Create a restore point of the original database
New-AzSqlDatabaseRestorePoint -ResourceGroupName $ResourceGroupName -ServerName $ServerName -DatabaseName $DatabaseName -RestorePointLabel $Label

```

6. 查看所有现有还原点的列表。

```Powershell
# List all restore points
Get-AzSqlDatabaseRestorePoints -ResourceGroupName $ResourceGroupName -ServerName $ServerName -DatabaseName $DatabaseName
```

## <a name="create-user-defined-restore-points-through-the-azure-portal"></a>通过 Azure 门户创建用户定义的还原点

还可以通过 Azure 门户创建用户定义的还原点。

1. 登录到 [Azure 门户](https://portal.azure.cn/)帐户。

2. 导航到要为其创建还原点的 SQL 池。

3. 从左窗格中选择“概述”  ，选择“+ 新建还原点”  。 如果“新建还原点”按钮未启用，请确保 SQL 池未暂停。

    ![新建还原点](./media/sql-data-warehouse-restore-points/creating-restore-point-01.png)

4. 为用户定义的还原点指定名称，然后单击“应用”  。 用户定义的还原点的默认保留期为七天。

    ![还原点的名称](./media/sql-data-warehouse-restore-points/creating-restore-point-11.png)

## <a name="next-steps"></a>后续步骤

- [还原现有的 SQL 池](sql-data-warehouse-restore-active-paused-dw.md)
- [还原已删除的 SQL 池](sql-data-warehouse-restore-deleted-dw.md)
- [从异地备份 SQL 池进行还原](sql-data-warehouse-restore-from-geo-backup.md)

