---
title: TLS - Azure Database for PostgreSQL - 单一服务器
description: 有关如何为 Azure Database for PostgreSQL（单一服务器）配置 TLS 连接的说明和信息。
author: WenJason
ms.author: v-jay
ms.service: postgresql
ms.topic: conceptual
origin.date: 03/10/2020
ms.date: 06/08/2020
ms.openlocfilehash: bab41e6bf1a6c4f3245e9f6bbc3204475852608b
ms.sourcegitcommit: 9811bf312e0d037cb530eb16c8d85238fd276949
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/02/2020
ms.locfileid: "84275603"
---
# <a name="configure-tls-connectivity-in-azure-database-for-postgresql---single-server"></a>在 Azure Database for PostgreSQL（单一服务器）中配置 TLS 连接

Azure Database for PostgreSQL 倾向于使用传输层安全性 (TLS)（以前成为安全套接字层 (SSL)）将客户端应用程序连接到 PostgreSQL 服务。 通过在数据库服务器与客户端应用程序之间强制实施 TLS 连接，可以加密服务器与应用程序之间的数据流，这有助于防止“中间人”攻击。

默认情况下，PostgreSQL 数据库服务配置为需要 TLS 连接。 如果客户端应用程序不支持 TLS 连接，则可以选择禁用 TLS。

## <a name="enforcing-tls-connections"></a>强制实施 TLS 连接

对于通过 Azure 门户或 CLI 预配的所有 Azure Database for PostgreSQL 服务器，默认会强制实施 TLS 连接。 

同样，在 Azure 门户中，用户服务器的“连接字符串”设置中预定义了连接字符串，该字符串中包含以通用语言使用 TLS 连接到数据库服务器所需的参数。 TLS 参数因连接器而异，例如“ssl=true”、“sslmode=require”或“sslmode=required”，以及其他变体。

## <a name="configure-enforcement-of-tls"></a>配置强制实施 TLS

（可选）可以禁用强制实施 TLS 连接。 Azure 建议你始终启用“强制实施 SSL 连接”设置，以增强安全性。

### <a name="using-the-azure-portal"></a>使用 Azure 门户

访问 Azure Database for PostgreSQL 服务器，并单击“连接安全性”。 使用切换按钮来启用或禁用“强制实施 SSL 连接”设置。 然后单击“保存” 。

![连接安全性 - 禁用强制实施 TLS/SSL](./media/concepts-ssl-connection-security/1-disable-ssl.png)

可以通过在“概述”页中查看“SSL 强制实施状态”指示器来确认设置。

### <a name="using-azure-cli"></a>使用 Azure CLI

可以通过在 Azure CLI 中分别使用 `Enabled` 或 `Disabled` 值来启用或禁用“ssl-enforcement”参数。

```azurecli
az postgres server update --resource-group myresourcegroup --name mydemoserver --ssl-enforcement Enabled
```

## <a name="ensure-your-application-or-framework-supports-tls-connections"></a>确保应用程序或框架支持 TLS 连接

某些使用 PostgreSQL 作为其数据库服务的应用程序框架在安装期间默认不启用 TLS。 如果 PostgreSQL 服务器强制实施 TLS 连接，但应用程序未配置 TLS，则应用程序可能无法连接到数据库服务器。 请查阅应用程序文档，了解如何启用 TLS 连接。

## <a name="applications-that-require-certificate-verification-for-tls-connectivity"></a>需要证书验证才可启用 TLS 连接性的应用程序
在某些情况下，应用程序需要具备从受信任的证书颁发机构 (CA) 证书文件 (.cer) 生成的本地证书文件才能实现安全连接。 请参阅以下步骤获取 .cer 文件，解码证书并将其绑定到应用程序。

### <a name="download-the-certificate-file-from-the-certificate-authority-ca"></a>从证书颁发机构 (CA) 下载证书文件 
可在[此处](https://www.digicert.com/CACerts/DigiCertGlobalRootCA.crt)找到通过 SSL 与 Azure Database for PostgreSQL 服务器通信所需的证书。 本地下载证书文件。

### <a name="install-a-cert-decoder-on-your-machine"></a>在计算机上安装证书解码器 
可以使用 [OpenSSL](https://github.com/openssl/openssl) 来解码应用程序安全连接到数据库服务器所需的证书文件。 若要了解如何安装 OpenSSL，请参阅 [OpenSSL 安装说明](https://github.com/openssl/openssl/blob/master/INSTALL)。 


### <a name="decode-your-certificate-file"></a>解码证书文件
下载的根 CA 文件采用加密格式。 使用 OpenSSL 解码证书文件。 要执行此操作，请运行此 OpenSSL 命令：

```
openssl x509 -inform DER -in DigiCertGlobalRootCA.crt -text -out root.crt
```

### <a name="connect-using-psql"></a>使用 psql 进行连接

以下示例演示如何使用 psql 命令行实用程序连接到 PostgreSQL 服务器。 使用 `sslmode=verify-full` 连接字符串设置强制实施 TLS/SSL 证书验证。 将本地证书文件路径传递给 `sslrootcert` 参数。

以下命令是 psql 连接字符串的示例：

```shell
psql "sslmode=verify-full sslrootcert=root.crt host=mydemoserver.postgres.database.chinacloudapi.cn dbname=postgres user=myusern@mydemoserver"
```

> [!TIP]
> 确认传递给 `sslrootcert` 的值与你保存的证书的文件路径匹配。

## <a name="next-steps"></a>后续步骤

在 [Azure Database for PostgreSQL 的连接库](concepts-connection-libraries.md)中查看各种应用程序连接选项。
