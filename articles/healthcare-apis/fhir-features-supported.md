---
title: Azure 中支持的 FHIR 功能 - Azure API for FHIR
description: 本文介绍 Azure API for FHIR 中实现的 FHIR 规范功能
services: healthcare-apis
author: hansenms
ms.service: healthcare-apis
ms.subservice: fhir
ms.topic: reference
ms.date: 02/07/2019
ms.author: mihansen
ms.openlocfilehash: 5520a6afc18dec7f07324cbf3b2b153924fcd7cb
ms.sourcegitcommit: 6ddc26f9b27acec207b887531bea942b413046ad
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/27/2020
ms.locfileid: "80343855"
---
# <a name="features"></a>功能

Azure API for FHIR 为适用于 Azure 的 Microsoft FHIR 服务器提供完全托管的部署。 该服务器是 [FHIR](https://hl7.org/fhir) 标准的实现。 本文档列出了 FHIR 服务器的主要功能。

## <a name="fhir-version"></a>FHIR 版本

支持的最新版本：`4.0.0`

目前还支持的旧版本包括：`3.0.1`

## <a name="rest-api"></a>REST API

| API                            | 支持 - PaaS | 支持 - OSS (SQL) | 支持 - OSS (Cosmos DB) | 注释                                             |
|--------------------------------|-----------|-----------|-----------|-----------------------------------------------------|
| read                           | 是       | 是       | 是       |                                                     |
| vread                          | 是       | 是       | 是       |                                                     |
| update                         | 是       | 是       | 是       |                                                     |
| 使用乐观锁定的更新 | 是       | 是       | 是       |                                                     |
| 更新（条件性）           | 是       | 是       | 是       |                                                     |
| 修补                          | 否        | 否        | 否        |                                                     |
| 删除                         | 是       | 是       | 是       |                                                     |
| 删除（条件性）           | 否        | 否        | 否        |                                                     |
| create                         | 是       | 是       | 是       | 支持 POST/PUT                               |
| 创建（条件性）           | 是       | 是       | 是       |                                                     |
| 搜索                         | 部分   | 部分   | 部分   | 请参阅下文                                           |
| capabilities                   | 是       | 是       | 是       |                                                     |
| 或批处理                          | 是       | 是       | 是       |                                                     |
| transaction                    | 否        | 是       | 否        |                                                     |
| history                        | 是       | 是       | 是       |                                                     |
| 分页                         | 部分   | 部分   | 部分   | 支持 `self` 和 `next`                     |
| 中间项                 | 否        | 否        | 否        |                                                     |

## <a name="search"></a>搜索

支持所有搜索参数类型。 不支持链接的参数和反向链接。 

| 搜索参数类型 | 支持 - PaaS | 支持 - OSS (SQL) | 支持 - OSS (Cosmos DB) | 注释 |
|-----------------------|-----------|-----------|-----------|---------|
| Number                | 是       | 是       | 是       |         |
| 日期/日期时间         | 是       | 是       | 是       |         |
| String                | 是       | 是       | 是       |         |
| 令牌                 | 是       | 是       | 是       |         |
| 参考             | 是       | 是       | 是       |         |
| 复合实体             | 是       | 是       | 是       |         |
| 数量              | 是       | 是       | 是       |         |
| URI                   | 是       | 是       | 是       |         |
| 特殊               | 否        | 否        | 否        |         |


| 修饰符             | 支持 - PaaS | 支持 - OSS (SQL) | 支持 - OSS (Cosmos DB) | 注释 |
|-----------------------|-----------|-----------|-----------|---------|
|`:missing`             | 是       | 是       | 是       |         |
|`:exact`               | 是       | 是       | 是       |         |
|`:contains`            | 是       | 是       | 是       |         |
|`:text`                | 是       | 是       | 是       |         |
|`:in`（标记）          | 否        | 否        | 否        |         |
|`:below`（标记）       | 否        | 否        | 否        |         |
|`:above`（标记）       | 否        | 否        | 否        |         |
|`:not-in`（标记）      | 否        | 否        | 否        |         |
|`:[type]`（引用）  | 否        | 否        | 否        |         |
|`:below` (uri)         | 是       | 是       | 是       |         |
|`:not`                 | 否        | 否        | 否        |         |
|`:above` (uri)         | 否        | 否        | 否        | 问题 [#158](https://github.com/Microsoft/fhir-server/issues/158) |

| 常用搜索参数 | 支持 - PaaS | 支持 - OSS (SQL) | 支持 - OSS (Cosmos DB) | 注释 |
|-------------------------| ----------| ----------| ----------|---------|
| `_id`                   | 是       | 是       | 是       |         |
| `_lastUpdated`          | 是       | 是       | 是       |         |
| `_tag`                  | 是       | 是       | 是       |         |
| `_profile`              | 是       | 是       | 是       |         |
| `_security`             | 是       | 是       | 是       |         |
| `_text`                 | 否        | 否        | 否        |         |
| `_content`              | 否        | 否        | 否        |         |
| `_list`                 | 否        | 否        | 否        |         |
| `_has`                  | 否        | 否        | 否        |         |
| `_type`                 | 是       | 是       | 是       |         |
| `_query`                | 否        | 否        | 否        |         |

| 搜索操作       | 支持 - PaaS | 支持 - OSS (SQL) | 支持 - OSS (Cosmos DB) | 注释 |
|-------------------------|-----------|-----------|-----------|---------|
| `_filter`               | 否        | 否        | 否        |         |
| `_sort`                 | 否        | 否        | 否        |         |
| `_score`                | 否        | 否        | 否        |         |
| `_count`                | 是       | 是       | 是       |         |
| `_summary`              | 部分   | 部分   | 部分   | 支持 `_summary=count` |
| `_include`              | 否        | 是       | 否        |         |
| `_revinclude`           | 否        | 否        | 否        |         |
| `_contained`            | 否        | 否        | 否        |         |
| `_elements`             | 否        | 否        | 否        |         |

## <a name="persistence"></a>持久性

Microsoft FHIR 服务器包含可插接式的持久性模块（请参阅 [`Microsoft.Health.Fhir.Core.Features.Persistence`](https://github.com/Microsoft/fhir-server/tree/master/src/Microsoft.Health.Fhir.Core/Features/Persistence)）。

目前，FHIR 服务器开源代码包括 [Azure Cosmos DB](../cosmos-db/index-overview.md) 和 [SQL 数据库](/sql-database/)的实现。

Cosmos DB 是全球分布式的多模型（SQL API、MongoDB API 等）数据库。 它支持不同的[一致性级别](../cosmos-db/consistency-levels.md)。 默认部署模板使用 `Strong` 一致性配置 FHIR 服务器，但可以使用 `x-ms-consistency-level` 请求标头按请求修改一致性策略（通常为宽松策略）。

## <a name="role-based-access-control"></a>基于角色的访问控制

FHIR 服务器使用 [Azure Active Directory](/active-directory/) 进行访问控制。 具体而言，如果 `FhirServer:Security:Enabled` 配置参数设置为 `true`，则强制实施基于角色的访问控制 (RBAC)；对 FHIR 服务器发出的所有请求（`/metadata` 除外）的 `Authorization` 请求标头必须设置为 `Bearer <TOKEN>`。 令牌必须包含 `roles` 声明中定义的一个或多个角色。 如果令牌包含允许对指定的资源执行指定操作的角色，则将允许请求。

目前，允许对给定角色执行的操作将在 API 中全局应用。 

## <a name="next-steps"></a>后续步骤

在本文中，你已了解 Azure API for FHIR 中支持的 FHIR 功能。 接下来请部署 Azure API for FHIR。
 
>[!div class="nextstepaction"]
>[部署 Azure API for FHIR](fhir-paas-portal-quickstart.md)