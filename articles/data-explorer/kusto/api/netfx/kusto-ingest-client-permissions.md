---
title: Kusto.Ingest 权限 - Azure 数据资源管理器
description: 本文介绍 Kusto.Ingest（Azure 数据资源管理器中的引入权限）。
services: data-explorer
author: orspod
ms.author: v-tawe
ms.reviewer: rkarlin
ms.service: data-explorer
ms.topic: reference
origin.date: 02/13/2020
ms.date: 07/01/2020
ms.openlocfilehash: a84739b90af058141bb31082c4941ec4c14ba66b
ms.sourcegitcommit: 9bc3e55f01e0999f05e7b4ebaea95f3ac91d32eb
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/10/2020
ms.locfileid: "86226253"
---
# <a name="kustoingest---ingestion-permissions"></a>Kusto.Ingest - 引入权限 

本文介绍了为使 `Native` 引入正常工作而需要在服务上设置的权限。

## <a name="prerequisites"></a>先决条件
 
* 若要查看和修改 Kusto 服务和数据库的授权设置，请参阅 [Kusto 控制命令](../../management/security-roles.md)。

* 在以下示例中用作示例主体的 Azure Active Directory (Azure AD) 应用程序：
    * 测试 Azure AD 应用 (2a904276-1234-5678-9012-66fc53add60b; microsoft.com)
    * Kusto 内部引入 Azure AD 应用 (76263cdb-1234-5678-9012-545644e9c404; microsoft.com)
 
## <a name="ingestion-permission-mode-for-queued-ingestion"></a>排队引入的引入权限模式

引入权限模式在 [IKustoQueuedIngestClient](kusto-ingest-client-reference.md#interface-ikustoqueuedingestclient) 中定义。 此模式使客户端代码仅依赖 Azure 数据资源管理器服务。 引入是通过将 Kusto 引入消息发布到 Azure 队列来完成的。 队列（也称为引入服务）从 Azure 数据资源管理器服务获得。 中间存储项目将由引入客户端使用 Azure 数据资源管理器服务分配的资源创建。

此关系图概述了排队引入客户端与 Kusto 的交互。

:::image type="content" source="../images/kusto-ingest-client-permissions/queued-ingest.png" alt-text="排队引入":::

### <a name="permissions-on-the-engine-service"></a>引擎服务的权限

若要符合将数据引入数据库 `DB1` 的表 `T1` 中的条件，执行引入操作的主体必须获得授权。
所需的最小权限级别是 `Database Ingestor` 和 `Table Ingestor`，它们可以将数据引入到某个数据库的所有现有表中，或者引入到特定的现有表中。
如果需要创建表，则还必须分配 `Database User` 或更高的访问权限角色。


|角色                 |PrincipalType        |PrincipalDisplayName
|---------------------|---------------------|------------
|`Database Ingestor`  |Azure AD 应用程序 |`Test App (app id: 2a904276-1234-5678-9012-66fc53add60b)`
|`Table Ingestor`     |Azure AD 应用程序 |`Test App (app id: 2a904276-1234-5678-9012-66fc53add60b)`

>`Kusto Internal Ingestion Azure AD App (76263cdb-1234-5678-9012-545644e9c404)` 主体（Kusto 的内部引入应用）一成不变地映射到 `Cluster Admin` 角色。 这样，它就有权将数据引入到任何表。 这种情况适用于 Kusto 托管的引入管道。

向 Azure AD 应用 `Test App (2a904276-1234-5678-9012-66fc53add60b in Azure AD tenant microsoft.com)` 授予数据库 `DB1` 或表 `T1` 上的必需权限的代码如下所示：

```kusto
.add database DB1 ingestors ('aadapp=2a904276-1234-5678-9012-66fc53add60b;microsoft.com') 'Test Azure AD App'
.add table T1 ingestors ('aadapp=2a904276-1234-5678-9012-66fc53add60b;microsoft.com') 'Test Azure AD App'
```
 