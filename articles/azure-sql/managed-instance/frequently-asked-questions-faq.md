---
title: 常见问题 (FAQ)
titleSuffix: Azure SQL Managed Instance
description: Azure SQL 托管实例常见问题解答 (FAQ)
services: sql-database
ms.service: sql-managed-instance
ms.subservice: operations
ms.custom: sqldbrb=1
ms.devlang: ''
ms.topic: conceptual
author: WenJason
ms.author: v-jay
ms.reviewer: sstein, carlrab
origin.date: 03/17/2020
ms.date: 07/13/2020
ms.openlocfilehash: 5c8e22f7e3ae2dc5c3c1fd3132b2ed8b9dbbe830
ms.sourcegitcommit: fa26665aab1899e35ef7b93ddc3e1631c009dd04
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/10/2020
ms.locfileid: "86227278"
---
# <a name="azure-sql-managed-instance-frequently-asked-questions-faq"></a>Azure SQL 托管实例常见问题解答 (FAQ)
[!INCLUDE[appliesto-sqlmi](../includes/appliesto-sqlmi.md)]

本文包含有关 [Azure SQL 托管实例](sql-managed-instance-paas-overview.md)的最常见问题。

## <a name="supported-features"></a>支持的功能

**在何处可以找到 SQL 托管实例上受支持的功能列表？**

有关SQL 托管实例中受支持的功能列表，请参阅 [Azure SQL 托管实例功能](../database/features-comparison.md)。

有关 Azure SQL 托管实例与 SQL Server 之间的语法和行为差异，请参阅 [T-SQL 与 SQL Server 之间的差异](transact-sql-tsql-differences-sql-server.md)。


## <a name="tech-spec--resource-limits"></a>技术规范和资源限制
 
**在何处可以找到 SQL 托管实例的技术特征和资源限制？**

