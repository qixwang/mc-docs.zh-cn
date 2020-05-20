---
title: 从 Azure Stack Hub 删除 Azure 应用服务
description: 了解如何从 Azure Stack Hub 删除 Azure 应用服务
author: WenJason
ms.topic: article
origin.date: 04/17/2020
ms.date: 05/18/2020
ms.author: v-jay
ms.reviewer: anwestg
ms.lastreviewed: 04/17/20207
ms.openlocfilehash: c3fc4c543c68bb9ecb63902e8db57792d8ce73ec
ms.sourcegitcommit: 134afb420381acd8d6ae56b0eea367e376bae3ef
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/15/2020
ms.locfileid: "83423058"
---
# <a name="remove-azure-app-service-from-azure-stack-hub"></a>从 Azure Stack Hub 删除 Azure 应用服务

本文介绍如何从 Azure Stack Hub 删除 Azure 应用服务资源提供程序和相关组件。

## <a name="remove-resource-provider"></a>删除资源提供程序

> [!Important]
> 此操作会删除所有租户资源、从所有计划中删除服务和配额，以及彻底删除 Azure 应用程序服务资源提供程序。  如果已部署应用服务高可用文件服务器和 SQL Server 快速启动模板，则这些资源也会被删除，因为它们与 Azure Stack Hub 上的 Azure 应用服务部署在同一资源组中。

若要从 Azure Stack Hub 删除 Azure 应用服务，请执行下面的这个步骤：

1. 删除用于保存 Azure Stack Hub 上的 Azure应用服务资源的资源组（例如 AppService.local）

## <a name="remove-databases-and-file-share-content"></a>删除数据库和文件共享内容

仅当 SQL Server 和/或文件服务器部署在缩放单元之外或其他资源组中时，才需要执行此部分步骤，否则请继续执行下一部分。

### <a name="remove-databases-and-logins"></a>删除数据库和登录名

1. 如果使用 SQL Server Always On，请从可用性组中删除 AppService_Hosting 和 AppService_Metering 数据库：

1. 执行以下 SQL 脚本以删除数据库和登录名

   ```sql
   --******************************************************************
   /*
   Script to clean up App Service objects (databases and logins).
   */
   USE [master]
   GO

   DROP DATABASE [appservice_hosting]
   GO

   DROP DATABASE [appservice_metering]
   GO

   DECLARE @sql NVARCHAR(MAX) = N'';    
 
   SELECT @sql += '
   DROP LOGIN [' + name + '];' 
   from master.sys.sql_logins
   WHERE name LIKE  '%_hosting_%' OR 
   name LIKE  '%_metering_%' OR
   name LIKE  '%WebWorker_%';

   PRINT @sql;
   EXEC sp_executesql @sql;
   PRINT 'Completed';

   --******************************************************************
   ```

### <a name="remove-the-application-file-content-from-the-file-server"></a>从文件服务器中删除应用程序文件内容

1. 从文件服务器中删除文件共享内容。

## <a name="next-steps"></a>后续步骤

若要重新进行安装，请返回到[在 Azure Stack Hub 上部署应用服务的先决条件](azure-stack-app-service-before-you-get-started.md)一文。
