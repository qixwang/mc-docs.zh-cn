---
title: SSL/TLS 连接 - Azure Database for MySQL
description: 有关配置 Azure Database for MySQL 和关联应用程序以正确使用 SSL 连接的信息
author: WenJason
ms.author: v-jay
ms.service: mysql
ms.topic: conceptual
origin.date: 07/09/2020
ms.date: 08/17/2020
ms.openlocfilehash: bbcfae3df383e33ed3461ce43b75127c2e9e6acb
ms.sourcegitcommit: 3cf647177c22b24f76236c57cae19482ead6a283
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/10/2020
ms.locfileid: "88029694"
---
# <a name="ssltls-connectivity-in-azure-database-for-mysql"></a>Azure Database for MySQL 中的 SSL/TLS 连接

Azure Database for MySQL 支持使用安全套接字层 (SSL) 将数据库服务器连接到客户端应用程序。 通过在数据库服务器与客户端应用程序之间强制实施 SSL 连接，可以加密服务器与应用程序之间的数据流，有助于防止“中间人”攻击。

> [!NOTE]
> 将要查看的是 Azure Database for MySQL 的新服务。 若要查看经典 MySQL Database for Azure 的文档，请访问[此页](https://docs.azure.cn/zh-cn/mysql-database-on-azure/)。

> [!NOTE]
> 更新 `require_secure_transport` 服务器参数值不会影响 MySQL 服务的行为。 使用本文中概述的 SSL 和 TLS 增强功能来保护与数据库的连接。

## <a name="ssl-default-settings"></a>SSL 默认设置

默认情况下，应将数据库服务配置为需要 SSL 连接才可连接到 MySQL。  建议尽量不要禁用 SSL 选项。

通过 Azure 门户和 CLI 预配新的 Azure Database for MySQL 服务器时，默认情况下会强制实施 SSL 连接。 

Azure 门户中显示了各种编程语言的连接字符串。 这些连接字符串包含连接到数据库所需的 SSL 参数。 在 Azure 门户中，选择服务器。 在“设置”标题下，选择“连接字符串” 。 SSL 参数因连接器而异，例如“ssl=true”、“sslmode=require”或“sslmode=required”，以及其他变体。

在某些情况下，应用程序需要具备从受信任的证书颁发机构 (CA) 证书文件生成的本地证书文件才能实现安全连接。 目前，客户只能使用预定义的证书来连接位于 [Azure 中国](https://dl.cacerts.digicert.com/DigiCertGlobalRootCA.crt.pem)的 Azure Database For MySQL 服务器。 

若要了解如何在开发应用程序期间启用或禁用 SSL 连接，请参阅[如何配置 SSL](howto-configure-ssl.md)。

## <a name="tls-enforcement-in-azure-database-for-mysql"></a>Azure Database for MySQL 中的 TLS 强制

对于使用传输层安全性 (TLS) 连接到数据库服务器的客户端，Azure Database for MySQL 支持加密。 TLS 是一种行业标准协议，可确保在数据库服务器与客户端应用程序之间实现安全的网络连接，使你能够满足合规性要求。

### <a name="tls-settings"></a>TLS 设置

Azure Database for MySQL 提供了为客户端连接强制使用 TLS 版本的功能。 若要强制使用 TLS 版本，请使用“最低 TLS 版本”选项设置。 此选项设置允许以下值：

|  最低 TLS 设置             | 支持的客户端 TLS 版本                |
|:---------------------------------|-------------------------------------:|
| TLSEnforcementDisabled（默认值） | 不需要 TLS                      |
| TLS1_0                           | TLS 1.0、TLS 1.1、TLS 1.2 及更高版本           |
| TLS1_1                           | TLS 1.1、TLS 1.2 及更高版本                   |
| TLS1_2                           | TLS 版本 1.2 及更高版本                     |


例如，将此最低 TLS 设置版本的值设置为 TLS 1.0 意味着服务器将允许那些使用 TLS 1.0、1.1 和 1.2+ 的客户端进行连接。 也可将此选项设置为 1.2，这意味着仅允许那些使用 TLS 1.2+ 的客户端进行连接，将拒绝使用 TLS 1.0 和 TLS 1.1 进行的所有连接。

> [!Note] 
> 默认情况下，Azure Database for MySQL 不强制执行最低 TLS 版本要求（设置为 `TLSEnforcementDisabled`）。
>
> 一旦强制实施最低 TLS 版本要求后，以后将无法禁用最低版本强制实施。

若要了解如何为 Azure Database for MySQL 设置 TLS 设置，请参阅 [如何配置 TLS 设置](howto-tls-configurations.md)。

## <a name="next-steps"></a>后续步骤

- [Azure Database for MySQL 的连接库](concepts-connection-libraries.md)
- 了解如何[配置 SSL](howto-configure-ssl.md)
- 了解如何[配置 TLS](howto-tls-configurations.md)