有关可用硬件的代系特征，请参阅[硬件代系的技术差异](resource-limits.md#hardware-generation-characteristics)。
有关可用的服务层级及其特征，请参阅[服务层级之间的技术差异](resource-limits.md#service-tier-characteristics)。

## <a name="known-issues--bugs"></a>已知问题和 bug

**在何处可以找到已知问题和 bug？**

有关 bug 和已知问题，请参阅[已知问题](../database/doc-changes-updates-release-notes.md#known-issues)。

## <a name="new-features"></a>新增功能

**在何处可以找到最新功能和公共预览版功能？**

对于新功能和预览功能，请参阅[发行说明](../database/doc-changes-updates-release-notes.md?tabs=managed-instance)。

## <a name="deployment-times"></a>部署时间 

**创建或更新实例或还原数据库需要多长时间？**

创建托管实例或更改服务层（vCore、存储）所需的时间取决于多个因素。 请查看[管理操作](/sql-database/sql-database-managed-instance#managed-instance-management-operations)。 

## <a name="naming-conventions"></a>命名约定

**托管实例是否可与 SQL Server本地实例同名？**

不支持更改托管实例名称。

可以更改托管实例的默认 DNS 区域 .database.chinacloudapi.cn。 

若要使用其他 DNS 区域而不是默认区域（例如“.contoso.com”），请执行以下操作： 
- 使用 CliConfig 定义别名。 该工具只是一个注册表设置包装器，因此也可以使用组策略或脚本完成此操作。
- 将 CNAME 与 TrustServerCertificate=true 选项一起使用 。

## <a name="move-a-database-from-sql-managed-instance"></a>从 SQL 托管实例移动数据库 

**如何将数据库从 SQL 托管实例移回到 SQL Server 或 Azure SQL 数据库？**

可[将数据库导出到 BACPAC](../database/database-export.md)，然后[导入 BACPAC 文件](../database/database-import.md)。 如果数据库小于 100 GB，我们建议使用此方法。

如果数据库中的所有表具有主键，则可以使用事务复制。

由于 SQL 托管实例的数据库版本高于 SQL Server，因此无法将从 SQL 托管实例创建的本机 `COPY_ONLY` 备份还原到 SQL Server。

## <a name="migrate-an-instance-database"></a>迁移实例数据库

**如何将实例数据库迁移到 Azure SQL 数据库？**

一种做法是[将数据库导出到 BACPAC](../database/database-export.md)，然后[导入 BACPAC 文件](../database/database-import.md)。 

如果数据库小于 100 GB，则建议使用此方法。 如果数据库中的所有表都具有主键，则可以使用事务复制。

## <a name="switch-hardware-generation"></a>切换硬件代系 

**能否在第 4 代和第 5 代 SQL 托管实例硬件代系之间联机切换？**

如果这两种硬件代系都可以在预配 SQL 托管实例的区域中使用，则可以在硬件代系之间自动联机切换。 在这种情况下，可以查看 [vCore 模型概述页面](../database/service-tiers-vcore.md)，该页说明了如何在硬件代系之间切换。

这是一个长时间运行的操作，因为新托管实例将在后台预配，数据库将在旧实例与新实例之间自动转移，该过程结束时，可以快速故障转移。 

**如果没有足够的 IP 地址来执行更新操作怎么办？**

如果预配托管实例的子网中没有足够的 IP 地址，需要创建一个新的子网并在其中创建新的托管实例。 同时建议在创建新的子网时分配更多 IP 地址，以免在将来的更新操作中遇到类似情况。 （若想了解子网多大算合适，请查看[如何确定 VNet 子网大小](vnet-subnet-determine-size.md)。）预配新实例后，可以在新旧实例之间手动备份和还原数据，或执行跨实例[时间点还原](point-in-time-restore.md?tabs=azure-powershell)。 


## <a name="tune-performance"></a>调整性能

**如何优化 SQL 托管实例的性能？**

“常规用途”层中的 SQL 托管实例使用远程存储，因此数据和日志文件的大小对性能的影响很大。 有关详细信息，请参阅[日志文件大小对常规用途 SQL 托管实例性能的影响](https://medium.com/azure-sqldb-managed-instance/impact-of-log-file-size-on-general-purpose-managed-instance-performance-21ad170c823e)。

如果工作负荷包含大量小型事务，请考虑将连接类型从代理切换为重定向模式。

## <a name="maximum-storage-size"></a>最大存储大小

**SQL 托管实例的最大存储大小是多少？**

SQL 托管实例的存储大小取决于所选的服务层级（“常规用途”或“业务关键”）。 有关这些服务层级的存储限制，请参阅[服务层级特征](../database/service-tiers-general-purpose-business-critical.md)。

## <a name="backup-storage-cost"></a>备份存储成本 

**备份存储是否是从 SQL 托管实例存储中扣减出来的？**

不是，备份存储不是从 SQL 托管实例的存储空间中扣减出来的。 备份存储与实例存储空间无关，其大小不受限制。 备份存储受实例数据库备份的保留期限（可配置为 7 到 35 天）的限制。 有关详细信息，请参阅[自动化备份](../database/automated-backups-overview.md)。
  
## <a name="inbound-nsg-rules"></a>入站 NSG 规则

**如何针对管理端口设置入站 NSG 规则？**

SQL 托管实例控制平面会维护用于保护管理端口的 NSG 规则。

下面是管理端口的用途：

端口 9000 和 9003 由 Azure Service Fabric 基础结构使用。 Service Fabric 的主要作用是使虚拟群集保持正常，并根据组件副本数保持目标状态。

端口 1438、1440 和 1452 由节点代理使用。 节点代理是群集中运行的一个应用程序，控制平面使用它来执行管理命令。

除了 NSG 规则外，内置防火墙还会保护网络层上的实例。 在应用程序层上，使用证书保护通信。

有关详细信息以及了解如何验证内置防火墙，请参阅 [Azure SQL 数据库托管实例内置防火墙](management-endpoint-verify-built-in-firewall.md)。


## <a name="mitigate-data-exfiltration-risks"></a>缓解数据透露风险  

**如何缓解数据透露风险？**

为了缓解任何数据透露风险，我们建议客户应用一组安全设置和控制：

- 在所有数据库上启用[透明数据加密 (TDE)](/sql-database/transparent-data-encryption-azure-sql)。
- 关闭公共语言运行时 (CLR)。 也建议在本地禁用 CLR。
- 仅使用 Azure Active Directory (Azure AD) 身份验证。
- 使用低特权 DBA 帐户访问实例。
- 为 sysadmin 帐户配置 JiT jumpbox 访问权限。
- 启用 [SQL 审核](https://docs.microsoft.com/sql/relational-databases/security/auditing/sql-server-audit-database-engine)，并将其与警报机制相集成。
- 启用[高级数据安全 (ADS)](/sql-database/sql-database-advanced-data-security) 套件中的[威胁检测](/sql-database/sql-database-threat-detection)。


## <a name="cost-saving-use-cases"></a>有关节省成本的用例

**在何处可以找到用例，以及通过 SQL 托管实例可实现的成本节省？**

SQL 托管实例案例研究：

- [Komatsu](https://customers.microsoft.com/story/komatsu-australia-manufacturing-azure)
- [KMD](https://customers.microsoft.com/en-ca/story/kmd-professional-services-azure-sql-database)
- [PowerDETAILS](https://customers.microsoft.com/story/powerdetails-partner-professional-services-azure-sql-database-managed-instance)
- [Allscripts](https://customers.microsoft.com/story/allscripts-partner-professional-services-azure)

为了让用户更好地了解部署 Azure SQL 托管实例的优势、成本和风险，我们还提供了一份 Forrester 案例研究：[Microsoft Azure SQL 数据库托管实例的总体经济影响](https://azure.microsoft.com/resources/forrester-tei-sql-database-managed-instance)。


## <a name="dns-refresh"></a>DNS 刷新 

**是否可以执行 DNS 刷新？**

目前我们不提供刷新 SQL 托管实例 DNS 服务器配置的功能。

DNS 配置最终会刷新：

- 当 DHCP 租约过期时。
- 平台升级时。

一种解决方法是将 SQL 托管实例降级为 4 个 vCore，然后再将其升级。 这样刷新 DNS 配置会产生一种负面影响。


## <a name="ip-address"></a>IP 地址

**能否使用 IP 地址连接到 SQL 托管实例？**

不支持使用 IP 地址连接到 SQL 托管实例。 SQL 托管实例主机名会映射到 SQL 托管实例虚拟群集前面的负载均衡器。 由于一个虚拟群集可以托管多个托管实例，因此如果不显式指定名称，则无法将连接路由到正确的托管实例。

有关 SQL 托管实例虚拟群集体系结构的详细信息，请参阅[虚拟群集连接体系结构](connectivity-architecture-overview.md#virtual-cluster-connectivity-architecture)。

**SQL 托管实例是否可以使用静态 IP 地址？**

在罕见但必要的情况下，我们可能需要将 SQL 托管实例联机迁移到新的虚拟群集。 需要进行这种迁移的原因是，我们的技术堆栈发生了变化，旨在提高服务的安全性和可靠性。 迁移到新的虚拟群集会导致映射到 SQL 托管实例主机名的 IP 地址发生变化。 SQL 托管实例服务不会提出静态 IP 地址支持，且有权在不另行通知的情况下，在定期维护周期更改此 IP 地址。

出于此原因，我们强烈反对依赖于 IP 地址的不可变性，因为这可能会导致不必要的停机时间。

## <a name="change-time-zone"></a>更改时区

**是否可以更改现有托管实例的时区？**

首次预配托管实例时可以设置时区配置。 不支持更改现有托管实例的时区。 有关详细信息，请参阅[时区限制](timezones-overview.md#limitations)。

解决方法包括使用适当的时区创建新的托管实例，然后执行手动备份和还原，我们建议执行[跨实例时间点还原](https://blogs.msdn.microsoft.com/sqlserverstorageengine/2018/06/07/cross-instance-point-in-time-restore-in-azure-sql-database-managed-instance/)。


## <a name="resolve-performance-issues"></a>解决性能问题

**如何解决 SQL 托管实例的性能问题？**

若要在 SQL 托管实例与 SQL Server 之间进行性能比较，可以从[有关 Azure SQL 托管实例与 SQL Server 之间的性能比较的最佳做法](https://techcommunity.microsoft.com/t5/Azure-SQL-Database/The-best-practices-for-performance-comparison-between-Azure-SQL/ba-p/683210)着手。

由于必需的完整恢复模型以及事务日志写入吞吐量[限制](/sql-database/sql-database-managed-instance-resource-limits#service-tier-characteristics)方面的原因，SQL 托管实例上的数据加载速度通常比 SQL Server 中的速度更慢。 有时，将瞬态数据载入 tempdb 而不是用户数据库，或者使用聚集列存储或内存优化表，可以解决此问题。


## <a name="restore-encrypted-backup"></a>还原已加密的备份

**是否可将加密的数据库还原到 SQL 托管实例？**

可以，无需解密数据库即可将其还原到 SQL 托管实例。 需将一个在源系统中用作加密密钥保护器的证书/密钥提供给 SQL 托管实例，才能从加密的备份文件中读取数据。 要运行此操作有两个可行的方式：

- 将证书保护器上传到 SQL 托管实例。 只能使用 PowerShell 执行此操作。 [示例脚本](/sql-database/sql-database-managed-instance-migrate-tde-certificate)描述了整个过程。
- 将非对称密钥保护器上传到 Azure Key Vault，并将 SQL 托管实例指向该保护器。 此方法类似于自带密钥 (BYOK) TDE 用例，该用例也使用 Key Vault 集成来存储加密密钥。 如果你不想将该密钥用作加密密钥保护器，只想为 SQL 托管实例提供密钥来还原加密的数据库，请按照有关[设置 BYOK TDE](/sql-database/transparent-data-encryption-azure-sql#manage-transparent-data-encryption) 的说明进行操作，不要选中“将所选密钥设为默认 TDE 保护器”复选框。

将加密保护器提供给 SQL 托管实例使用后，可以继续执行标准的数据库还原过程。

## <a name="migrate-from-sql-database"></a>从 SQL 数据库迁移 

**如何从 Azure SQL 数据库迁移到 SQL 托管实例？**

SQL 托管实例根据计算和存储大小提供与 Azure SQL 数据库相同的性能级别。 若要在单一实例上合并数据，或者只是需要一种仅在 SQL 托管实例中受支持的功能，可以使用导出/导入 (BACPAC) 功能来迁移数据。

## <a name="password-policy"></a>密码策略 

**对于 SQL 托管实例 SQL 登录名，可应用什么密码策略？**

用于 SQL 登录名的 SQL 托管实例密码策略沿袭了Azure 平台策略，该策略应用于构成保存托管实例的虚拟群集的 VM。 目前不能更改这些设置，因为这些设置由 Azure 定义并由托管实例继承。

 > [!IMPORTANT]
 > Azure 平台可能会更改策略要求，且不会通知依赖于该策略的服务。

**当前的 Azure 平台策略是什么？**

每个登陆者必须在登录时设置其密码，并在达到最长密码期限后更改其密码。

| **策略** | **安全设置** |
| --- | --- |
| 最长密码期限 | 42 天 |
| 最短密码期限 | 1 天 |
| 最短密码长度 | 10 个字符 |
| 密码必须符合复杂性要求 | Enabled |

**是否可以在登录级别禁用 SQL 托管实例中的密码复杂性和过期时间？**

是的，可以在登录级别控制 CHECK_POLICY 和 CHECK_EXPIRATION 字段。 可以执行以下 T-SQL 命令来检查当前设置：

```sql
SELECT *
FROM sys.sql_logins
```

之后，可以执行以下操作来修改指定的登录设置：

```sql
ALTER LOGIN <login_name> WITH CHECK_POLICY = OFF;
ALTER LOGIN <login_name> WITH CHECK_EXPIRATION = OFF;
```

（将“test”替换为所需的登录 ID，并调整策略和过期时间值）
