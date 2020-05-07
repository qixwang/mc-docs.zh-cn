---
title: 排查连接问题
description: 排查 Synapse SQL 池中的连接问题。
services: synapse-analytics
author: WenJason
manager: digimobile
ms.service: synapse-analytics
ms.topic: conceptual
ms.subservice: ''
origin.date: 03/27/2019
ms.date: 05/11/2020
ms.author: v-jay
ms.reviewer: igorstan
ms.custom: seo-lt-2019, azure-synapse
ms.openlocfilehash: 827e36006a3a4101b2ac4f60a2dfab6ba30a8adb
ms.sourcegitcommit: f8d6fa25642171d406a1a6ad6e72159810187933
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/28/2020
ms.locfileid: "82198740"
---
# <a name="troubleshooting-connectivity-issues"></a>排查连接问题

本文列出了与连接到 SQL Analytics 数据库相关的常见故障排除技术。

## <a name="check-service-availability"></a>检查服务可用性

查看服务是否可用。 在 Azure 门户中转到尝试连接的 Synapse SQL 池。 在左侧 TOC 面板中，单击“诊断并解决问题”。 

![选择资源运行状况](./media/sql-data-warehouse-troubleshoot-connectivity/diagnostics-link.png)

Synapse SQL 池的状态会显示在此处。 如果服务未显示为“可用”，  请查看更多的步骤。

![服务可用](./media/sql-data-warehouse-troubleshoot-connectivity/resource-health.png)

如果资源运行状况显示 Synapse SQL 池实例已暂停或正在缩放，请按指南恢复实例。

![服务暂停](./media/sql-data-warehouse-troubleshoot-connectivity/resource-health-pausing.png) 可以在此处找到有关资源运行状况的其他信息。

## <a name="check-for-paused-or-scaling-operation"></a>检查暂停的或正在缩放的操作

检查门户，了解 Synapse SQL 池实例是已暂停还是正在缩放。

![服务已暂停](./media/sql-data-warehouse-troubleshoot-connectivity/overview-paused.png)

如果看到服务已暂停或正在缩放，请查看一下这是否是你的维护计划。 在门户的 Synapse SQL 池概览中，可以看到所选的维护计划。 

![维护计划概览](./media/sql-data-warehouse-troubleshoot-connectivity/overview-maintance-schedule.png)

否则，请与 IT 管理员联系，验证该维护是否为计划的事件。 若要恢复 SQL Analytics 实例，请按照[这些步骤](pause-and-resume-compute-portal.md)操作。

## <a name="check-your-firewall-settings"></a>检查防火墙设置

