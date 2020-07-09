---
title: 安全概述
titleSuffix: Azure Cognitive Search
description: Azure 认知搜索符合 SOC 2、HIPAA 和其他认证的要求。 在筛选器表达式中通过用户和组安全标识符进行连接和数据加密、身份验证和标识访问。
manager: nitinme
author: HeidiSteen
ms.author: v-tawe
ms.service: cognitive-search
ms.topic: conceptual
origin.date: 06/03/2020
ms.date: 07/02/2020
ms.openlocfilehash: 55f5c515bd226f222dd96ce07fc4a322b9101f9f
ms.sourcegitcommit: 5afd7c4c3be9b80c4c67ec55f66fcf347aad74c6
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/03/2020
ms.locfileid: "85942585"
---
# <a name="security-in-azure-cognitive-search---overview"></a>Azure 认知搜索中的安全性 - 概述

本文介绍 Azure 认知搜索中可以保护内容和操作的关键安全功能。 

+ 在存储层上，静态加密是在平台级别进行的，但对于想要对用户自有密钥和 Microsoft 托管密钥提供双重保护的客户，认知搜索还提供了“双重加密”选项。

+ 入站安全性通过不断提升的安全性级别来保护搜索服务终结点：从请求所使用的 API 密钥到防火墙中的入站规则，再到全面保护服务不受公共 Internet 影响的专用终结点。

<!-- + Outbound security applies to indexers that pull content from external sources. For outbound requests, set up a managed identity to make search a trusted service when accessing data from Azure Storage, Azure SQL, Cosmos DB, or other Azure data sources. A managed identity is a substitute for credentials or access keys on the connection. Outbound security is not covered in this article. For more information about this capability, see [Connect to a data source using a managed identity](search-howto-managed-identities-data-sources.md). -->

<!--
Watch this fast-paced video for an overview of the security architecture and each feature category.

