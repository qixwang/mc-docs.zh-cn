---
title: 安装 Visual Studio 2019
description: 安装适用于 Azure SQL 数据仓库的 Visual Studio 和 SQL Server 开发工具 (SSDT)
services: sql-data-warehouse
ms.custom: seo-lt-2019
ms.workload: azure-vs
author: WenJason
manager: digimobile
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.subservice: development
origin.date: 11/06/2019
ms.date: 01/20/2020
ms.author: v-jay
ms.reviewer: igorstan
ms.openlocfilehash: ab4f2289acf4ab9cc9ae7a6fc0cb454ea4fa6f34
ms.sourcegitcommit: c1ba5a62f30ac0a3acb337fb77431de6493e6096
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2020
ms.locfileid: "75964895"
---
# <a name="getting-started-with-visual-studio-2019-for-sql-data-warehouse"></a>适用于 SQL 数据仓库的 Visual Studio 2019 入门
Visual Studio **2019** SQL Server Data Tools (SSDT) 是一个工具，可用于执行以下操作：

- 连接、查询和开发 SQL 数据仓库的应用程序 
- 利用对象资源管理器直观地浏览数据模型中的所有对象，包括表、视图、存储过程等。
- 为对象生成 T-SQL 数据定义语言 (DDL) 脚本
- 对 SSDT 数据库项目使用基于状态的方法来开发数据仓库
- 将数据库项目与源代码管理系统（例如 Git 和 Azure Repos）集成
- 通过自动化服务器（如 Azure DevOps）设置持续集成和部署管道

## <a name="install-visual-studio-2019"></a>安装 Visual Studio 2019
请参阅[下载 Visual Studio 2019][] 以下载并安装 Visual Studio **16.3 及更高版本**。 在安装过程中，选择“数据存储和处理”工作负载。 Visual Studio 2019 不再需要单独安装 SSDT。

## <a name="unsupported-features-in-ssdt"></a>SSDT 中不支持的功能

有时，SQL 数据仓库的功能版可能不包括对 SSDT 的支持。 目前不支持以下功能：

- [具体化视图](https://docs.microsoft.com/sql/t-sql/statements/create-materialized-view-as-select-transact-sql?view=azure-sqldw-latest)（正在开发）
- [有序聚集列存储索引](https://docs.microsoft.com/sql/t-sql/statements/create-columnstore-index-transact-sql?view=azure-sqldw-latest#examples--and-)（正在开发）
- [COPY 语句](https://docs.microsoft.com/sql/t-sql/statements/copy-into-transact-sql?view=azure-sqldw-latest)（正在开发）
- [工作负荷管理](/sql-data-warehouse/sql-data-warehouse-workload-management) - 工作负荷组和分类器（正在开发）
- [行级安全](https://docs.microsoft.com/sql/relational-databases/security/row-level-security?view=sql-server-ver15)
- [动态数据屏蔽](https://docs.microsoft.com/sql/relational-databases/security/dynamic-data-masking?toc=%2Fsql-data-warehouse%2Ftoc.json&view=sql-server-2017#defining-a-dynamic-data-mask)
- [PREDICT](https://docs.microsoft.com/sql/t-sql/queries/predict-transact-sql?view=sql-server-ver15&viewFallbackFrom=azure-sqldw-latest) 函数 
- [带约束的表](/sql-data-warehouse/sql-data-warehouse-table-constraints#table-constraints)不受支持。 对于这些表对象，请将生成操作设为“无”。

## <a name="next-steps"></a>后续步骤

安装最新版本的 SSDT 后，便可以[连接][connect]到 SQL 数据仓库。

<!--Anchors-->

<!--Image references-->

<!--Articles-->
[connect]: ./sql-data-warehouse-query-visual-studio.md

<!--Other-->
[下载 Visual Studio 2019]: https://visualstudio.microsoft.com/downloads/
[Installing Visual Studio]: https://msdn.microsoft.com/library/e2h7fzkw.aspx
[SSDT Download]: https://msdn.microsoft.com/library/mt204009.aspx