SQL Analytics 数据库通过端口 1433 进行通信。 如果尝试从企业网络内部进行连接，则该网络的防火墙可能不允许经端口 1433 的出站流量。 在这种情况下，将无法连接到 Azure SQL 数据库服务器，除非 IT 部门打开了端口 1433。 可从[此处](../../sql-database/sql-database-firewall-configure.md?toc=/synapse-analytics/sql-data-warehouse/toc.json&bc=/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json#create-and-manage-ip-firewall-rules)找到有关防火墙配置的更多信息。

## <a name="check-your-vnetservice-endpoint-settings"></a>检查 VNet/服务终结点设置

如果收到 40914 和 40615 错误，请参阅[此处的错误说明和解决方法](../../sql-database/sql-database-vnet-service-endpoint-rule-overview.md?toc=/synapse-analytics/sql-data-warehouse/toc.json&bc=/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json#errors-40914-and-40615)。

## <a name="check-for-the-latest-drivers"></a>检查最新驱动程序

### <a name="software"></a>软件

请进行检查，确保使用最新工具连接到 Synapse SQL 池：

- SSMS
- Azure Data Studio
- SQL Server Data Tools (Visual Studio)

### <a name="drivers"></a>驱动程序

请进行检查，确保使用最新的驱动程序版本。  使用旧版驱动程序可能会导致意外行为，因为旧驱动程序可能不支持新功能。

- [ODBC](https://docs.microsoft.com/sql/connect/odbc/download-odbc-driver-for-sql-server?toc=/synapse-analytics/sql-data-warehouse/toc.json&bc=/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest)
- [JDBC](https://docs.microsoft.com/sql/connect/jdbc/download-microsoft-jdbc-driver-for-sql-server?toc=/synapse-analytics/sql-data-warehouse/toc.json&bc=/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest)
- [OLE DB](https://docs.microsoft.com/sql/connect/oledb/download-oledb-driver-for-sql-server?toc=/synapse-analytics/sql-data-warehouse/toc.json&bc=/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest)
- [PHP](https://docs.microsoft.com/sql/connect/php/download-drivers-php-sql-server?toc=/synapse-analytics/sql-data-warehouse/toc.json&bc=/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest)

## <a name="check-your-connection-string"></a>检查连接字符串

请进行检查，确保正确设置了连接字符串。  下面是一些示例。  可在[此处](sql-data-warehouse-connection-strings.md)找到有关连接字符串的其他信息。

ADO.NET 连接字符串

```csharp
Server=tcp:{your_server}.database.chinacloudapi.cn,1433;Database={your_database};User ID={your_user_name};Password={your_password_here};Encrypt=True;TrustServerCertificate=False;Connection Timeout=30;
```

ODBC 连接字符串

```csharp
Driver={SQL Server Native Client 11.0};Server=tcp:{your_server}.database.chinacloudapi.cn,1433;Database={your_database};Uid={your_user_name};Pwd={your_password_here};Encrypt=yes;TrustServerCertificate=no;Connection Timeout=30;
```

PHP 连接字符串

```PHP
Server: {your_server}.database.chinacloudapi.cn,1433 \r\nSQL Database: {your_database}\r\nUser Name: {your_user_name}\r\n\r\nPHP Data Objects(PDO) Sample Code:\r\n\r\ntry {\r\n   $conn = new PDO ( \"sqlsrv:server = tcp:{your_server}.database.chinacloudapi.cn,1433; Database = {your_database}\", \"{your_user_name}\", \"{your_password_here}\");\r\n    $conn->setAttribute( PDO::ATTR_ERRMODE, PDO::ERRMODE_EXCEPTION );\r\n}\r\ncatch ( PDOException $e ) {\r\n   print( \"Error connecting to SQL Server.\" );\r\n   die(print_r($e));\r\n}\r\n\rSQL Server Extension Sample Code:\r\n\r\n$connectionInfo = array(\"UID\" => \"{your_user_name}\", \"pwd\" => \"{your_password_here}\", \"Database\" => \"{your_database}\", \"LoginTimeout\" => 30, \"Encrypt\" => 1, \"TrustServerCertificate\" => 0);\r\n$serverName = \"tcp:{your_server}.database.chinacloudapi.cn,1433\";\r\n$conn = sqlsrv_connect($serverName, $connectionInfo);
```

JDBC 连接字符串

```Java
jdbc:sqlserver://yourserver.database.chinacloudapi.cn:1433;database=yourdatabase;user={your_user_name};password={your_password_here};encrypt=true;trustServerCertificate=false;hostNameInCertificate=*.database.chinacloudapi.cn;loginTimeout=30;
```

## <a name="intermittent-connection-issues"></a>间歇性连接问题

检查是否在服务器上遇到具有大量排队请求的重型负载。 可能需要纵向扩展 Synapse SQL 池，以获取更多资源。

## <a name="common-error-messages"></a>常见错误消息

对于 40914 和 40615 错误，请参阅[此处的错误说明和解决方法](../../sql-database/sql-database-vnet-service-endpoint-rule-overview.md?toc=/synapse-analytics/sql-data-warehouse/toc.json&bc=/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json#errors-40914-and-40615)。

## <a name="still-having-connectivity-issues"></a>仍有连接问题？

请联系 [Azure 支持部门](https://support.azure.cn/zh-cn/support/contact)，让工程团队为你提供支持。
