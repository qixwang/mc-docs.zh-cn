---
title: Azure Cosmos DB 的安全控制
description: 获取安全控制（如网络、监视、标识和数据保护）的清单，以评估 Azure Cosmos DB
services: cosmos-db
author: rockboyfor
ms.service: cosmos-db
ms.topic: conceptual
origin.date: 12/02/2019
ms.date: 01/20/2020
ms.author: v-yeche
ms.openlocfilehash: a8d0d844adfb9fa735082634a99ba9c8807a172e
ms.sourcegitcommit: 23dc63b6fea451f6a2bd4e8d0fbd7ed082ba0740
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/04/2020
ms.locfileid: "76980435"
---
# <a name="security-controls-for-azure-cosmos-db"></a>Azure Cosmos DB 的安全控制

本文介绍 Azure Cosmos DB 中内置的安全控制。

[!INCLUDE [Security controls Header](../../includes/security-controls-header.md)]

## <a name="network"></a>网络

| 安全控制 | 是/否 | 注释 |
|---|---|--|
| 服务终结点支持| 是 |  |
| VNet 注入支持| 是 | 使用 VNet 服务终结点可将 Azure Cosmos DB 帐户配置为仅允许从虚拟网络 (VNet) 的特定子网进行访问。 还可以将 VNet 访问与防火墙规则相结合。 若要了解详细信息，请参阅[从虚拟网络访问 Azure Cosmos DB](VNet-service-endpoint.md)。 |
| 网络隔离和防火墙支持| 是 | 借助防火墙支持，可将 Azure Cosmos 帐户配置为仅允许从一组已批准的 IP 地址、某个 IP 地址范围和/或云服务进行访问。 若要了解详细信息，请参阅[在 Azure Cosmos DB 中配置 IP 防火墙](how-to-configure-firewall.md)。|
| 强制隧道支持| 是 | 可以在虚拟机所在的 VNet 中的客户端上配置。   |

## <a name="monitoring--logging"></a>监视和日志记录

| 安全控制 | 是/否 | 注释|
|---|---|--|
| Azure 监视支持（Log Analytics、App Insights 等）| 是 | 发送到 Azure Cosmos DB 的所有请求将被记录。 支持 [Azure 监视](../azure-monitor/overview.md)、Azure 指标和 Azure 审核日志记录。  可以记录对应于数据平面请求、查询运行时统计、查询文本和 MongoDB 请求的信息。 还可以设置警报。 |
| 控制和管理平面日志记录和审核| 是 | 帐户级操作（例如防火墙、VNet、密钥访问和 IAM）的 Azure 活动日志。 |
| 数据平面日志记录和审核 | 是 | 容器级操作（例如创建容器、预配吞吐量、为策略编制索引，以及对文档执行 CRUD 操作）的诊断监视日志记录。 |

## <a name="identity"></a>标识

| 安全控制 | 是/否 | 注释|
|---|---|--|
| 身份验证| 是 | 在数据库帐户级别为“是”；在数据平面级别，Cosmos DB 将使用资源令牌和密钥访问。 |
| 授权| 是 | 在使用主密钥（主要和辅助密钥）与资源令牌的 Azure Cosmos 帐户级别受支持。 可以使用主密钥获取对数据的读/写或只读访问权限。 使用资源令牌可以在有限的时间内访问文档和容器等资源。 |

## <a name="data-protection"></a>数据保护

| 安全控制 | 是/否 | 注释 |
|---|---|--|
| 服务器端静态加密：Azure 托管的密钥 | 是 | 所有 Azure Cosmos 数据库和备份默认已加密；请参阅 [Azure Cosmos DB 中的数据加密](database-encryption-at-rest.md)。 |
| 服务器端静态加密：客户管理的密钥 (BYOK) | 是 | 请参阅[为 Azure Cosmos DB 帐户配置客户托管密钥](how-to-setup-cmk.md)  |
| 列级加密（Azure 数据服务）| 是 | 只能在表 API 高级版中使用。 并非所有 API 都支持此功能。 请参阅 [Azure Cosmos DB 简介：表 API](table-introduction.md)。 |
| 传输中加密（例如 ExpressRoute 加密、VNet 中加密，以及 VNet-VNet 加密）| 是 | 所有 Azure Cosmos DB 数据在传输中都会经过加密。 |
| 加密的 API 调用| 是 | 与 Azure Cosmos DB 建立的所有连接都支持 HTTPS。 Azure Cosmos DB 还支持 TLS 1.2。<br />可以在服务器端强制实施最低 TLS 版本。 为此，请联系 [azurecosmosdbtls@service.microsoft.com](mailto:azurecosmosdbtls@service.microsoft.com)。 |

## <a name="configuration-management"></a>配置管理

| 安全控制 | Yes/No | 注释|
|---|---|--|
| 配置管理支持（配置的版本控制等）| 否  | | 

## <a name="additional-security-controls-for-cosmos-db"></a>Cosmos DB 的其他安全控制

| 安全控制 | 是/否 | 注释|
|---|---|--|
| 跨域资源共享 (CORS) | 是 | 请参阅[配置跨域资源共享 (CORS)](how-to-configure-cross-origin-resource-sharing.md)。 |

<!--Not Available on ## Next steps-->
<!--Not Available on [built-in security controls across Azure services](../security/fundamentals/security-controls.md)-->

<!-- Update_Description: update meta properties, wording update, update link -->