> [!VIDEO https://channel9.msdn.com/Shows/AI-Show/Azure-Cognitive-Search-Whats-new-in-security/player]
-->

## <a name="encrypted-transmissions-and-storage"></a>加密的传输和存储

在 Azure 认知搜索中，加密无处不在：从连接和传输开始，一直延伸到磁盘上存储的内容。 对于公共 Internet 上的搜索服务，Azure 认知搜索会侦听 HTTPS 端口 443。 客户端到服务的所有连接都使用 TLS 1.2 加密。 不支持更早的版本（1.0 或 1.1）。

### <a name="data-encryption-at-rest"></a>静态数据加密

Azure 认知搜索存储了索引定义和内容、数据源定义、索引器定义、技能组定义和同义词映射。

在整个存储层中，系统使用 Microsoft 管理的密钥在磁盘上对数据进行加密。 你无法在门户中或以编程方式启用或禁用加密，或者查看加密设置。 加密已完全内部化，对完成索引所需的时间或索引大小并无显著影响。 加密自动对所有索引进行，包括对未完全加密的索引（在 2018 年 1 月前创建）的增量更新。

在内部，加密基于 [Azure 存储服务加密](../storage/common/storage-service-encryption.md)，使用 256 位 AES 加密进行。

> [!NOTE]
> 静态加密已于 2018 年 1 月 24 日宣布推出并应用于所有区域中的所有服务层级，包括免费层。 对于完全加密，必须删除该日期之前创建的索引并重新生成，以便进行加密。 否则，仅对 1 月 24 日以后添加的新数据进行加密。

### <a name="customer-managed-key-cmk-encryption"></a>客户管理的密钥 (CMK) 加密

对于想要获得额外存储保护的客户，可以在磁盘上存储和加密数据和对象之前，对这些数据和对象进行加密。 此方法基于通过 Azure 密钥保管库管理和存储并独立于 Microsoft 的用户自有密钥。 在磁盘上加密内容之前对其进行加密称为“双重加密”。 目前，可以有选择地对索引和同义词映射进行双重加密。 有关详细信息，请参阅 [Azure 认知搜索中客户管理的加密密钥](search-security-manage-encryption-keys.md)。

> [!NOTE]
> CMK 加密常用于 2019 年 1 月之后创建的搜索服务。 免费（共享）服务不支持此功能。 
>
>启用此功能会增大索引大小，降低查询性能。 根据迄今为止的观察结果，查询时间预期会增加 30%-60%，不过，实际性能根据索引定义和查询类型而有所不同。 由于这种性能影响，我们建议仅对真正需要此功能的索引启用此功能。

<a name="service-access-and-authentication"></a>

## <a name="inbound-security-and-endpoint-protection"></a>入站安全性和终结点保护

入站安全性功能通过不断提升的安全性和复杂性级别来保护搜索服务终结点。 首先，所有请求都需要 API 密钥才能进行经过身份验证的访问。 其次，你可以选择设置防火墙规则，以限制对特定 IP 地址的访问。 为了提供高级保护，第三个选项是启用 Azure 专用链接来保护服务终结点不受所有 Internet 流量的影响。

### <a name="public-access-using-api-keys"></a>使用 API 密钥进行公共访问

默认情况下，将使用基于密钥的身份验证（用于对搜索服务终结点进行管理员或查询访问）通过公有云来访问搜索服务。 API 密钥是随机生成的数字和字母所组成的字符串。 密钥的类型（管理员或查询）确定访问的级别。 提交有效密钥被视为请求源自受信任实体的证明。 

有两个搜索服务访问级别，可通过以下 API 密钥启用它们：

+ 管理员密钥（允许对搜索服务上的创建-读取-更新-删除操作进行读写访问）

+ 查询密钥（允许对索引的文档集合进行只读访问）

预配服务时会创建*管理员密钥*。 有两个管理密钥，分别指定为*主要*和*辅助*密钥以将它们保持在各自的位置，但事实上是可互换的。 每个服务都有两个管理密钥，以便在转换其中一个时不会丢失服务的访问权限。 可以根据 Azure 安全最佳做法定期[重新生成管理密钥](search-security-api-keys.md#regenerate-admin-keys)，但不能将其添加到管理密钥总数。 每个搜索服务最多有两个管理密钥。

*查询密钥*根据需要创建，专用于发出查询的客户端应用程序。 最多可以创建 50 个查询密钥。 在应用程序代码中，可以指定搜索 URL 和查询 API 密钥，以便对特定索引的文档集合进行只读访问。 终结点、仅供只读访问的 API 密钥以及目标索引共同定义客户端应用程序连接的作用域和访问级别。

需要对每个请求进行身份验证，而每个请求由必需密钥、操作和对象组成。 链接在一起后，两个权限级别（完全或只读）加上上下文（例如，索引上的查询操作）便足以针对服务操作提供全面的安全性。 有关密钥的详细信息，请参阅[创建和管理 API 密钥](search-security-api-keys.md)。

### <a name="ip-restricted-access"></a>IP 限制访问

为了进一步控制对搜索服务的访问，可以创建入站防火墙规则，以允许访问特定 IP 地址或某个范围的 IP 地址。 所有客户端连接必须通过允许的 IP 地址建立，否则连接将被拒绝。

可以使用门户[配置入站访问](service-configure-firewall.md)。 

或者，可以使用管理 REST API。 带有 [IpRule](https://docs.microsoft.com/rest/api/searchmanagement/2019-10-01-preview/createorupdate-service#IpRule) 参数的 API 版本 2020-03-13 允许你标识希望向其授予搜索服务访问权限的 IP 地址（单个或在某个范围内），通过这种方式限制对该服务的访问。 

<!-- ### Private access ->

## Index access

In Azure Cognitive Search, an individual index is not a securable object. Instead, access to an index is determined at the service layer (read or write access to the service), along with the context of an operation.

For end-user access, you can structure query requests to connect using a query key, which makes any request read-only, and include the specific index used by your app. In a query request, there is no concept of joining indexes or accessing multiple indexes simultaneously so all requests target a single index by definition. As such, construction of the query request itself (a key plus a single target index) defines the security boundary.

Administrator and developer access to indexes is undifferentiated: both need write access to create, delete, and update objects managed by the service. Anyone with an admin key to your service can read, modify, or delete any index in the same service. For protection against accidental or malicious deletion of indexes, your in-house source control for code assets is the remedy for reversing an unwanted index deletion or modification. Azure Cognitive Search has failover within the cluster to ensure availability, but it does not store or execute your proprietary code used to create or load indexes.

For multitenancy solutions requiring security boundaries at the index level, such solutions typically include a middle tier, which customers use to handle index isolation. For more information about the multitenant use case, see [Design patterns for multitenant SaaS applications and Azure Cognitive Search](search-modeling-multitenant-saas-applications.md).

## User access

How a user accesses an index and other objects is determined by the type of API key on the request. Most developers create and assign [*query keys*](search-security-api-keys.md) for client-side search requests. A query key grants read-only access to searchable content within the index.

If you require granular, per-user control over search results, you can build security filters on your queries, returning documents associated with a given security identity. Instead of predefined roles and role assignments, identity-based access control is implemented as a *filter* that trims search results of documents and content based on identities. The following table describes two approaches for trimming search results of unauthorized content.

| Approach | Description |
|----------|-------------|
|[Security trimming based on identity filters](search-security-trimming-for-azure-search.md)  | Documents the basic workflow for implementing user identity access control. It covers adding security identifiers to an index, and then explains filtering against that field to trim results of prohibited content. |
|[Security trimming based on Azure Active Directory identities](search-security-trimming-for-azure-search-with-aad.md)  | This article expands on the previous article, providing steps for retrieving identities from Azure Active Directory (AAD), one of the [free services](https://www.azure.cn/pricing/1rmb-trial/) in the Azure cloud platform. |

## Administrative rights

[Role-based access (RBAC)](../role-based-access-control/overview.md) is an authorization system built on [Azure Resource Manager](../azure-resource-manager/management/overview.md) for provisioning of Azure resources. In Azure Cognitive Search, Resource Manager is used to create or delete the service, manage API keys, and scale the service. As such, RBAC role assignments will determine who can perform those tasks, regardless of whether they are using the [portal](search-manage.md), [PowerShell](search-manage-powershell.md), or the [Management REST APIs](https://docs.microsoft.com/rest/api/searchmanagement/search-howto-management-rest-api).

In contrast, admin rights over content hosted on the service, such as the ability to create or delete an index, is conferred through API keys as described in the [previous section](#index-access).

> [!TIP]
> Using Azure-wide mechanisms, you can lock a subscription or resource to prevent accidental or unauthorized deletion of your search service by users with admin rights. For more information, see [Lock resources to prevent unexpected deletion](../azure-resource-manager/management/lock-resources.md).

<!--
## Certifications and compliance

Azure Cognitive Search has been certified compliant for multiple global, regional, and industry-specific standards for both the public cloud and Azure Government. For the complete list, download the [**Microsoft Azure Compliance Offerings** whitepaper](https://aka.ms/azurecompliance) from the official Audit reports page.
-->

## <a name="see-also"></a>另请参阅

+ [Azure 安全基础知识](../security/fundamentals/index.yml)
+ [Azure 安全性](https://docs.azure.cn/security)
+ [Azure 安全中心](../security-center/index.yml)