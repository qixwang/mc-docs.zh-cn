---
title: 使用 Azure 门户重启 Azure Database for MariaDB 服务器
description: 本文介绍了如何使用 Azure 门户重启 Azure Database for MariaDB 服务器。
author: WenJason
ms.author: v-jay
ms.service: mariadb
ms.topic: conceptual
origin.date: 2/7/2019
ms.date: 05/27/2019
ms.openlocfilehash: 2c1f603e2151a8e78e679370a8dc2f3805437c9c
ms.sourcegitcommit: c1ba5a62f30ac0a3acb337fb77431de6493e6096
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2020
ms.locfileid: "66173338"
---
# <a name="restart-azure-database-for-mariadb-server-using-azure-portal"></a>使用 Azure 门户重启 Azure Database for MariaDB 服务器
本主题介绍了如何重启 Azure Database for MariaDB 服务器。 出于维护原因，可能需要重启服务器，这会在服务器执行操作时导致短暂中断。

如果服务处于繁忙状态，则会阻止重启服务器。 例如，服务可能正在处理先前请求的操作（例如缩放 vCore）。

完成重启所需的时间取决于 MariaDB 恢复过程。 若要减少重启时间，建议在重启之前尽量减少服务器上发生的活动量。

## <a name="prerequisites"></a>先决条件
若要完成本操作指南，需要：
- [Azure Database for MariaDB 服务器](./quickstart-create-mariadb-server-database-using-azure-portal.md)

## <a name="perform-server-restart"></a>执行服务器重启

可通过以下步骤重启 MariaDB 服务器：

1. 在 Azure 门户中，选择 Azure Database for MariaDB 服务器。

2. 在服务器“概述”页的工具栏中，单击“重启”   。

   ![Azure Database for MariaDB - 概述 -“重启”按钮](./media/howto-restart-server-portal/2-server.png)

3. 单击“是”以确认重启服务器  。

   ![Azure Database for MariaDB - 重启确认](./media/howto-restart-server-portal/3-restart-confirm.png)

4. 观察到服务器状态更改为“正在重启”。

   ![Azure Database for MariaDB - 重启状态](./media/howto-restart-server-portal/4-restarting-status.png)

5. 确认服务器重启成功。

   ![Azure Database for MariaDB - 重启成功](./media/howto-restart-server-portal/5-restart-success.png)

## <a name="next-steps"></a>后续步骤

[快速入门：使用 Azure 门户创建 Azure Database for MariaDB 服务器](./quickstart-create-mariadb-server-database-using-azure-portal.md)