---
title: 安全性和数据隐私
titleSuffix: Azure Cognitive Search
description: Azure 认知搜索符合 SOC 2、HIPAA 和其他认证的要求。 在筛选器表达式中通过用户和组安全标识符进行连接和数据加密、身份验证和标识访问。
manager: nitinme
author: HeidiSteen
ms.author: v-tawe
ms.service: cognitive-search
ms.topic: conceptual
origin.date: 04/15/2020
ms.date: 04/20/2020
ms.openlocfilehash: 625647ea14b181d4a5bdd56c268d810034893959
ms.sourcegitcommit: 89ca2993f5978cd6dd67195db7c4bdd51a677371
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/30/2020
ms.locfileid: "82588707"
---
# <a name="security-and-data-privacy-in-azure-cognitive-search"></a>Azure 认知搜索中的安全性和数据隐私

Azure 认知搜索中内置了全面的安全功能和访问控制，以确保私有内容保持原有状态。 本文枚举了 Azure 认知搜索中内置的安全功能和标准符合性。

Azure 认知搜索安全体系结构涵盖物理安全性、加密传输、加密存储和平台范围标准符合性。 在操作上，Azure 认知搜索仅接受经过身份验证的请求。 可以选择通过安全筛选器针对内容添加基于用户的访问控制。 本文涉及每个层的安全性，但侧重于介绍如何在 Azure 认知搜索中保护数据和操作。

## <a name="standards-compliance-iso-27001-soc-2-hipaa"></a>标准符合性：ISO 27001、SOC 2、HIPAA

