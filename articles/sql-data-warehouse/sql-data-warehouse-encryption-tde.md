---
title: SQL 数据仓库（门户）中的透明数据加密 | Microsoft 文档
description: SQL 数据仓库中的透明数据加密 (TDE)
services: sql-data-warehouse
author: WenJason
manager: digimobile
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.subservice: implement
origin.date: 04/17/2018
ms.date: 03/25/2019
ms.author: v-jay
ms.reviewer: igorstan
ms.openlocfilehash: e67fbb2e86639a651e9195adbe629a74d2058758
ms.sourcegitcommit: c1ba5a62f30ac0a3acb337fb77431de6493e6096
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2020
ms.locfileid: "75624342"
---
# <a name="get-started-with-transparent-data-encryption-tde-in-sql-data-warehouse"></a>SQL 数据仓库中的透明数据加密 (TDE) 入门
> [!div class="op_single_selector"]
> * [安全概述](sql-data-warehouse-overview-manage-security.md)
> * [身份验证](sql-data-warehouse-authentication.md)
> * [加密（门户）](sql-data-warehouse-encryption-tde.md)
> * [加密 (T-SQL)](sql-data-warehouse-encryption-tde-tsql.md)
> 
> 

## <a name="required-permissions"></a>所需的权限
若要启用透明数据加密 (TDE)，用户必须是管理员或 dbmanager 角色的成员。

## <a name="enabling-encryption"></a>启用加密
若要为 SQL 数据仓库启用 TDE，请遵循以下步骤：

1. 在 [Azure 门户](https://portal.azure.cn)中打开数据库
3. 选择“透明数据加密”  选项![][1]
4. 选择“打开”  设置![][2]
5. 选择“保存”  
   ![][3]  

## <a name="disabling-encryption"></a>禁用加密
若要为 SQL 数据仓库禁用 TDE，请遵循以下步骤：

1. 在 [Azure 门户](https://portal.azure.cn)中打开数据库
3. 选择“透明数据加密”  选项![][1]
4. 选择“关闭”设置  ![][4]
5. 选择“保存”  
   ![][5]  

## <a name="encryption-dmvs"></a>加密 DMV
可使用下列 DMV 来确认加密：

* [sys.databases]
* [sys.dm_pdw_nodes_database_encryption_keys]

<!--MSDN references-->
[Transparent Data Encryption (TDE)]: https://msdn.microsoft.com/library/bb934049.aspx
[sys.databases]: https://msdn.microsoft.com/library/ms178534.aspx
[sys.dm_pdw_nodes_database_encryption_keys]: https://msdn.microsoft.com/library/mt203922.aspx

<!--Image references-->
[1]: ./media/sql-data-warehouse-security-tde/sql-data-warehouse-security-tde-portal-settings.png
[2]: ./media/sql-data-warehouse-security-tde/sql-data-warehouse-security-tde-portal-settings-on.png
[3]: ./media/sql-data-warehouse-security-tde/sql-data-warehouse-security-tde-portal-settings-save.png
[4]: ./media/sql-data-warehouse-security-tde/sql-data-warehouse-security-tde-portal-settings-off.png
[5]: ./media/sql-data-warehouse-security-tde/sql-data-warehouse-security-tde-portal-settings-save2.png

<!--Link references-->