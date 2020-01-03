---
title: 用于搜索索引的 Azure SQL 托管实例连接
titleSuffix: Azure Cognitive Search
description: 启用公共终结点以允许从 Azure 认知搜索上的索引器连接到 SQL 托管实例。
manager: nitinme
author: vl8163264128
ms.author: v-tawe
ms.service: cognitive-search
ms.topic: conceptual
origin.date: 11/04/2019
ms.date: 12/16/2019
ms.openlocfilehash: 99d275c9ef1863fdde51809303d9df75a1d80634
ms.sourcegitcommit: 4a09701b1cbc1d9ccee46d282e592aec26998bff
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/25/2019
ms.locfileid: "75336030"
---
# <a name="configure-a-connection-from-an-azure-cognitive-search-indexer-to-sql-managed-instance"></a>配置从 Azure 认知搜索索引器到 SQL 托管实例的连接

如[使用索引器将 Azure SQL 数据库连接到 Azure 认知搜索](search-howto-connecting-azure-sql-database-to-azure-search-using-indexers.md#faq)中所述，Azure 认知搜索通过公共终结点支持针对 **SQL 托管实例**创建索引器。

## <a name="create-azure-sql-managed-instance-with-public-endpoint"></a>通过公共终结点创建 Azure SQL 托管实例
在选中“启用公共终结点”  选项的情况下创建 SQL 托管实例。

   ![启用公共终结点](media/search-howto-connecting-azure-sql-mi-to-azure-search-using-indexers/enable-public-endpoint.png "启用公共终结点")

## <a name="enable-azure-sql-managed-instance-public-endpoint"></a>启用 Azure SQL 托管实例公共终结点
还可以在“安全性”   > “虚拟网络”   > “公共终结点”   > “启用”  下为现有 SQL 托管实例启用公共终结点。

   ![启用公共终结点](media/search-howto-connecting-azure-sql-mi-to-azure-search-using-indexers/mi-vnet.png "启用公共终结点")

## <a name="verify-nsg-rules"></a>验证 NSG 规则
检查网络安全组是否具有允许来自 Azure 服务的连接的正确的**入站安全规则**。

   ![NSG 入站安全规则](media/search-howto-connecting-azure-sql-mi-to-azure-search-using-indexers/nsg-rule.png "NSG 入站安全规则")

## <a name="get-public-endpoint-connection-string"></a>获取公共终结点连接字符串
请确保使用**公共终结点**（端口 3342，而不是端口 1433）的连接字符串。

   ![公共终结点连接字符串](media/search-howto-connecting-azure-sql-mi-to-azure-search-using-indexers/mi-connection-string.png "公共终结点连接字符串")

## <a name="next-steps"></a>后续步骤
无需进行配置，现在可以使用门户或 REST API 将 SQL 托管实例指定为 Azure 认知搜索索引器的数据源。 有关详细信息，请参阅[使用索引器将 Azure SQL 数据库连接到 Azure 认知搜索](search-howto-connecting-azure-sql-database-to-azure-search-using-indexers.md)。
