---
title: 重启服务器 - Azure PowerShell - Azure Database for MariaDB
description: 本文介绍了如何使用 PowerShell 重启 Azure Database for MariaDB 服务器。
author: WenJason
ms.author: v-jay
ms.service: mariadb
ms.topic: conceptual
origin.date: 5/26/2020
ms.date: 07/06/2020
ms.openlocfilehash: faedbfe225880d38b511869a2d337999d7b001b2
ms.sourcegitcommit: 7ea2d04481512e185a60fa3b0f7b0761e3ed7b59
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/02/2020
ms.locfileid: "85845927"
---
# <a name="restart-azure-database-for-mariadb-server-using-powershell"></a>使用 PowerShell 重启 Azure Database for MariaDB 服务器

本主题介绍了如何重启 Azure Database for MariaDB 服务器。 出于维护原因，可能需要重启服务器，这会在操作期间导致短暂中断。

如果服务处于繁忙状态，则会阻止重启服务器。 例如，服务可能正在处理先前请求的操作（例如缩放 vCore）。

完成重启所需的时间长短取决于 MariaDB 恢复过程。 若要减少重启时间，建议在重启之前尽量减少服务器上发生的活动量。

## <a name="prerequisites"></a>先决条件

若要完成本操作指南，需要：

- 在本地安装了 [Az PowerShell 模块](https://docs.microsoft.com/powershell/azure/install-az-ps) 
- [Azure Database for MariaDB 服务器](quickstart-create-mariadb-server-database-using-azure-powershell.md)

> [!IMPORTANT]
> 尽管 Az.MariaDb PowerShell 模块为预览版，但必须使用以下命令从 Az PowerShell 模块单独安装它：`Install-Module -Name Az.MariaDb -AllowPrerelease`。

使用 [Connect-AzAccount](https://docs.microsoft.com/powershell/module/az.accounts/connect-azaccount) cmdlet 连接到 Azure 帐户。

## <a name="restart-the-server"></a>重启服务器

使用以下命令重启服务器：

```azurepowershell
Restart-AzMariaDbServer -Name mydemoserver -ResourceGroupName myresourcegroup
```

## <a name="next-steps"></a>后续步骤

> [!div class="nextstepaction"]
> [使用 PowerShell 创建 Azure Database for MariaDB 服务器](quickstart-create-mariadb-server-database-using-azure-powershell.md)
