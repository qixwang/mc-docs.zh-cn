---
title: 配置 SSL - Azure Database for MariaDB
description: 介绍了如何正确配置 Azure Database for MariaDB 和关联的应用程序，以正确使用 SSL 连接
author: WenJason
ms.author: v-jay
ms.service: mariadb
ms.topic: conceptual
origin.date: 5/7/2020
ms.date: 06/08/2020
ms.openlocfilehash: 90881504b1eb063a2b0415ac7508c18d21386420
ms.sourcegitcommit: 9811bf312e0d037cb530eb16c8d85238fd276949
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/02/2020
ms.locfileid: "84275611"
---
# <a name="configure-ssl-connectivity-in-your-application-to-securely-connect-to-azure-database-for-mariadb"></a>配置应用程序的 SSL 连接性以安全连接到 Azure Database for MariaDB
Azure Database for MariaDB 支持使用安全套接字层 (SSL) 将 Azure Database for MariaDB 服务器连接到客户端应用程序。 通过在数据库服务器与客户端应用程序之间强制实施 SSL 连接，可以加密服务器与应用程序之间的数据流，有助于防止“中间人”攻击。

## <a name="obtain-ssl-certificate"></a>获取 SSL 证书
从 [https://www.digicert.com/CACerts/DigiCertGlobalRootCA.crt](https://www.digicert.com/CACerts/DigiCertGlobalRootCA.crt) 下载通过 SSL 与 Azure Database for MariaDB 服务器通信所需的证书，再将证书文件保存到本地驱动器（例如，本教程使用 c:\ssl）。

## <a name="download-and-install-openssl"></a>下载并安装 OpenSSL
从[下载页](http://slproweb.com/products/Win32OpenSSL.html)查找并下载最新版本的 OpenSSL。

## <a name="move-the-local-certificate-file-to-the-openssl-directory"></a>将本地证书文件移到 OpenSSL 目录
将步骤 1 中下载的证书放入 **...\OpenSSL-Win32\bin** 目录。

## <a name="convert-the-certificate-file-to-pem-format"></a>将证书文件转换为 PEM 格式
下载的根证书文件采用 crt 格式。 需要使用 openssl.exe 命令行工具执行以下命令来转换文件格式：

```
OpenSSL>x509 -inform DEV -in DigiCertGlobalRootCA.crt -out DigiCertGlobalRootCA.pem
```

## <a name="bind-ssl"></a>绑定 SSL

### <a name="connecting-to-server-using-mysql-workbench-over-ssl"></a>使用 MySQL Workbench 通过 SSL 连接到服务器
配置 MySQL Workbench，以便安全地通过 SSL 连接。 

1. 从“设置新连接”对话框，导航到“SSL”选项卡  。 

1. 将“使用 SSL”  字段更新为“必需”。

1. 在“SSL CA 文件:”  字段中，输入 **DigiCertGlobalRootCA.crt** 的文件位置。 
    
    ![保存 SSL 配置](./media/howto-configure-ssl/mysql-workbench-ssl.png)

对于现有连接，可以通过右键单击“连接”图标并选择“编辑”来绑定 SSL。 然后导航到“SSL”  选项卡，并绑定证书文件。

### <a name="connecting-to-server-using-the-mysql-cli-over-ssl"></a>使用 MySQL CLI 通过 SSL 连接到服务器
绑定 SSL 证书的另一种方法是使用 MySQL 命令行接口执行以下命令。 

```bash
mysql.exe -h mydemoserver.mariadb.database.chinacloudapi.cn -u Username@mydemoserver -p --ssl-mode=REQUIRED --ssl-ca=C:\OpenSSL-Win32\bin\DigiCertGlobalRootCA.pem
```

> [!NOTE]
> 在 Windows 上使用 MySQL 命令行接口时，可能会收到错误 `SSL connection error: Certificate signature check failed`。 如果发生这种情况，请将 `--ssl-mode=REQUIRED --ssl-ca={filepath}` 参数替换为 `--ssl`。

## <a name="enforcing-ssl-connections-in-azure"></a>在 Azure 中强制实施 SSL 连接 
### <a name="using-the-azure-portal"></a>使用 Azure 门户
在 Azure 门户中，访问 Azure Database for MariaDB 服务器，并单击“连接安全性”  。 使用切换按钮来启用或禁用“强制实施 SSL 连接”设置，并单击“保存”   。 Azure 建议你始终启用“强制实施 SSL 连接”设置，以增强安全性  。
![enable-ssl](./media/howto-configure-ssl/enable-ssl.png)

### <a name="using-azure-cli"></a>使用 Azure CLI
可以通过在 Azure CLI 中分别使用“Enabled”或“Disabled”值来启用或禁用“ssl-enforcement”参数  。
```azurecli
az mariadb server update --resource-group myresource --name mydemoserver --ssl-enforcement Enabled
```

## <a name="verify-the-ssl-connection"></a>验证 SSL 连接
执行 mysql status 命令，验证是否已使用 SSL 连接到 MariaDB 服务器： 
```sql
status
```
查看输出以确认连接是否已加密，如果已加密，输出应显示为：“SSL:  使用中的密码为 AES256-SHA” 

## <a name="sample-code"></a>代码示例
若要从应用程序通过 SSL 与 Azure Database for MariaDB 建立安全连接，请参阅以下代码示例：

### <a name="php"></a>PHP
```php
$conn = mysqli_init();
mysqli_ssl_set($conn,NULL,NULL, "C:\OpenSSL-Win32\bin\DigiCertGlobalRootCA.pem", NULL, NULL) ; 
mysqli_real_connect($conn, 'mydemoserver.mariadb.database.chinacloudapi.cn', 'myadmin@mydemoserver', 'yourpassword', 'quickstartdb', 3306, MYSQLI_CLIENT_SSL, MYSQLI_CLIENT_SSL_DONT_VERIFY_SERVER_CERT);
if (mysqli_connect_errno($conn)) {
die('Failed to connect to MySQL: '.mysqli_connect_error());
}
```
### <a name="python-mysqlconnector-python"></a>Python (MySQLConnector Python)
```python
try:
    conn=mysql.connector.connect(user='myadmin@mydemoserver', 
        password='yourpassword', 
        database='quickstartdb', 
        host='mydemoserver.mariadb.database.chinacloudapi.cn', 
        ssl_ca='C:\OpenSSL-Win32\bin\DigiCertGlobalRootCA.pem')
except mysql.connector.Error as err:
    print(err)
```
### <a name="python-pymysql"></a>Python (PyMySQL)
```python
conn = pymysql.connect(user = 'myadmin@mydemoserver', 
        password = 'yourpassword', 
        database = 'quickstartdb', 
        host = 'mydemoserver.mariadb.database.chinacloudapi.cn', 
        ssl = {'ca': 'C:\OpenSSL-Win32\bin\DigiCertGlobalRootCA.pem'}})
```
### <a name="ruby"></a>Ruby
```ruby
client = Mysql2::Client.new(
        :host     => 'mydemoserver.mariadb.database.chinacloudapi.cn', 
        :username => 'myadmin@mydemoserver',      
        :password => 'yourpassword',    
        :database => 'quickstartdb',
        :sslca => 'C:\OpenSSL-Win32\bin\DigiCertGlobalRootCA.pem'
        :ssl_mode => 'required'
    )
```
#### <a name="ruby-on-rails"></a>Ruby on Rails
```ruby
default: &default
  adapter: mysql2
  username: username@mydemoserver
  password: yourpassword
  host: mydemoserver.mariadb.database.chinacloudapi.cn
  sslca: BaltimoreCyberTrustRoot.crt.pem
  sslverify: true
```

### <a name="golang"></a>Golang
```go
rootCertPool := x509.NewCertPool()
pem, _ := ioutil.ReadFile("C:\OpenSSL-Win32\bin\DigiCertGlobalRootCA.pem")
if ok := rootCertPool.AppendCertsFromPEM(pem); !ok {
    log.Fatal("Failed to append PEM.")
}
mysql.RegisterTLSConfig("custom", &tls.Config{RootCAs: rootCertPool})
var connectionString string
connectionString = fmt.Sprintf("%s:%s@tcp(%s:3306)/%s?allowNativePasswords=true&tls=custom",'myadmin@mydemoserver' , 'yourpassword', 'mydemoserver.mariadb.database.chinacloudapi.cn', 'quickstartdb')  
db, _ := sql.Open("mysql", connectionString)
```
### <a name="java-jdbc"></a>Java (JDBC)
```java
# generate truststore and keystore in code
String importCert = " -import "+
    " -alias mysqlServerCACert "+
    " -file " + ssl_ca +
    " -keystore truststore "+
    " -trustcacerts " + 
    " -storepass password -noprompt ";
String genKey = " -genkey -keyalg rsa " +
    " -alias mysqlClientCertificate -keystore keystore " +
    " -storepass password123 -keypass password " + 
    " -dname CN=MS ";
sun.security.tools.keytool.Main.main(importCert.trim().split("\\s+"));
sun.security.tools.keytool.Main.main(genKey.trim().split("\\s+"));

# use the generated keystore and truststore 
System.setProperty("javax.net.ssl.keyStore","path_to_keystore_file");
System.setProperty("javax.net.ssl.keyStorePassword","password");
System.setProperty("javax.net.ssl.trustStore","path_to_truststore_file");
System.setProperty("javax.net.ssl.trustStorePassword","password");

url = String.format("jdbc:mysql://%s/%s?serverTimezone=UTC&useSSL=true", 'mydemoserver.mariadb.database.chinacloudapi.cn', 'quickstartdb');
properties.setProperty("user", 'myadmin@mydemoserver');
properties.setProperty("password", 'yourpassword');
conn = DriverManager.getConnection(url, properties);
```
### <a name="java-mariadb"></a>Java (MariaDB)
```java
# generate truststore and keystore in code
String importCert = " -import "+
    " -alias mysqlServerCACert "+
    " -file " + ssl_ca +
    " -keystore truststore "+
    " -trustcacerts " + 
    " -storepass password -noprompt ";
String genKey = " -genkey -keyalg rsa " +
    " -alias mysqlClientCertificate -keystore keystore " +
    " -storepass password123 -keypass password " + 
    " -dname CN=MS ";
sun.security.tools.keytool.Main.main(importCert.trim().split("\\s+"));
sun.security.tools.keytool.Main.main(genKey.trim().split("\\s+"));

# use the generated keystore and truststore 
System.setProperty("javax.net.ssl.keyStore","path_to_keystore_file");
System.setProperty("javax.net.ssl.keyStorePassword","password");
System.setProperty("javax.net.ssl.trustStore","path_to_truststore_file");
System.setProperty("javax.net.ssl.trustStorePassword","password");

url = String.format("jdbc:mariadb://%s/%s?useSSL=true&trustServerCertificate=true", 'mydemoserver.mariadb.database.chinacloudapi.cn', 'quickstartdb');
properties.setProperty("user", 'myadmin@mydemoserver');
properties.setProperty("password", 'yourpassword');
conn = DriverManager.getConnection(url, properties);
```

### <a name="net-mysqlconnector"></a>.NET (MySqlConnector)
```csharp
var builder = new MySqlConnectionStringBuilder
{
    Server = "mydemoserver.mysql.database.chinacloudapi.cn",
    UserID = "myadmin@mydemoserver",
    Password = "yourpassword",
    Database = "quickstartdb",
    SslMode = MySqlSslMode.VerifyCA,
    CACertificateFile = "C:\OpenSSL-Win32\bin\DigiCertGlobalRootCA.pem",
};
using (var connection = new MySqlConnection(builder.ConnectionString))
{
    connection.Open();
}
```

<!-- 
## Next steps
Review various application connectivity options following [Connection libraries for Azure Database for MariaDB](concepts-connection-libraries.md)
-->
