---
title: 重启服务器 - Azure PowerShell - Azure Database for MySQL
description: 本文介绍了如何使用 PowerShell 重启 Azure Database for MySQL 服务器。
author: WenJason
ms.author: v-jay
ms.service: mysql
ms.topic: conceptual
origin.date: 4/28/2020
ms.date: 06/01/2020
ms.openlocfilehash: 128e3f979996075216d1292735812170a5cece71
ms.sourcegitcommit: be0a8e909fbce6b1b09699a721268f2fc7eb89de
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/29/2020
ms.locfileid: "84200000"
---
# <a name="restart-azure-database-for-mysql-server-using-powershell"></a>使用 PowerShell 重启 Azure Database for MySQL 服务器

本主题介绍如何重启 Azure Database for MySQL 服务器。 出于维护原因，可能需要重启服务器，这会在操作期间导致短暂中断。

如果服务处于繁忙状态，则会阻止重启服务器。 例如，服务可能正在处理先前请求的操作（例如缩放 vCore）。

完成重启所需的时间量取决于 MySQL 恢复过程。 若要减少重启时间，建议在重启之前尽量减少服务器上发生的活动量。

## <a name="prerequisites"></a>先决条件

若要完成本操作指南，需要：

- 在本地安装了 [Az PowerShell 模块](https://docs.microsoft.com/powershell/azure/install-az-ps)。
- [Azure Database for MySQL 服务器](quickstart-create-mysql-server-database-using-azure-powershell.md)

> [!IMPORTANT]
> 尽管 Az.MySql PowerShell 模块为预览版，但必须使用以下命令从 Az PowerShell 模块单独安装它：`Install-Module -Name Az.MySql -AllowPrerelease`。
> Az.MySql PowerShell 模块正式版推出后，它会包含在将来的 Az PowerShell 模块发行版中。

使用 [Connect-AzAccount](https://docs.microsoft.com/powershell/module/az.accounts/Connect-AzAccount) cmdlet 连接到 Azure 帐户。

## <a name="restart-the-server"></a>重启服务器

使用以下命令重启服务器：

```azurepowershell
Restart-AzMySqlServer -Name mydemoserver -ResourceGroupName myresourcegroup
```

## <a name="next-steps"></a>后续步骤

> [!div class="nextstepaction"]
> [使用 PowerShell 创建 Azure Database for MySQL 服务器](quickstart-create-mysql-server-database-using-azure-powershell.md)