Azure 认知搜索针对以下标准进行了认证，如 [2018 年 6 月发布的公告](https://azure.microsoft.com/blog/azure-search-is-now-certified-for-several-levels-of-compliance/)所述：

+ [ISO 27001:2013](https://www.iso.org/isoiec-27001-information-security.html) 
+ [SOC 2 类型 2 符合性](https://www.aicpa.org/interestareas/frc/assuranceadvisoryservices/aicpasoc2report.html) 有关完整报告，请转到 [Azure - Azure 政府版 SOC 2 类型 II 报告](https://servicetrust.microsoft.com/ViewPage/MSComplianceGuide?command=Download&downloadType=Document&downloadId=93292f19-f43e-4c4e-8615-c38ab953cf95&docTab=4ce99610-c9c0-11e7-8c2c-f908a777fa4d_SOC%20%2F%20SSAE%2016%20Reports)。 
+ [健康保险可携性和责任法案 (HIPAA)](https://en.wikipedia.org/wiki/Health_Insurance_Portability_and_Accountability_Act)
+ [GxP（CFR 第 21 篇第·11 部分）](https://en.wikipedia.org/wiki/Title_21_CFR_Part_11)
+ [HITRUST](https://en.wikipedia.org/wiki/HITRUST)
+ [PCI DSS 1 级](https://en.wikipedia.org/wiki/Payment_Card_Industry_Data_Security_Standard)

标准符合性应用于正式版功能。 预览版功能在转变为正式版时进行认证，不能用于具有严格标准要求的解决方案中。 符合性认证记录在 [Microsoft Azure 符合性概述](https://gallery.technet.microsoft.com/Overview-of-Azure-c1be3942)和[信任中心](https://www.trustcenter.cn/default.html)中。 

## <a name="encrypted-transmission-and-storage"></a>加密的传输和存储

加密扩展到整个索引管道：从连接到传输到存储在 Azure 认知搜索中的索引数据，都进行了加密。

| 安全层 | 说明 |
|----------------|-------------|
| 传输中加密 <br>(HTTPS/SSL/TLS) | Azure 认知搜索在 HTTPS 端口 443 上侦听。 与 Azure 服务建立的跨平台连接经过加密。 <br/><br/>所有从客户端到服务的 Azure 认知搜索交互都支持 SSL/TLS 1.2。  请务必为你的服务的 SSL 连接使用 TLSv1.2。|
| 静态加密 <br>Microsoft 托管的密钥 | 加密在索引过程中完全进行内部化处理，而不会显著影响完成索引所需的时间或索引大小。 加密自动对所有索引进行，包括对未完全加密的索引（在 2018 年 1 月前创建）的增量更新。<br><br>在内部，加密基于 [Azure 存储服务加密](https://docs.azure.cn/storage/common/storage-service-encryption)，使用 256 位 [AES 加密](https://en.wikipedia.org/wiki/Advanced_Encryption_Standard)进行。<br><br> 加密在 Azure 认知搜索内部进行，证书和加密密钥由 Microsoft 进行内部管理，并得到了广泛应用。 无法在门户中或以编程方式打开或关闭加密、管理或替换为自己的密钥，或者查看加密设置。<br><br>静态加密已于 2018 年 1 月 24 日宣布推出并应用于所有区域中的所有服务层级，包括免费层。 对于完全加密，必须删除该日期之前创建的索引并重新生成，以便进行加密。 否则，仅对 1 月 24 日以后添加的新数据进行加密。|

<!-- | Encryption at rest <br>Customer managed keys | Encryption with customer managed keys is now generally available for search services created on or after January 2019. It is not supported on Free (shared) services.<br><br>Azure Cognitive Search indexes and synonym maps can now be encrypted at rest with customer keys managed keys in Azure Key Vault. To learn more, see [Manage encryption keys in Azure Cognitive Search](search-security-manage-encryption-keys.md).<br><br>This feature is not replacing the default encryption at rest, but rather applied in addition to it.<br><br>Enabling this feature will increase index size and degrade query performance. Based on observations to date, you can expect to see an increase of 30%-60% in query times, although actual performance will vary depending on the index definition and types of queries. Because of this performance impact, we recommend that you only enable this feature on indexes that really require it. -->

## <a name="azure-wide-user-access-controls"></a>Azure 范围的用户访问控制

我们在 Azure 范围内提供多种安全机制，因此，你创建的 Azure 认知搜索资源会自动获得这些安全机制。

+ [订阅或资源级别的锁可防止删除](../azure-resource-manager/management/lock-resources.md)
+ [基于角色的访问控制 (RBAC) 可以控制对信息和管理操作的访问](../role-based-access-control/overview.md)

所有 Azure 服务支持使用基于角色的访问控制 (RBAC) 在不同的服务之间以一致的方式设置访问级别。 例如，仅限“所有者”和“参与者”角色可查看敏感数据（如管理密钥）。 但是，任何角色的成员都可以查看服务状态。 RBAC 提供“所有者”、“参与者”和“读取者”角色。 默认情况下，所有服务管理员是“所有者”角色的成员。

<a name="service-access-and-authentication"></a>

## <a name="endpoint-access"></a>终结点访问

### <a name="public-access"></a>公共访问权限

Azure 认知搜索继承了 Azure 平台的安全保护措施，并提供了自己的基于密钥的身份验证。 API 密钥是随机生成的数字和字母所组成的字符串。 密钥的类型（管理员或查询）确定访问的级别。 提交有效密钥被视为请求源自受信任实体的证明。 

有两个搜索服务访问级别，可通过两种类型的密钥启用它们：

* 管理员访问（适用于服务的任何读写操作）
* 查询访问权限（适用于只读操作，例如，针对索引文档集合运行的查询）

预配服务时会创建*管理员密钥*。 有两个管理密钥，分别指定为*主要*和*辅助*密钥以将它们保持在各自的位置，但事实上是可互换的。 每个服务都有两个管理密钥，以便在转换其中一个时不会丢失服务的访问权限。 可以根据 Azure 安全最佳做法定期[重新生成管理密钥](search-security-api-keys.md#regenerate-admin-keys)，但不能将其添加到管理密钥总数。 每个搜索服务最多有两个管理密钥。

*查询密钥*根据需要创建，专用于发出查询的客户端应用程序。 最多可以创建 50 个查询密钥。 在应用程序代码中，可以指定搜索 URL 和查询 API 密钥，以便对特定索引的文档集合进行只读访问。 终结点、仅供只读访问的 API 密钥以及目标索引共同定义客户端应用程序连接的作用域和访问级别。

需要对每个请求进行身份验证，而每个请求由必需密钥、操作和对象组成。 链接在一起后，两个权限级别（完全或只读）加上上下文（例如，索引上的查询操作）便足以针对服务操作提供全面的安全性。 有关密钥的详细信息，请参阅[创建和管理 API 密钥](search-security-api-keys.md)。

### <a name="restricted-access"></a>受限访问

如果你有公共服务，但想要限制该服务的使用，则可使用管理 REST API 版本中的 IP 限制规则：2020-03-13 [IpRule](https://docs.microsoft.com/rest/api/searchmanagement/2019-10-01-preview/createorupdate-service#IpRule)。 IpRule 允许你标识希望向其授予搜索服务访问权限的 IP 地址（单个或在某个范围内），通过这种方式限制对该服务的访问。 

<!-- ### Private access ->

## Index access

In Azure Cognitive Search, an individual index is not a securable object. Instead, access to an index is determined at the service layer (read or write access), along with the context of an operation.

For end-user access, you can structure query requests to connect using a query key, which makes any request read-only, and include the specific index used by your app. In a query request, there is no concept of joining indexes or accessing multiple indexes simultaneously so all requests target a single index by definition. As such, construction of the query request itself (a key plus a single target index) defines the security boundary.

Administrator and developer access to indexes is undifferentiated: both need write access to create, delete, and update objects managed by the service. Anyone with an admin key to your service can read, modify, or delete any index in the same service. For protection against accidental or malicious deletion of indexes, your in-house source control for code assets is the remedy for reversing an unwanted index deletion or modification. Azure Cognitive Search has failover within the cluster to ensure availability, but it does not store or execute your proprietary code used to create or load indexes.

For multitenancy solutions requiring security boundaries at the index level, such solutions typically include a middle tier, which customers use to handle index isolation. For more information about the multitenant use case, see [Design patterns for multitenant SaaS applications and Azure Cognitive Search](search-modeling-multitenant-saas-applications.md).

## Authentication

### Admin access

[Role-based access (RBAC)](https://docs.azure.cn/role-based-access-control/overview) determines whether you have access to controls over the service and its content. If you are an Owner or Contributor on an Azure Cognitive Search service, you can use the portal or the PowerShell **Az.Search** module to create, update, or delete objects on the service. You can also use the [Azure Cognitive Search Management REST API](https://docs.microsoft.com/rest/api/searchmanagement/search-howto-management-rest-api).

### User access

By default, user access to an index is determined by the access key on the query request. Most developers create and assign [*query keys*](search-security-api-keys.md) for client-side search requests. A query key grants read access to all content within the index.

If you require granular, per-user control over content, you can build security filters on your queries, returning documents associated with a given security identity. Instead of predefined roles and role assignments, identity-based access control is implemented as a *filter* that trims search results of documents and content based on identities. The following table describes two approaches for trimming search results of unauthorized content.

| Approach | Description |
|----------|-------------|
|[Security trimming based on identity filters](search-security-trimming-for-azure-search.md)  | Documents the basic workflow for implementing user identity access control. It covers adding security identifiers to an index, and then explains filtering against that field to trim results of prohibited content. |
|[Security trimming based on Azure Active Directory identities](search-security-trimming-for-azure-search-with-aad.md)  | This article expands on the previous article, providing steps for retrieving identities from Azure Active Directory (AAD), one of the [free services](https://www.azure.cn/pricing/1rmb-trial/) in the Azure cloud platform. |

## Table: Permissioned operations

The following table summarizes the operations allowed in Azure Cognitive Search and which key unlocks access a particular operation.

| Operation | Permissions |
|-----------|-------------------------|
| Create a service | Azure subscription holder|
| Scale a service | Admin key, RBAC Owner, or Contributor on the resource  |
| Delete a service | Admin key, RBAC Owner, or Contributor on the resource |
| Create, modify, delete objects on the service: <br>Indexes and component parts (including analyzer definitions, scoring profiles, CORS options), indexers, data sources, synonyms, suggesters. | Admin key, RBAC Owner, or Contributor on the resource  |
| Query an index | Admin or query key (RBAC not applicable) |
| Query system information, such as returning statistics, counts, and lists of objects. | Admin key, RBAC on the resource (Owner, Contributor, Reader) |
| Manage admin keys | Admin key, RBAC Owner or Contributor on the resource. |
| Manage query keys |  Admin key, RBAC Owner or Contributor on the resource.  |

## Physical security

Azure data centers provide industry-leading physical security and are compliant with an extensive portfolio of standards and regulations.

## See also

+ [Get started .NET (demonstrates using an admin key to create an index)](search-create-index-dotnet.md)
+ [Get started REST (demonstrates using an admin key to create an index)](search-create-index-rest-api.md)
+ [Identity-based access control using Azure Cognitive Search filters](search-security-trimming-for-azure-search.md)
+ [Active Directory identity-based access control using Azure Cognitive Search filters](search-security-trimming-for-azure-search-with-aad.md)
+ [Filters in Azure Cognitive Search](search-filters.md)